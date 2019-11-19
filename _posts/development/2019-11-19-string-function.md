---
date: 2019-11-19 17:02:05
layout: post
title: String 함수들
subtitle: 문자열 처리 함수
description: >-
  문자열 처리를 위한 다양한 함수. strstr, strcat, strcmp 등
image: >-
  https://upload.wikimedia.org/wikipedia/commons/thumb/1/18/ISO_C%2B%2B_Logo.svg/1200px-ISO_C%2B%2B_Logo.svg.png
optimized_image: >-
  https://upload.wikimedia.org/wikipedia/commons/thumb/1/18/ISO_C%2B%2B_Logo.svg/1200px-ISO_C%2B%2B_Logo.svg.png
category: development
tags:
  - development
  - string
  - c
  - c++
author: Stop_Male
paginate: true
---

### strstr (대상 문자열, 검색할 문자열)

```c++
char* strstr(char * const_string, char const* const_substring)
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
char* strrchr(char* const_string, int const_ch)
```

* 문자열 안에서 문자 검색.
* 문자열의 오른쪽 끝부터 문자 검색 시작.
* 문자열의 끝에서부터 역순으로 검색해서 문자를 찾았으면 해당 문자로 시작하는 문자열의 포인터를 반환, 문자가 없으면 NULL 반환.
* 대소문자 구분.

### sprintf(배열, 서식, 값)

```c++
int sprintf(char* const_buffer, char const* const_format, ...)
```

### strtok(대상 문자열, 기준 문자)

```c++
char* strtok(char* _string, char const* _delimiter)
```

* 특정 문자를 기준으로 문자열 자르기.
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

![placeholder](C:\Users\WorkBench\CloudStation\github_blog\jeongjiung.github.io\_images\unit45-1.png)

![placeholder](C:\Users\WorkBench\CloudStation\github_blog\jeongjiung.github.io\_images\unit45-2.png)

![placehoder](C:\Users\WorkBench\CloudStation\github_blog\jeongjiung.github.io\_images\unit45-3.png)

![placeholder](C:\Users\WorkBench\CloudStation\github_blog\jeongjiung.github.io\_images\unit45-4.png)