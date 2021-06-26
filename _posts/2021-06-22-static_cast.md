---
layout: post
title: static_cast<>이란 무엇일까.
category: C++
tag: C++
---

## static_cast<>

static_cast<바꾸려고 하는 타입>(대상);
static_cast<new_type>(expression)

특징 (논리적으로 변환 가능한 타입을 변환한다)

compile 타임에 형변환에 대한 타입 오류를 잡아줍니다.

실수와 정수, 열거형과 정수형, 실수와 실수 사이의 변환 등을 허용한다.
arr -> point로 변경 가능합니다.

function -> function pointer로 변경 가능합니다.

포인터 타입을 다른것으로 변환 하는 것을 허용하지 않습니다. (compile error)

그러나, 상속 관계에 있는 포인터 끼리 변환이 가능합니다.

downcast (static_cast<자식클래스>(부모클래스))시에는 unsafe 하게 동작할 수 있습니다. (safe 하게 사용하려면 dynamic_cast사용)

바꾸려고 하는 타입(new_type)에는 void 타입이 올 수 있지만 계산 후 제거 합니다. (return 이 없다 -> discard expression)

### 일반 변수 타입

```c++
#include<iostream>
using namespace std;
int main(void){
    double d = 13.24;
    float f = 10.12f;

    double tmp_double;
    int tmp_int;
    float tmp_float;

    tmp_int = static_cast<int>(d);    //double -> int 로 형변환
    cout << "static_cast<int>(double) : " << tmp_int << endl;

    tmp_float = static_cast<float>(d); //double -> float 로 형변환
    cout << "static_cast<float>(double) : " << tmp_float << endl;

    tmp_double = static_cast<double>(f); //float -> double 로 형변환
    cout << "static_cast<double>(float) : " << tmp_double << endl;

    return 0;
}
```

### 포인터 타입

```c++
#include<iostream>
#include<cstdio>
using namespace std;
int main(void){
    //1. array->point//
    int arr[10] = {11,13,15,17,19,21,23,25,27,29};
    int * ptr;
    ptr = static_cast<int *>(arr);
    cout << "1. int array -> int * : ";
    for(int i=0; i<10; i++){
        cout <<  ptr[i] << " ";
    }
    printf("\n");

    //2. char * -> int *//
    char str[] = "BlockDMask";
    int * ptr2;
    //ptr2 = static_cast<int *>(str); //Compile error
    //cout << *ptr2 << endl;


    //3. int * -> char *//
    int tmp = 10;
    int * ptr3 = &tmp;
    //char * c = static_cast<char *>(ptr3); //Compile error
    //cout << "3. int* -> char* : " << *c << endl;

    return 0;
}
```
