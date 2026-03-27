# 📚 2학년 데이터구조 (Data Structures)
## 3장 : 스택

### 스택이란?
- 스택(stack) : 쌓아놓은 더미
- 후입선출 : LIFO(Last-In First-Out)
  - 가장 최근에 들어온 데이터가 가장 먼저 나감
- 구현 : 1차원 배열, 연결 리스트

### 스택의 구조
- 스택 상단 : top
- 스택 하단 : 불필요
- 요소(element), 항목
  - 스택에 저장되는 것
- 공백(empty) 상태
- 포화(full) 상태
- 삽입, 삭제 연산
  - 삽입 → full 상태에서 삽입 ❌
```
포화(full) 상태
|-----|
|  C  |
|  B  |
|  A  |

공백(empty) 상태
|-----|
|     |
|     |
|     |
```

### 스택 추상 자료형
- 추상 자료형 : ADT
  - "무엇을 할 수 있는지"만 정의 "어떻게 구현하는지" 숨김
- 객체 : 무엇이든 가능
  - 예시) 닦은 접시, 상자, 해야 할 일, 문서편집기 뒤로가기 등

### 스택 ADT 연산
- init() : 스택의 초기화
- is_empty() : 스택이 비어 있으면 TRUE 아니면 FALSE를 반환
- is_full() : 스택이 가득 차 있으면 TRUE 아니면 FALSE를 반환
- size() : 스택내의 모든 요소들의 개수를 반환
- push() : 주어진 요소를 스택의 맨 위에 추가
- pop() : 스택 맨 위에 있는 요소를 삭제하고 반환
- peek() : 스택 맨 위에 있는 요소를 삭제하지 않고 반환

### 스택의 연산
- 삽입(push) 삭제(pop)
```
|   |     |   |     |   |     |   |     |   |
|   |     |   |     |   |     |   |     |   |
|   |  →  |   |  →  |   |  →  | C |  →  |   |
|   |     |   |     | B |     | B |     | B |
|___|     | A |     | A |     | A |     | A |
         push(A)   push(B)   push(C)    pop(C)
```
- is_empty() : 스택이 공백상태인지 검사
  - pop()을 하기 위해선 공백 상태에선 안됨, is_empty()로 먼저 체크
- is_full() : 스택이 포화상태인지 검사
  - push()을 하기 위해선 포화 상태에선 안됨, is_full()로 먼저 체크
- peek() : 요소를 스택에서 삭제하지 않고 보기만 하는 연산
  - pop() : 요소를 스택에서 완전히 삭제하면서 가져옴

### 스택의 활용
- 자료의 출력순서가 입력순서의 역순으로 이루어져야 할 경우 사용
- 예시) (A, B, C, D) - push순서 → (D, C, B, A) - pop순서
- 되돌리기, 괄호닫기 검사, 계산기 등
```
#include <stdio.h>
int main()
{
     a();  (1)
     return 0; (7)
}
------∨--------
int a()
{
    b();  (2)
    c();  (4)
    return 0; (6)
}
------∨--------
int b()
{
    ...
    return 0;  (3)
}
int c()
{
    ...
    return 0;  (5)
}
-----------------
초기 상태 → main()에서 a()호출 → a()에서 b()호출 → b()반환
→ a()에서 c()호출 → c()반환 → a()반환
```

### 스택의 구현 방법
- 배열 : 가장 간단하게 구현, 고정된 크기
  - 예시) 일반 공책 : 고정된 페이지에 작성
- 연결 리스트 : 복잡한 코드, 유연한 크기
  - 예시) 바인더 공책 : 페이지를 추가 삭제 작성 가능

### 배열을 이용한 스택의 구현
- 1차원 배열 stack[]
  - 정수 항목들을 저장
    - 배열이름[] = data[MAX_STACK_SIZE]
  - top : 가장 최근에 입력된 자료를 가리키는 변수
  - stack[0] .... stack[top] : 먼저 들어온 순으로 저장
  - 공백상태 : top은 -1
  - 포화상태 : top은 MAX_STACK_SIZE - 1
```
data[MAX_STACK_SIZE]
data[4] |       |
data[3] |       |
data[2] |   C   | ← top
data[1] |   B   |
data[0] |   A   |

        공백 상태
data[4] |       |
data[3] |       |
data[2] |       |
data[1] |       |
data[0] |       | 
data[-1] ← top

        포화 상태
data[4] |   E   | ← top
data[3] |   D   |
data[2] |   C   |
data[1] |   B   |
data[0] |   A   |
data[-1]
```

