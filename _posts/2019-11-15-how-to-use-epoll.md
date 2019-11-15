---
data: 2019-11-15 18:35:00
layout: post
title: Epoll의 기초 개념 및 사용 방법
subtitle: Linux I/O 통지모델
description: >-
  Linux 10K를 위한 I/O 통지모델
category: Linux
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
> #include <sys/epoll.h>

> int epoll_create(int size)
