---
data: 2019-11-15 18:35:00
layout: post
title: Epoll의 기초 개념 및 사용 방법
subtitle: Linux I/O 통지모델
description: >-
  Linux 10K를 위한 I/O 통지모델
image: >-
  https://wallpaperstream.com/wallpapers/full/linux-tux-logos/Linux-Tux-Logo-HD-Wallpaper.jpg
optimized_image: >-
  https://wallpaperstream.com/wallpapers/full/linux-tux-logos/Linux-Tux-Logo-HD-Wallpaper.jpg
category: linux
tags:
  - linux
  - server
  - I/O
author: Stop_Male
paginate: true
---
## Epoll?
Epoll은 리눅스에서 select의 단점을 보완하여 사용할 수 있도록 만든 I/O통지 모델이다. 파일 디스크립터를 사용자가 아닌 커널이 관리를 하며, 그만큼 CPU는 계속해서 파일 디스크립터의 상태 변화를 감시할 필요가 없다. 즉, select처럼 어느 파일 디스크립터에 이벤트가 발생하였는지 찾기 위해 전체 파일디스크립터에 대해서 순차검색을 위한 FD_ISSET 루프를 돌려야 하지만, Epoll의 경우 이벤트가 발생한 파일 디스크립터들만 구조체 배열을 통해 넘겨주므로 메모리 카피에 대한 비용이 줄어든다.

## Epoll 함수

```c++
#include <sys/epoll.h>

int epoll_create(int size)
```
fd들의 입출력 이벤트 저장을 위한 공간을 만들어야 하는데, epoll_create는 size만큼의 입출력 이벤트를 저장할 공간을 만든다. 그러나 리눅스 2.6.8 이후부터 size 인자는 사용되지 않지만 0보다는 큰 값으로 설정을 해 주어야 한다. 커널은 필요한 데이터 구조의 크기를 동적으로 조정하기 때문에 0보다 큰 값만 입력하면 된다.

반환 값으로는 정수형 데이터가 반환이 되는데, 이를 일반적으로 epoll fd라하며 이 fd를 통해 앞으로 epoll에 등록 된 fd들을 조작하게 된다. 

```c++
#include <sys/epoll.h>

int epoll_ctl(int epfd, int op, int fd, struct epoll_event* event)
```
epoll_ctl은 epoll에 fd들을 등록/수정/삭제를 하는 함수인데 일반적으로 epoll이 관심을 가져주길 바라는 fd와 그 fd에서 발생하는 관심있는 사건의 종류를 등록하는 인터페이스로 설명된다.

* epfd : epoll fd 값
* op : 관심가질 fd를 등록할지, 등록되어 있는 fd의 설정을 변경할지, 등록되어 있는 fd를 관심 목록에서 제거할지에 대한 옵션값
<table>
  <tfoot>
    <tr>
      <td>EPOLL_CTL_ADDD</td>
      <td>fd를 epfd의 관심 목록에 추가, 이미 목록에 존재한다면 EEXIST에러를 발생 시킨다. event 집합은 *event에 저장된다.</td>
    </tr>
    <tr>
      <td>EPOLL_CTL_MODD</td>
      <td>*event에 저장된 정보를 이용해 fd설정 변경. 관심 목록에 없는 fd라면 ENOENT에러를 발생시킨다.</td>
    </tr>
    <tr>
      <td>EPOLL_CTL_DEL</td>
      <td>epfd에서 fd를 제공한다. epfd관심 목록에 없는 fd를 제거하려면 ENOENT 에러를 발생한다. fd를 닫으면 epoll관심 목록에서 자동 제거된다.</td>
    </tr>
  </tfoot>
</table>
* fd : epfd에 등록할 관심있는 파일 디스크립터 값
* event : epfd에 등록할 관심있는 fd가 어떤 이벤트가 발생할 때 관심을 가질지에 대한 구조체. 관찰 대상의 관찰 이벤트 유형

```c++
typedef union epoll_data
{
    void *ptr;
    int fd;
    __uint32_t u32;
    __uint64_t u64;
} epoll_data_t

struct epoll_event 
{
    __uint32_t events;  /* Epoll events */
    epoll_data_t data;  /* User data variable */
}
```

