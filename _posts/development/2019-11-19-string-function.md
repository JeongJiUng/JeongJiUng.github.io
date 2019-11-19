---
date: 2019-11-19 17:02:05
layout: post
title: 문자열 처리 함수
subtitle: 문자열 처리 함수들
description: >-
  문자열 처리를 위한 다양한 함수. strstr, strcat, strcmp 등
image: >-
  https://upload.wikimedia.org/wikipedia/commons/thumb/1/18/ISO_C%2B%2B_Logo.svg/1200px-ISO_C%2B%2B_Logo.svg.png
optimized_image: >-
  https://upload.wikimedia.org/wikipedia/commons/thumb/1/18/ISO_C%2B%2B_Logo.svg/1200px-ISO_C%2B%2B_Logo.svg.png
category: development
tags:
  - development
  - 문자열
  - c
  - c++
author: Stop_Male
paginate: true
---

### strstr (대상 문자열, 검색할 문자열)

```c++
char* strstr(char * const_string, const char* const_substring)
```

* 문자열 안에서 문자열 검색.
* 문자열을 찾았으면 문자열로 시작하는 문자열의 포인터를 반환, 문자열이 없으면 NULL 반환.
* 대소문자 구분.

### strchar(대상 문자열, 검색할 문자)

```c++
char* strchr(char* const_string, int const_ch)
```

* 문자열 안에서 문자 검색.
* 문자열의 왼쪽부터 문자 검색 시작.
* 문자를 찾았으면 문자로 시작하는 문자열의 포인터 반환, 문자가 없으면 NULL을 반환.
* 대소문자 구분.

### strrchar(대상 문자열, 검색할 문자)

```c++
char* strrchr(const char* _string, const int _ch)
```

* 문자열 안에서 문자 검색.
* 문자열의 오른쪽 끝부터 문자 검색 시작.
* 문자열의 끝에서부터 역순으로 검색해서 문자를 찾았으면 해당 문자로 시작하는 문자열의 포인터를 반환, 문자가 없으면 NULL 반환.
* 대소문자 구분.

### sprintf(배열, 서식, 값)

```c++
int sprintf(const char* _buffer, const char* _format, ...)
```

* _buffer에 _format 형식으로 문자열 저장.

### strtok(대상 문자열, 기준 문자)

```c++
char* strtok(char* _string, const char* _delimiter)
```

* 특정 문자를 기준으로 문자열 자르기.
* 특정 문자(기준 문자) _delimiter는 한 번에 여러 개를 지정할 수 있다.
  * "-T:" : -, T, 콜론을 기준으로 문자열 자름.
* 자른 문자열을 반환, 더 이상 자를 문자열이 없으면 NULL을 반환.

```c++
int main()
{
    char s1[30] = "The Littel Prince";
    char* ptr = strtok(s1, " ");
    
    while (ptr != NULL)
    {
        printf("%s\n", ptr);
        ptr = strtok(NULL, " ");
    }
    
    return 0;
}
```

```c++
/*** 실행 결과. ***/
The
Littel
Prince
```

* ptr = strtok(NULL, " ")처럼 대상 문자열 부분에 NULL을 넣어주면, 직전 strtok 함수에서 처리했던 문자열에서 잘린 문자열 만큼 다음 문자로 이동한 뒤 다음 문자열을 자른다. 만약 ptr = strtok(ptr, " ") 처럼 잘린 문자열의 포인터를 다시 넣었을 때는 다음 문자로 이동하지 못하고 처음에 나오는 문자열만 계속 자르게 된다. 즉, 다음과 같은 결과가 나온다.

```c++
/*** 실행 결과. ***/
The
The
The
...
```

* strtok 함수는 문자열을 새로 생성해서 반환하는 것이 아니라 자르는 부분을 널 문자(NULL)로 채운 뒤 잘린 문자열의 포인터를 반환한다. 따라서 원본 문자열의 내용을 바꾸므로 사용에 주의해야 한다.

