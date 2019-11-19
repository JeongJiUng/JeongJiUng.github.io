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

