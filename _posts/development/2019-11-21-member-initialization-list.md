---
date: 2019-11-21 22:00:00
layout: post
title: 초기화 리스트 
subtitle: 생성 시 초기화, 생성 후 초기화
description: >-
  초기화 리스트와 대입 방식의 차이
category: development
tags:
  - development
  - class
  - c++
author: Stop_Male
paginate: true
---

### 생성 후 초기화 (대입)

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

### 생성 시 초기화 (초기화 리스트)

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

### 초기화 리스트를 반드시 사용해야 하는 상황

* 상수 멤버 변수 초기화
* 레퍼런스 멤버 변수 초기화

```c++
class cTest
{
public:
	cTest(int _a) : a(_a), b(a), c(a)
	{

	}
	
	virtual ~cTest()
	{

	}

public:
	int a;
	int &b;
	const int c;
};
```

* 클래스 내부에 선언된 인스턴스 또는 부모 클래스에 기본 생성자가 없을 때

```c++
class cTest
{
public:
	cTest(int _a) : a(_a), b(a), c(a)
	{

	}
	
	virtual ~cTest()
	{

	}

public:
	int a;
	int &b;
	const int c;
};

class cParent
{
public:
	cParent(int _a) : a(_a)
	{

	}

private:
	int a;
};

class cTest2 : public cParent
{
public:
	cTest2() : A(10), cParent(10)
	{
	}

	~cTest2()
	{

	}

private:
	cTest A;
};
```