### 배열을 이용한 스택의 구현
- 알고리즘 3.1 : 스택의 is_empty() 연산
```
is_empty() // 스택이 공백상태인지 검사

if top = -1 // 공백이면 top = -1
    then TRUE
else
      return FALSE
```
```
C언어 코드
int is_empty() {
    return (top == -1);
}
```
- 알고리즘 3.2 : 스택의 is_full() 연산
```
is_full() // 스택이 포화상태인지 검사

if top = MAX_STACK_SIZE - 1 // 포화상태면 top MAX_STACK_SIZE - 1
    then
         return TRUE
    else
         return FALSE
```
```
C언어 스타일
int is_full() {
    return (MAX_STACK_SIZE -1);
}
```
- 스택의 초기화와 요소의 개수 검사
- 초기화
  - 스택을 공백상태로 만드는 것
  - top을 -1
- 요소의 개수 : top + 1

- 알고리즘 3.3 : 스택의 init() 연산
```
init() // 스택 초기화

   top = -1; // 공백상태면 top = -1
```
```
C언어 스타일
void init_stack() {
    top = -1;
}
```

- 알고리즘 3.4 : 스택의 size() 연산
```
size() // 스택의 요소의 개수

   return top + 1 // 요소의 개수 : top + 1
```
```
C언어 스타일
int size() {
    return top +1;
}
```

### push 연산
- (step1) 스택이 포화상태인지 체크
- (step2) top을 하나 증가
- (step3) 새로운 항목은 스택의 맨 위에 올라가야함

- 알고리즘 3.5 : 스택의 push() 연산
```
push(x) // x요소 추가

if is_full() // 포화상태인지 검사
     then error "overflow" // 포화상태면 push를 한 순간 배열을 넘쳐 overflow
     else top ← top + 1 // 포화상태가 아니면 top 하나 증가
          data[tap] ← x // top의 요소 x
```
```
C언어 스타일
void push(Element e) {
    if (is_full())
       error("스택 포화 에러");
    data[++top] = e;
```

### pop 연산
- 스택의 맨 위에 있는 요소를 꺼내서 반환
- (step1) 공백상태인지 체크
- (step2) top이 가리키는 값을 반환
- (step3) top 하나 감소
```
pop() // 요소 꺼냄

if is_empty() // 공백상태인지 검사
     then error "underflow" // 공백상태면 pop을 한 순간 꺼낼게 없어서 underflow
     else e ← data[top] // 공백상태가 아니면 
          top ← top - 1
          return e
```
```
C언어 스타일
Element pop() {
    if (is_empty())
       error("스택 공백 에러");
    return data[top--];
```

### 스택의 구현 : int 스택
- 자료구조의 핵심 기능에 집중, 코드를 단순화하기 위해 다음과 같이 구현
  - 스택을 위한 데이터, 즉 data[], top을 전역 변수로 선언
  - 스택의 연산들은 함수로 구현함
  - 스택 항목의 자료형을 Element
  - Element는 응용에 따라 적절한 자료형을 지정 가능, typedef, #define을 사용할 수 있음
```
1. #define Element int // Element가 int를 의미
2. typedef double Element; // Element가 double를 의미
3. #define Element Student // Element가 구조체 Student를 의미
```
- int를 저장하는 스택
```
typedef int Element;
Element data[MAX_STACK_SIZE];
int top;
```
- 스택 초기화와 항목 개수 검사 함수
```
void init_stack() { top = -1; }
int size() { return top+1; }
```
- is_empty(), is_full() 함수
```
int is_empty() { return (top == -1); }
int is_full() { return (top == +1); }
```