<table>
  <thead>
    <tr>
      <th>Events</th>
    </tr>
  </thead>
  <tfoot>
    <tr>
      <td>EPOLLIN</td>
      <td>수신할 데이터가 있다</td>
    </tr>
    <tr>
      <td>EPOLLOUT</td>
      <td>송신 가능하다</td>
    </tr>
    <tr>
      <td>EPOLLPRI</td>
      <td>중요한 데이터(OOB) 발생</td>
    </tr>
    <tr>
      <td>EPOLLRDHUD</td>
      <td>연결 종료 또는 Half-close 발생</td>
    </tr>
    <tr>
      <td>EPOLLERR</td>
      <td>에러 발생</td>
    </tr>
    <tr>
      <td>EPOLLET</td>
      <td>엣지 트리거 방식으로 설정</td>
    </tr>
    <tr>
      <td>EPOLLONESHOT</td>
      <td>한번만 이벤트 받음</td>
    </tr>
  </tfoot>
</table>

```c++
#include <sys/epoll.h>
int epoll_wait(int epfd, struct epoll_event* events, int maxevents, int timeout)
```
epoll_wait는 관심있는 fd들에 무슨일이 일어났는지 조사한다. 다만 그 결과는 select나 poll과는 차이가 있다. 사건들의 리스트를 (epoll_event).events[] 의 배열로 전달한다. 리턴값은 발생한 사건들의 갯수가 리턴된다.

* events : 이벤트가 발생된 fd들을 모아놓은 구조체 배열.
* maxevents : 실제 동시접속수와 상관없이 maxevents 파라미터로 최대 몇개까지의 event만 처리할 것임을 지정해 주도록 하고 있다. 만약 현재 접속수가 1만이라면 최악의 경우 1만개의 연결에서 사건이 발생할 가능성도 있기 때문에 1만개의 events[] 배열을 위해 메모리를 확보해 놓아야 하지만, 이 maxevents 파라미터를 통해 한번에 처리하길 희망하는 숫자를 제한할 수 있다.
* timeout : epoll_wait의 동작특성을 지정해주는 중요한 요소인데, 밀리세컨드 단위로 지정해주도록 되어 있다. 이 시간만큼 사건발생을 기다리라는 의미이며 기다리는 도중에 사건이 발생하면 즉시 리턴된다.
  - timeout(-1)로 지정해주면 영원히 사건을 기다리는 blocking상태가 된다.
  - timeout(0)로 지정해주면 사건이 있건 없건 조사만 하고 즉시 리턴하는 상태가 된다.
> 간단한 채팅서버의 경우를 살펴보자. 서버가 어떠한 일을 해야하는 시점은 이용자 누군가가 데이터를 보내왔을 때인데, 아무도 아무말도 하지 않는다면 서버는 굳이 프로세싱을 할 이유가 없다. 이럴때 timeout을 (-1)로 지정해두고 이용자들의 입력이 없는 동안 운영체제에 프로세싱 타임을 넘기도록 한다. 
> 
> 온라인게임(특히 MMORPG)의 경우에는, 이용자의 입력이 전혀 없는 도중이라고 하더라도, 몬스터에 관련된 처리, 적절한 저장, 다른 서버와의 통신들을 해야 하므로 적절한 timeout (필자의 경우에는 1/100 sec, 즉 10ms를 선호한다)을 지정해 주도록 한다. 
> 
> 뭔가의 프로세싱을 주로 하면서 잠깐잠깐 통신이벤트를 처리하고자 하는 경우, 즉 프로세스의 CPU 점유를 높게해서 무언가를 하고 싶은 경우에는, timeout 0을 설정하여 CPU를 독점하도록 설계할 수도 있다. 
>
> 별도 thread를 구성하여 이 thread 가 입출력을 전담하도록 프로그램을 작성하고자 하는 경우에는, 당연히 timeout을 (-1)로 설정하여 남는 시간을 다른 thread, 혹은 운영체제에 돌려 주도록 한다.
>
> [link](http://biscuit.cafe24.com/moniwiki/wiki.php/epoll#s-4)


## Edge Trigger & Level Trigger
> **Edge Trigger**
>
> 특정 상태가 변화하는 시점에서만 감지.
>
> 특정 디지털 신호 000 111 000 111 000 111 일 경우 신호가 0에서 1로 변하는 시점에서만 이벤트가 발생한다.
> 
> 소켓 버퍼에 대응하면, 한번에 읽을 수 있는 데이터 버퍼가 600인데, 데이터가 1000바이트가 도착했다면, 600바이트만 읽고 나머지 400바이트는 읽지 않은 상태에서 더 이상의 이벤트는 발생하지 않는다.
>
> 읽은 시점을 기준으로 보면 더이상의 상태 변화가 없기 때문이다.
>
> 따라서 한번에 읽을 수 있는 바이트 이상의 데이터가 오게 된다면 추가적인 작업을 따로 해주어야 한다.
