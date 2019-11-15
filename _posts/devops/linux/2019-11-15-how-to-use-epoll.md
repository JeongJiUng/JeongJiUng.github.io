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
