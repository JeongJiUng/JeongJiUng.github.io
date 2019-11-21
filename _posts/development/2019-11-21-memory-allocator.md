---
date: 2019-11-21 23:10:00
layout: post
title: 메모리 할당자
subtitle: 메모리 할당자들의 특징
description: >-
  new, malloc, callco, realloc
category: development
tags:
  - development
  - class
  - c++
author: Stop_Male
paginate: true
---

## malloc

```c++
void* malloc(unsigned int);
```

* memory allocation

* 할당된 공간의 값들은 바꾸지 않는다.

  * 만약 A 유저에게 메모리 할당을 하고, 데이터를 입력한 다음, 메모리 해제를 했다고 가정을 했을 때, B 유저에게 메모리 할당을 했을 경우 확률로 A 유저가 사용한 메모리의 주소를 받게 될 것이고, 그 주소에 있던 값들은 초기화되지 않았기 때문에 A 유저의 데이터를 사용할 수 있는 불상사가 발생할 수 있다.

* 리턴값은 할당된 주소.

* ``` c++
  int *ar;
  int size = 5;
  ar = (int *)malloc(size*sizeof(int));  
  for (int i = 0; i < size; ++i)
      ar[i] = i;
  for (int i = 0; i<size; ++i)
      printf("%d", ar[i]);
  free(ar);
  ```

## calloc

```c++
void* calloc(unsigned int, unsigned int);
```

* clear allocation

* 첫번째 인자는 배열요소의 갯수, 두번째 인자는 할당받을 공간의 크기

* 할당된 공간의 값들을 0으로 초기화

* 리턴값은 할당된 주소, 실패시 NULL을 리턴.

  * NULL값을 리턴하게 되면, 기존 포인터에 할당되어 있던 주소값이 NULL로 대체되어 찾을 수 없기 때문에 이에대한 보완 대책이 필요함.

* ```c++
  int *ar;
  int size = 5;
  ar = (int *)calloc(size,sizeof(int));
  //  ar = (int *)calloc(1, size*sizeof(int));
  for (int i = 0; i<size; ++i)
      printf("%d", ar[i]);
  free(ar);
  ```

## realloc

```c++
void* realloc(void*, size_t size);
```

* resize allocation

* 동적 메모리의 재할당(주소를 새롭게 다시 잡음).

* 리턴값은 재할당된 주소(재변경시 메모리공간의 문제로 다른 위치에 변경될 수 있다.).

  * 실패시 NULL값을 리턴하기 때문에, 기존 포인터에 할당되어 있던 주소값이 NULL로 대체되어 찾을 수 없기 떄문에 이에대한 보완 대책이 필요함.

* ```c++
  int *ar;
  int size = 5;
  int resize = 10;
  ar = (int *)malloc(size*sizeof(int));
  ar = (int *)realloc(ar, resize*sizeof(int));
  for (int i = 0; i < resize; ++i)
      ar[i] = i;
  for (int i = 0; i<resize; ++i)
      printf("%d", ar[i]);
  free(ar);
  ```

## 함수 내에서 realloc 또는 malloc 사용시 주의점

```c++
void func(char* arr)
{
	arr = (char*)realloc(arr, 10 * sizeof(char));
	arr[5] = 'f';
	arr[6] = 'g';
	arr[7] = '\0';
}

void func2(char* arr)
{
	arr = (char*)malloc(6 * sizeof(char));
	arr[0] = 'a';
	arr[1] = 'b';
	arr[2] = '\0';
}

int main()
{
	char *arr = (char*)malloc(6 * sizeof(char));
	arr[0] = 'a';
	arr[1] = 'b';
	arr[2] = 'c';
	arr[3] = 'd';
	arr[4] = 'e';
	arr[5] = '\0';

	func(arr);
	func2(arr);

	cout << arr << endl;

	return 0;
}
```

위와 같이 단일 포인터를 파라메터로 전달 시 함수 내에서 현재 메모리 주소를 기준으로 메모리 크기를 늘렸을 때 공간 부족과 같은 메모리 사정으로 새롭게 메모리 주소가 할당이 되면 문제(메모리 누수)가 생길 수 있다.

그렇기 때문에 아래와 같이 2중 포인터를 이용하여 접근해야 한다. 함수 내부에서 새롭게 메모리 할당을 통해 새로운 메모리 주소를 얻게 되면, 그 값은 함수의 스택 영역에 저장되기 때문이다.

```c++
void func(char** arr)
{
	*arr = (char*)realloc(*arr, 10 * sizeof(char));
	arr[0][5] = 'f';
	arr[0][6] = 'g';
	arr[0][7] = '\0';
}

void func2(char** arr)
{
	*arr = (char*)malloc(6 * sizeof(char));
	arr[0][0] = 'a';
	arr[0][1] = 'b';
	arr[0][2] = '\0';
}

int main()
{
	char *arr = (char*)malloc(6 * sizeof(char));
	arr[0] = 'a';
	arr[1] = 'b';
	arr[2] = 'c';
	arr[3] = 'd';
	arr[4] = 'e';
	arr[5] = '\0';

	func(&arr);

	cout << arr << endl;

	return 0;
}
```

## new / delete

### 특징 

* malloc / free는 라이브러리가 제공하는 함수인데 비해 new / delete는 언어가 제공하는 연산자이다. 따라서 별도의 헤더 파일을 포함할 필요없이 언제든지 사용할 수 있으며 이 연산자를 쓴다고 해서 프로그램이 커지는 것도 아니다.

* 연산자이기 때문에 사용자 정의 타입에 대해 오버로딩할 수도 있다.

* malloc / free는 필요한 메모리양을 바이트 단위로 지정하고 void*를 리턴하므로 sizeof 연산자와 타입 캐스트 연산자의 도움을 받아야 한다. 이에 비해 new는 할당할 타입을 지정하고 해당 타입의 포인터를 리턴하므로 sizeof 연산자와 타입 캐스트 연산자를 쓸 필요가 없다.

* malloc은 메모리 할당이 목적이므로 초기값을 줄 수 없지만, new는 동적으로 생성한 변수의 초기값을 지정할 수 있다.(calloc은 0으로만 초기화 한다.)

  * ```c++
    int* a = new int(10);
    int* b = new int[3]{ 1, 2, 3 };
    char* c = new char[14]{ "hello" };
    ```

* new 연산자로 객체를 할당할 때 생성자가 자동으로 호출된다. 마찬가지로 delete로 객체를 삭제할 때는 파괴자가 자동으로 호출된다.