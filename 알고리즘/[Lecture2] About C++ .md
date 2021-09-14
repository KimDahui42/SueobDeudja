### C vs C++

객체지향프로그램 : 소프트웨어를 부품화하는 것.



### 컴파일 및 실행하기

- 컴파일하기

  `g++ -o 실행파일명 컴파일할파일명`

- 실행파일 만들기

  `./실행파일명`

- 추가 명령어

  `!g`: 제일 최근 연 파일 보여줌



### 화면 입출력

- `#include <iostream>`사용
- 출력 : cout <<
- 입력 : cin >>
- 개행 : '/n', 'endl' 사용



### 메모리 동적 할당

- 사이즈 낭비를 막기 위해 상황에 따라 동적할당

- C에서의 malloc / free 함수

- C++  new / delete 

- C++ 포인터 사용

```c++
int main (void){
    int *p;
    int i;
    p = new int[10];
    for(i=0; i<10; i++)	p[i] = 2*i+1;
    for(i=0; i<10; i++) cout << *(p+i) << endl; // p[i]와 동일
    delete[] p;

	return 0;

}
```



### 클래스

- 멤버 변수와 멤버 함수 사용
- class 선언은 함수 밖에서 한다.

```c++
class foo{
public:
    int a;
    void increas(int b){
        a += b;
    }
}; // ; 주의

int main (void){
    foo x;
    x.a = 4; // x의 멤버 a 사용
    x.increase(5);
    cout << x.a << endl;
    return 0;
}
```



- public과 private

  접근 권한 부여. 대부분 멤버 변수를 직접 접근하게 하지 않음.

```c++
class foo{
private:
    int a;
public:
    void increas(int b){
        a += b;
    }
};

int main (void){
    foo x;
    x.a = 4; // 컴파일에러 - private 영역 접근 불가
    x.increase(5);
    cout << x.a << endl; // 컴파일에러 - private 출력 불가
    return 0;
}
```

- 변수에 접근할 수 있도록 함수(setter getter)를 설정해준다.

  대부분의 규칙 : set, get 소문자 + 변수 대문자

```c++
class foo{
public:
    void setA(int t){
        a = t;
    }
    void getA(void){
        return a;
    }
};
```

- setter과 getter로 접근한 예

```c++
int main (void){
    foo x;
    x.setA(4);
    x.increase(5);
    cout << x.getA() << endl;
    return 0;
}
```



### Constructor: 생성자

- 클래스 이름과 동일한 멤버 함수
- 클래스 변수가 선언될 때 자동으로 실행

```c++
int main (void){
    foo x;
}
```

- Constructor 자동실행

```c++
class foo{
private:
    int a;
public:
    foo(){
        a = 0; // 초기에 변수 초기화
    }
};
```



### Overloading

- C++는 동일한 이름의 함수를 여러개 생성 가능. 하지만 매개변수는 달라야 한다.

```c++
class foo{
private:
    int a;
public:
    foo(){a = 0;}
    foo(int t){a = t;}
    void increase(int b){ a += b;}
    void increase(float b){a += (int)b;}
};
```



```c++
int main (void){
    foo x;
    foo y(10);
    x.increase(4);
    y.increase(5.6f);
}
```



### 클래스 포인터로 선언하기

```c++
int main (void){
    foo *x, *y;
    x = new foo();
    y = new foo(10);
    (*x).increase(4);
    (*y).increase(5.6f);
}
```

- 멤버 변수와 멤버 함수 엑세스 방법

```c++
// (*[변수이름])
(*x).increase(4);
(*y).increase(5.6f);	
```

```c++
// [변수이름] ->
x->increase(4);
y->increase(5.6f);
```

- 동적할당을 했으니 해제 해주자

`delete x,y;`



### 클래스 함수 밖으로 빼기: 선언은 앞에서 정의는 뒤에서

- 가독성을 위해 선언과 정의 분리

- 멤버함수 정의하는 법

  `[반환타입] [클래스이름] :: [멤버 함수 이름] ([매개변수]) {[함수내용]}`

```c++
class foo{
public:
    foo();
    foo(int t);
    void increase(int b);
    void increase(float b);
};

foo::foo(){ // foo 안의 함수이다.
    a = 0;
}
foo::foo(int t){
    a = t;
}
void foo::increase(int b){ 
    a += b;
}
void foo::increase(float b){
    a += (int)b;
}
```



### this 포인터

- 클래스의 멤버함수와 매개변수 이름이 같을 때 구분이 가도록 (물론 같은것 비추)
- 자기 자신을 가리키는 포인터
- this으로 멤버변수 표시

```c++
foo::foo(){
    a = 0;
}
foo::foo(int a){
    this->a = a;
}
void foo::increase(int a){
    this->a += a;
}
void foo::increase(float a){
    this->a += (int)a;
}
```



### 가독성을 위한 파일 분리

1.  foo.h : 선언부분

```c++
#ifndef __FOO_H
#define __FOO_H
class foo{
private:
    int a;
public:
    foo();
    foo(int t);
    void increase(int b);
    void increase(float b);
};

#endif
```

2.  foo.cpp : 멤버함수 정의부분 (`#include "foo.h"` 추가)

```c++
#include <iostream>
#include "foo.h

foo::foo(){
    a = 0;
}
foo::foo(int a){
    this->a = a;
}
void foo::increase(int a){ // foo 안의 함수이다.
    this->a += a;
}
void foo::increase(float a){
    this->a += (int)a;
}
```

3. hello.cpp : 메인함수 부분 (`#include "foo.h"` 추가)

```c++
#include <iostream>
#include "foo.h

int main (void){
    foo *x, *y;
    x = new foo();
    y = new foo(10);
    (*x).increase(4);
    (*y).increase(5.6f);
    return 0;
}
```



- 컴파일방법

`g++ -o hello hello.cpp foo.cpp`