#### 프로그램 3.1 : 배열을 이용한 int 스택의 구현
```
#include <stdio.h>
#include <stdlib.h> 
#define MAX_STACK_SIZE 100 // MAX_STACK_SIZE의 크기는 100으로 지정
typedef int Element; // 코드에서 int 요소만 가능

Element data[MAX_STACK_SIZE]; // data[] 선언
int top; // 초기 top 선언

// 오류 상황 처리를 위한 함수, 메시지 출력 후 프로그램 종료
void error(char c[]) // char c[] 매개변수
{
	printf("%s\n", c);
	exit(1); // 에러면 프로그램 종료, #include <stdlib.h> 필요
}

// 스택의 연산
// init(), size(), is_empty(), is_full(), push(), pop()
void init_stack() { top = -1; } // 초기화 : 반환 안해줘 됨. 그래서 void를 쓰고 return도 없음
int size() { return top + 1; } // 개수 반환
int is_empty() { return top == -1; } // 비어있는지 판단
int is_full() { return top == MAX_STACK_SIZE - 1; } // 꽉 찼는지 판단

void push(Element e) // 요소 추가
{
	if (is_full()) // 포화상태인지 검사
		error("스택 포화 에러"); // 포화면 에러
	data[++top] = e; // 포화가 아니면 data[top]은 Element e
}

Element pop() // 요소 삭제
{
	if (is_empty()) // 공백상태인지 검사
		error("스택 공백 에러"); // 공백이면 에러
	return data[top--]; // 공백이 아니면 data[top]은 top -1
}

Element peek() // 꺼내지 않고 요소들 확인
{
	if (is_empty()) // 공백상태인지 검사
		error("스택 공백 에러"); // 공백이면 에러
	return data[top]; // 공백이 아니면 요소 확인이니 top값 반환 
}

// 스택 테스트를 위한 코드: 요소 종류마다 수정
void print_stack(char msg[])
{
	int i;
	printf("%s[%2d]= ", msg, size());
	for (i = 0; i < size(); i++) {
		printf("%2d ", data[i]);
	}
	printf("\n");
}

void main() {
	int i;
	init_stack(); // top 초기화
	for (i = 1; i < 10; i++) { // 변수는 1 ~ 9
		push(i); // i 스택에 push
	}
		print_stack("스택 push 9회");
		printf("\tpop() ---> %d\n", pop()); // pop 1
		printf("\tpop() ---> %d\n", pop()); // pop 2
		printf("\tpop() ---> %d\n", pop()); // pop 3
		print_stack("스택 pop 3회");
}
```

### 스택의 구현 : 구조체를 저장하는 스택
- 학생 정보
  - 구조체 정의
```
typedef struct Student_t // 학생정보 구조체 이름
{
int id; // 학번
char name[32]; // 이름
char dept[32]; // 소속 학과
} Student; 
```
- Element 정의
  - 스택의 기본 연산을 그대로 사용가능
```
1. typedef Student Element; // 키워드 typedef를 사용하는 방법
2. #define Element Student // 전처리 명령어 #define을 사용하는 방법
```
- 화멸 출력을 위한 print_stack()에서는 구조체를 출력하도록 약간의 코드 수정 필요

#### 프로그램 3.2 : 배열을 이용한 Student 스택의 구현
```
#define _CRT_SECURE_NO_WARNINGS
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#define MAX_STACK_SIZE 100 // 스택 요소 저장을 위한 배열의 크기
typedef struct Student // 스택에 저장할 요소의 자료형, Student 구조체 정의
{
	int id; // 학번
	char name[32]; // 이름
	char dept[32]; // 학과
} Student; // 스택 요소 저장을 위한 배열의 크기
typedef Student Element; // 스택 요소의 자료형 정의, Element가 Student를 지정, Element = Student
Element data[MAX_STACK_SIZE]; // 실제 스택 요소의 배열
int top; // 실제 스택의 top

// 에러
void error(char er[])
{
	printf("%s\n", er);
	exit(1);
 }

// 연산
void init_stack() { top = -1; }
int size() { return top + 1; }
int is_empty(){ return top == -1; }
int is_full() { return top == MAX_STACK_SIZE - 1; }

Element push(Element e)
{
	if (is_full())
		error("Error 포화상태");
	else
		return data[++top] = e;
}

Element pop()
{
	if (is_empty())
		error("Error 공백상태");
	else
		return data[top--];
}

Element peek()
{
	if (is_empty())
		error("Error 공백상태");
	else
		return data[top];
}

void print_stack(char msg[])
{
	int i;
	printf("%s[%2d]= ", msg, size());
	for (i = 0; i < size(); i++)
		printf("\n\t%-15d %-10s %-20s", data[i].id, data[i].name, data[i].dept);
	printf("\n");
}

Student get_student(int id, char name[], char dept[]) // 새로운 Student 객체를 만들어 반환하는 함수
{
	Student s;
	s.id = id;
	strcpy(s.name, name);
	strcpy(s.dept, dept);

	return s;
}

void main()
{
	init_stack();
	push(get_student(20230844, "박지안", "컴퓨터공학과"));
	push(get_student(20230833, "조준형", "컴퓨터공학과"));
	push(get_student(20230822, "김동우", "컴퓨터공학과"));
	push(get_student(20230811, "최동훈", "컴퓨터공학과"));
	print_stack("친구 4명 입력 후");
	pop();
	print_stack("친구 1명 삭제 후");
}
```
- get_Student()가 없다면
```
Student s1 = { 20230844, "박지안", "컴퓨터공학과 };
push(s1);
이 코드를 이용해야 함
```
- 이 함수에서 문자열을 복사하기 위해 표준 라이브러리 함수인 strcpy() 사용
  - #include <string.h> 헤더 파일 생성
 
pg90까지 맞췄지만 앞에 코드 계속 써보면서 이해하기
