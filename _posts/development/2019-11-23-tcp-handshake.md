---
date: 2019-11-23 01:14:00
layout: post
title: 3 Way-Handshake & 4 Way-Handshake
subtitle: TCP의 연결과 연결 종료
description: >-
  TCP 프로토콜의 연결 과정과 연결 종료 과정.
category: development
tags:
  - development
  - tcp
  - 3 way-handshake
  - 4 way-handshake
  - server
  - network
author: Stop_Male
paginate: true
---

## 3-Way Handshake

- TCP 연결을 초기화 할 때 사용.
- SYN : Synchronization, 동기화, 연결 요청 플래그.
- ACK : Acknowledgement, 응답 플래그.

![image](https://github.com/JeongJiUng/jeongjiung.github.io/blob/master/_images/tcp-handshake-diagram.png?raw=true)

1. 클라이언트는 서버에 접속을 요청하는 SYN 패킷을 전송. 클라이언트의 상태는 SYN_SENT가 된다.
   * 서버 연결 해도 되니?
2. 서버는 SYN 요청을 받고 클라이언트에게 요청을 수락한다는 ACK와 SYN flag가 설정된 패킷 발송. 클라이언트가 다시 ACK로 응답하기를 기다린다. 서버는 SYN_RECEIVED가 된다.
   * 되는데 잠깐만, 연결준비 하고... 연결 준비 됬어!
3. 클라이언트는 서버에게 ACK를 보내고 이후로부터는 연결이 이루어지고 데이터가 오가게 된다. 이때 서버의 상태가는 ESTABLSIHED다.
   * 연결 한다!

## 4-Way Handshake

- TCP 연결을 종료할 때
- Initiator를 클라이언트, Receiver를 서버로 가정.

![image](https://github.com/JeongJiUng/jeongjiung.github.io/blob/master/_images/1280px-TCP_CLOSE.svg.png?raw=true)

1. 클라이언트에서는 세션 종료를 시작한다는 의미(shutdown)로 서버에 FIN 패킷을 전송하고 FIN_WAIT_1 상태가 된다.
2. 서버에서는 FIN 패킷을 받고 잘 받았다는 의미로 ACK 패킷을 클라이언트로 보낸 후 CLOSW_WAIT 상태로 돌입, 클라이언트는 FIN_WAIT_2 상태가 된다.
3. 클라이언트로부터 FIN 패킷을 받게 된 서버는 종료에 필요한 작업을 실행하게 되며, 종료할 준비가 완료 되면 클라이언트에 FIN 패킷을 보낸다. 서버는 LAST_ACK 상태가 된다.
4. 클라이언트는 서버로부터 FIN 패킷을 받게 되면 ACK를 서버로 보내게 되고, 자신은 TIME_WIAT 상태로 변경된다. 일정 시간이 지나면 TIME_WAIT에서 CLOSED가 되고, 최종 ACK를 받은 서버도 CLOSED 상태가 된다.