![placeholder](https://github.com/JeongJiUng/jeongjiung.github.io/blob/master/_images/unit45-1.png?raw=true)

![placeholder](https://github.com/JeongJiUng/jeongjiung.github.io/blob/master/_images/unit45-2.png?raw=true)

![placehoder](https://github.com/JeongJiUng/jeongjiung.github.io/blob/master/_images/unit45-3.png?raw=true)

![placeholder](https://github.com/JeongJiUng/jeongjiung.github.io/blob/master/_images/unit45-4.png?raw=true)

### strcmp(문자열1, 문자열2), strncmp(문자열1, 문자열2, 처음 부터 비교할 최대 문자의 개수)

```c++
int strcmp(const char* _str1, const char* _str2)
int strncmp(const char* _str1, const char* _str2, int n)
```

* 대소문자 구분.
* return value
  * -1 : ASCII 코드 기준으로 문자열2가 클 때.
  * 0 : ASCII 코드 기준으로 두 문자열이 같을 때.
  * 1 : ASCII 코드 기준으로 문자열1이 클 때.
* 리눅스 OS는 return value가 ASCII 코드 값의 차이로 반환된다.

### strcpy(복사될 문자열, 원본 문자열), strncpy(복사될 문자열, 원본 문자열, 크기)

```c++
char* strcpy(char* dest, const char* origin)
char* strncpy(char* dest, const char* origin, size_t n)
```

* 복사된 결과가 저장될 배열의 크기는 반드시 NULL까지 들어갈 수 있어야 한다. 크기가 더 작다면 복사가 되더라도 문자열이 정상적으로 출력되지 않는다(버퍼 오버런 발생 가능성 존재). 따라서 "hello" 문자열이 복사되려면 배열의 크기는 최소한 6이 되어야 한다.

* strncpy는 origin에 있는 문자열을 dest로 복사 하는데, n 만큼 복사한다.

  * n <= sizeof(origin)
  * n <= sizeof(dest)

* 문자열 끝('\0', NULL 문자) 까지 복사를 한다. 따라서, 다음과 같은 코드의 경우 복사 결과는 다음과 같다.

* ```c++
  char origin[] = "123";
  char dest1[] = "12345";
  strcpy(dest1, origin);
  
  /** 메모리 결과 **/
  dest1 : 1 2 3 \0 4 5 \0;
  /** 출력 결과 **/
  123
  ```

* 출력 함수는 문자열의 NULL 문자까지만 출력하기 때문에 123만 출력이 된다.

* 만약, "12345" 이런식으로 문자열을 연결하고 싶다면, origin의 마지막 널문자만 지워주면 되므로 다음과같이 코드를 수정하면 된다.

* ```c++
  char origin[] = "123";
  char dest1[] = "12345";
  strncpy(dest1, origin, sizeof(origin) - 1);
  
  /** 메모리 결과 **/
  dest1 : 1 2 3 4 5 \0;
  /** 출력 결과 **/
  12345
  ```

### strcat(복사될 문자열, 원본 문자열), strncat(복사될 문자열, 원본 문자열, 크기)

```c++
char* strcat(char* dest, const char* origin)
char* strncat(char* dest, const char* origin, size_t n)
```

* origin에 있는 문자열을 dest 뒤쪽에 이어 붙이는 함수.
* dest 문자열 끝에 있는 '\0'은 사라지고 그 위치에 origin이 바로 붙어버리는게 특징.
* strncat의 경우, origin에 있는 문자열 n개를 dest 뒤쪽에 이어 붙이는 함수.
* dset의 길이는 origin과 합쳐도 남을 정도로 충분히 길어야 한다.
* 마지막에 붙여 넣은 문자열 끝에만 '\0'이 붙으며, strncpy를 사용하여 origin에서 n번째 문자까지만 잘라 넣어서 합친 문자열 끝에도 '\0'가 붙는다는 사실을 기억해야 한다.