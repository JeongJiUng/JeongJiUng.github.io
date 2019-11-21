---
date: 2019-11-21 22:00:00
layout: post
title: 초기화 리스트 
subtitle: 생성 시 초기화, 생성 후 초기화
description: >-
  생성자의 다양한 멤버변수 초기화 방식.
category: development
tags:
  - development
  - class
  - c++
author: Stop_Male
paginate: true
---

### 생성 후 초기화

```c++
class cTemp
{
public:
    cTemp(int _A, int _B)
    {
        m_A = _A;
        m_B = _B;
    }

    ~cTemp()
    {

    }

private:
    int m_A;
    int m_B;
}
```

### 생성 시 초기화

```c++


class cTemp
{
public:
    cTemp(int _A, int _B) : m_A(_A), m_B(_B)
    {
        ...
    }

    ~cTemp()
    {

    }

private:
    int m_A;
    int m_B;
}
```

### 차이

결론부터 말하면, 생성 후 초기화보다 생성 시 초기화가 효율이 좋다.

생성 시 초기화는 한 흐름에 모든 것이 끝나고, 생성 후 초기화는 생성과 초기화 2가지 흐름으로 처리되기 때문이다.