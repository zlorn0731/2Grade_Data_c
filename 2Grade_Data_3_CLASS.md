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
 
### 괄호 검사
- 같은 유형의 괄호들이 사용, 같은 유형의 괄호가 쌍을 이루어야 함
- 대괄호[], 중괄호{}, 소괄호()
  - 조건 1 : 왼쪽 괄호의 개수와 오른쪽 괄호의 개수가 같아야함
  - 조건 2 : 같은 타입의 괄호에서 왼쪽 괄호는 오른쪽 괄호보다 먼저 나와야함
  - 조건 3 : 서로 다른 타입의 괄호 쌍이 서로를 교차하면 안됨
 ```
{ A[(i+1)]=0; } → 오류 없음
if((i==0) && (j==0) → 오류 : 조건 1 위반
A[(i+1])=0; → 오류 : 조건 3 위반
```
#### 괄호검사 예시
```
{ A[(i+1)]=0; }

   {      A      [             (    i+1      )             ]     =0;
|     |       |     |       |     |       |     |       |     |       |     |
|     |       |     |       |     |       |     |       |     |       |     |
|     |   →   |     |   →   |  (  |   →   |     |   →   |     |   →   |     |
|     |       |  [  |       |  [  |       |  [  |       |     |       |     |
|  {  |       |  {  |       |  {  |       |  {  |       |  {  |       |     |
                                                                        성공
```
```
if((i==0) && (j==0)

if (             (    i==0     )      &&     (    j==0     )    
|     |       |     |       |     |       |     |       |     |       |     |
|     |       |     |       |     |       |     |       |     |       |     |
|     |   →   |     |   →   |     |   →   |     |   →   |     |   →   |     |
|     |       |  (  |       |     |       |  (  |       |     |       |     |
|  (  |       |  (  |       |  (  |       |  (  |       |  (  |       |  (  |
                                                                     조건1 오류
```
```
A[(i+1])=0;

A  [             (     i+1     ]     )   =0;
|     |       |     |       |     |
|     |       |     |       |     |
|     |   →   |     |   →   |     |
|     |       |  (  |       |     |
|  [  |       |  [  |       |  (  |
                           조건3 오류
```

### 프로그램 3.3 : 괄호 검사 프로그램
```
#include <stdio.h>
#include <stdlib.h>
#define MAX_STACK_SIZE 100
typedef char Element;

Element data[MAX_STACK_SIZE];
int top;

void error(char str[])
{
	printf("%s\n", str);
	exit(1);
}

void init_stack() { top = -1; }
int size() { return top + 1; }
int is_full() { return top == MAX_STACK_SIZE - 1; }
int is_empty() { return top == -1; }

Element push(Element e)
{
	if (is_full())
		error("포화상태에러");
	else
		return data[++top] = e;
}

Element pop()
{
	if (is_empty())
		error("공백상태에러");
	else
		return data[top--];
}

Element peek()
{
	if (is_empty())
		error("공백상태에러");
	else
		return data[top];
}

int check_matching(char expr[])
{
	int i = 0, prev;
	char ch;

	init_stack();
	while (expr[i] != '\0')
	{
		ch = expr[i++];
		if (ch == '[' || ch == '(' || ch == '{')
			push(ch);
		else if (ch == ']' || ch == ')' || ch == '}')
		{
			if (is_empty())
				return 2;
			prev = pop();
			if ((ch == ']' && prev != '[') || (ch == ')' && prev != '(') || (ch == '}' && prev != '{'))
			{
				return 3;
			}
		}
	}
	if (is_empty() == 0)
		return 1;
	return 0;
}

void main()
{
	char expr[4][80] = { "{A[(i+1])=0;}", "if ((i == 0) && (j == 0)", "A[(i+1)]=0;", "A[i] = f)(;" };
	int errorCode, i;

	for (i = 0; i < 4; i++)
	{
		errorCode = check_matching(expr[i]);
		if (errorCode == 0)
			printf(" 정상: %s\n", expr[i]);
		else
			printf(" 오류: %s (조건%d 위반)\n", expr[i], errorCode);
	}
}
```

### 프로그램 3.3 : 괄호 검사 프로그램 설명
- 문자열 안에 있는 괄호들 '()', '[]', '{}'이 올바르게 짝이 맞는지 검사하는 코드

#### 헤더파일
```
#include <stdio.h>
#include <stdlib.h>
#define MAX_STACK_SIZE 100
typedef int Element;
```
- #include <stdio.h> : printf() 쓰려고 필요
- #include <stdlib.h> : exit() 쓰려고 필요
- #define MAX_STACK_SIZE 100 : 스택의 최대 크기를 100으로 지정 | 스택에 괄호를 최대 100개
- typedef int Element; : 스택에 저장할 자료형 이름을 Element로 별칭

#### 전역 변수 
```
Element data[MAX_STACK_SIZE];
int top;
```
- data[] : 스택에 실제 데이터를 저장하는 배열
  - 여기에는 괄호가 들어감
- top : 스택의 맨 위 위치를 나타내는 변수

#### error 함수
```
void error(char str[])
{
	printf("%s\n", str);
	exit(1);
}
```
- 에러 메시지를 출력하고 프로그램 종료하는 함수
- char str[] : 문자열을 받기 위함
- exit(1) : 프로그램을 강제 종료

#### 스택 기본 함수들
- 초기화
```
void init_stack() { top = -1; }
```
  - 스택을 비운 상태로 시작하게 만드는 함수

- 크기
```
int size() { return top +1; }
```
  - 현재 스택에 원소가 몇 개 있는지 반환

- 가득 차있는지 확인
```
int is_full() { return top == MAX_STACK_SIZE - 1; }
```
  - 스택이 꽉 찼으면 1, 아니면 0 반환

- 비어 있는지 확인
```
int is_empty() { return top == -1; }
```
  - 스택이 비었으면 1, 아니면 0 반환

- push 함수
```
Element push(Element e)
{
	if (is_full())
		error("포화상태에러");
	else
		return data[++top] = e;
}
```
  - return data[++top] = e; → data[++top] = e;로 대부분 많이 사용

- pop 함수
```
Element pop()
{
	if (is_empty())
		error("공백상태에러");
	else
		return data[top--];
}
```

- peek 함수
```
Element peek()
{
	if (is_empty())
		error("공백상태에러");
	else
		return data[top];
}
```

#### 핵심 함수 : check_matching 🔥
```
int check_mathcing(char expr[])
```
- 문자열 expr을 받아서 괄호가 올바른지 검사하고 에러 코드 반환

#### 지역 변수들
```
int i = 0, prev;
char ch;
```
- i = 0 : 문자열의 몇 번째 문자를 보고 있는지 나타내는 인덱스
- prev : 스택에서 꺼낸 이전 여는 괄호를 저장하는 변수

#### 스택 초기화
```
init_stack();
```
- 괄호 검사를 시작하기 전에 스택을 비워야함

#### 문자열 끝까지 반복
```
while (expr[i] != '\0')
```
- 문자열의 끝 문자인 '\0'가 나올 때까지 한 글자씩 검사하겠다는 뜻
- C 문자열은 끝에 항상 '\0'

#### 문자 하나 읽기
```
ch = expr[i++];
```
- 현재 위치의 문자를 ch에 저장
- 그다음 i를 1증가시켜 다음 문자를 넘어감
```
예시:
처음 i=0
ch = expr[0]
그 다음 i=1
```

#### 여는 괄호를 만나면 '[', '(', '{'
```
if (ch == '[' || ch == '(' || ch == '{')
	push(ch);
```
- 현재 문자가 여는 괄호이면 스택에 넣음

#### 닫는 괄호를 만나면 ']', ')', '}'
```
else if (ch == ']' || ch == ')' || ch == '}')
```
- 닫는 괄호를 만났다면 검사 시작

#### 스택이 비어 있는지 검사
```
if (is_empty())
	return 2;
```
- 닫는 괄호가 나왔는데 스택이 비어 있다는 건 짝이 되는 여는 괄호가 없다는 뜻
-  조건 2 위반

#### 스택에서 여는 괄호 하나 꺼냄
```
prev = pop();
```
- 가장 최근에 열린 괄호를 꺼냄
- 스택은 LIFO이기 떄문에 가장 최근 여는 괄호부터 검사해야 함

#### 짝이 맞는지 검사 🔥
```
if ((ch == ']' && prev != '[') || (ch == ')' && prev != '(') || (ch == '}' && prev != '{'))
{
	return 3;
}
```
- 닫는 괄호 ch, 방금 pop한 여는 괄호 prev가 올바른 짝인지 확인
- 조건 3 위반

#### 문자열 검사 끝난 뒤 마지막 확인
```
if (is_empty() == 0)
	return 1;
return 0;
```
- 검사 다 끝났는데 스택이 안 비어 있으면 → 여는 괄호가 남아 있음 → 오류 1
- 스택이 비어 있으면 → 정상 → 0

#### main 함수
```
void main()
{
	char expr[4][80] = { "{A[(i+1])=0;}", "if ((i == 0) && (j == 0)", "A[(i+1)]=0;", "A[i] = f)(;" };
	int errorCode, i;
```
- char expr[4][80] : 문자열 4개를 저장하는 2차원 문자 배열

#### 반복문으로 4개 모두 검사
```
for (i = 0; i < 4; i++)
{
	errorCode = check_matching(expr[i]);
```
- 문자열 하나씩 check_matching()에 넣어서 검사

#### 결과 출력
```
if (errorCode == 0)
	printf(" 정상: %s\n", expr[i]);
else
	printf(" 오류: %s (조건%d 위반)\n", expr[i], errorCode);
```

### 수식의 계산
- 수식의 표기 방법
  - 전위(prefix) : 연산자를 피연산자 앞에 표기
  - 중위(infix) : 연산자를 피연산자 사이에 표기
  - 후위(postfix) : 연산자를 피연산자 뒤에 표기
- 
| 중위 표기법 | 전위 표기법 | 후위 표기법 |
|------------|------------|--------------|
| 2+3*4 | +2*34 | 234*+ |
| (A+B)*(C-D) | *+AB-CD | AB+CD-* |
| A+B*C-D | -+A*BCD | ABC*+D- |

### 후위 표기 수식 계산
- 동작원리
  - 전체 수식을 왼쪽에서 오른쪽으로 스캔
  - 스캔 과정에서 피연산자가 나오면 무조건 스택에 저장
  - 연산자가 나오면 스택에서 피연산자 두 개를 꺼내 연산을 실행
  - 그 결과를 다시 스택에 저장
```
예시: 82/3-32*+

|     |       |     |       |     |       |     |       |     |       |     |       |     |       |     |       |     |       |     |       |     |       |     |       |     |       |     |
|     |       |     |       |     |       |     |       |     |       |     |       |     |       |     |       |     |       |     |       |     |       |     |       |     |       |     |
|     |   →   |     |   →   |     |   →   |     |   →   |     |   →   |     |   →   |     |   →   |     |   →   |     |   →   |     |   →   |  2  |   →   |     |   →   |     |   →   |     |
|     |       |  2  |       |     |       |     |       |     |       |  3  |       |     |       |     |       |     |       |  3  |       |  3  |       |  3  |       |     |       |  6  |
|  8  |       |  8  |       |  8  |       |     |       |  4  |       |  4  |       |  4  |       |     |       |  1  |       |  1  |       |  1  |       |  1  |       |  1  |       |  1  |
push(8)       push(2)    /   pop(2)        pop(8)       push(4)       push(3)   -    pop(3)        pop(4)       push(1)       push(3)       push(2)   *   pop(2)         pop(3)       push(6)    
                                    8/2=4                                                   4-3=1                                                                 3*2=6
-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
    |     |       |     |       |     |
    |     |       |     |       |     |
→   |     |   →   |     |   →   |     |
    |     |       |     |       |     |
    |  1  |       |     |       |  7  |
     pop(6)   +    pop(1)       push(7) 
                          6+1=7
```

### 프로그램 3.4 : 후위 표기 수식의 계산 프로그램
```
#include <stdio.h>
#include <stdlib.h>
#define MAX_STACK_SIZE 100
typedef double Element;

Element data[MAX_STACK_SIZE];
int top;

void error(char str[])
{
	printf("%s\n", str);
	exit(1);
}

void init_stack() { top = -1; }
int size() { return top + 1; }
int is_full() { return top == MAX_STACK_SIZE - 1; }
int is_empty() { return top == -1; }

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

double calc_postfix(char expr[])
{
	char c;
	int i = 0;
	double val, val1, val2;

	init_stack( );
	while (expr[i] != '\0') {
		c = expr[i++];
		if (c >= '0' && c <= '9') {
			val = c - '0';
			push(val);
		}
		else if (c == '+' || c == '-' || c == '*' || c == '/') {
			val2 = pop();
			val1 = pop();
			switch (c) {
			case '+': push(val1 + val2); break;
			case '-': push(val1 - val2); break;
			case '*': push(val1 * val2); break;
			case '/': push(val1 / val2); break;
			}
		}
	}
	return pop();
}

void main()
{
	char expr[2][80] = { "8 2 / 3 - 3 2 * +", "1 2 / 4 * 1 4 / *" };

	printf("수식: %s = %1f\n", expr[0], calc_postfix(expr[0]));
	printf("수식: %s = %1f\n", expr[1], calc_postfix(expr[1]));
}
```

### 프로그램 3.4 : 후위 표기 수식의 계산 프로그램 설명
#### 자료형 정의
```
typedef double Element;
```
- Element라는 이름을 double 대신 씀
- double x; = Element  x;
- 후위 표기 수식 계산에서 나눗셈 결과가 소수가 나올 수 있어 double 사용

#### 스택 저장 공간
```
Element data[MAX_STACK_SIZE];
```
- double data[100]; = Element data[100]; 같은 뜻임
- 계산 도중 나오는 숫자들을 이 배열에 넣어두는 것

#### 핵심 함수 : calc_postfix 🔥
```
double calc_postfix(char expr[])
```
- 후위 표기식 문자열 expr을 받아 계산 결과를 double로 반환

#### 지역 변수 선언
```
char c;
int i = 0;
double val, val1, val2;
```
- char c; : 현재 읽고 있는 문자 1개를 저장하는 변수
```
예시:
'8'
'+'
'/'
' '
```
- int i = 0; : 문자열 인덱스
  - expr의 몇 번째 글자를 보고 있는지 나타냄
- val : 숫자를 하나 읽었을 때 임시 저장하는 변수
```
예시:
문자 '8'을 숫자 8로 바꿔서 val에 넣음
```
- val1, val2 : 연산자 만났을 때 스택에서 꺼낸 두 피연산자를 저장하는 변수
```
예시:
8 2 /
val2 = 2
val1 = 8
그리고 계산
val1 / val2 = 8 / 2
```

#### 숫자인지 검사
```
if (c >= '0' && c <= '9') {
	val = c - '0';
	push(val);
}
```
- 이 부분은 현재 문자가 숫자라면 스택에 넣는 코드
```
예시:
'8' → 참
'3' → 참
'+' → 거짓
' ' → 거짓
```
- val = c - '0'; : 문자 '8'을 숫자 8로 바꾸는 코드
  - 아스키 코드 숫자를 내부적으로 갖고 있음
```
예시:
'8'의 코드값 = 56
'0'의 코드값 = 48
'8' - '0' = 56 - 48 = 8
```
- push(val); : 변환된 숫자를 스택에 넣음

#### 연산자인지 검사
```
else if (c == '+' || c == '-' || c == '*' || c == '/') {
	val2 = pop();
	val1 = pop();
	switch (c) {
	case '+': push(val1 + val2); break;
	case '-': push(val1 - val2); break;
	case '*': push(val1 * val2); break;
	case '/': push(val1 / val2); break;
	}
}
```
- 현재 문자가 연산자이면 계산을 수행하는 부분
- switch문
  - 현재 연산자 c에 따라 계산을 다르게
  - if로 해도 가능하지만 코드가 길어지면 가독성 이유로 switch 추천
  - 하나의 변수에 대해 여러 값 비교할 때 추천

### 중위 표기 수식의 후위 표기 변환
#### 중위 표기 수식 : 우리가 평소 쓰는 식
- 연산자가 중간에 있음
```
A + B
A * B + C
(A + B) * C
```

#### 후위 표기식 : 컴퓨터가 계산하기 쉬운 상태
- 연산자가 뒤에 있음
```
A B +
A B * C +
A B + C *
```

#### 핵심 아이디어
- 연산자는 스택에 넣고, 피연산자는 바로 출력
 
#### 기본 규칙 4개
- 피연산자 : A, B, 1, 2 그냥 출력
- 여는 괄호 '(', '[', '{' : 무조건 스택에 push
- 다는 괄호 ')', ']', '}' : 같은 괄호가 나올 때까지 pop해서 출력
- 연산자(+, -, *, /) : 우선순위 비교해서 처리
  - 우선순위 : 괄호-가장 낮음 |  (+, -)-낮음 | (*, /)-높음
  - 현재 연산자보다 스택 위 연산자가 우선순위 높거나 같으면 pop
   
#### 알고리즘 흐름
- 1. 피연산자 → 바로 출력
  2. '(' → push
  3. ')' → '(' 만날 때까지 pop
  4. 연산자 → 스택 top이 더 강하면 pop, 아니면 push
  5. 끝나면 → 스택에 남은 거 전부 pop
```
예시 1:
A + B * C

중위 표기식
| A | + | B | * | C |

A → 출력   A

+ → push
|   |
|   |
| + |

B → 출력   A B

* → push
|   |
| * |
| + |

C → 출력   A B C

* → pop   A B C * 
|   |
|   |
| + |

+ → pop   A B C * +
|   |
|   |
|   |

후위 표기식
| A | B | C | * | + |
```
```
예시 2:
( A + B ) * C

중위 표기식
| ( | A | + | B | ) | * | C |

( → 괄호는 일단 스택에 push
|   |
|   |
| ( |

A → 출력   A

+ → push
|   |
| + |
| ( |

B → 출력   A B

) → )가 나오면 (전까지 모두 출력 괄호는 후위표기식에 출력  ❌   A B +

* → push
|   |
|   |
| * |

C → 출력   A B + C

* → pop   A B + C *

후위 표기 수식
| A | B | + | C | * |
```

### 프로그램 3.5 : 중위 수식의 후위 표기 변환 프로그램
```
#include <stdio.h>
#include <stdlib.h>
#define MAX_STACK_SIZE 100
typedef char Element;

Element data[MAX_STACK_SIZE];
int top;

void error(char str[])
{
	printf("%s\n", str);
	exit(1);

}

void init_stack() { top = -1; }
int size() { return top + 1; }
int is_full() { return top == MAX_STACK_SIZE - 1; }
int is_empty() { return top == -1; }

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

int precedence(char op)
{
	switch (op)
	{
	case '(': case ')': return 0;
	case '+': case '-': return 1;
	case '*': case '/': return 2;
	}
	return -1;
}

void infix_to_postfix(char expr[])
{
	int i = 0;
	char c, op;

	init_stack();
	while (expr[i] != '\0')
	{
		c = expr[i++];
		if ((c >= '0' && c <= '9'))
		{
			printf("%c ", c);
		}
		else if (c == '(')
			push(c);
		else if (c == ')')
		{
			while (is_empty() == 0)
			{
				op = pop();
				if (op == '(')
					break;
				else printf("%c", op);
			}
		}
		else if (c == '+' || c == '-' || c == '*' || c == '/')
		{
			while (is_empty() == 0)
			{
				op = peek();
				if (precedence(c) <= precedence(op))
				{
					printf("%c ", op);
					pop();
				}
				else break;
			}
			push(c);
		}
	}
	while (is_empty() == 0)
		printf("%c ", pop());
	printf("\n");
}

void main()
{
	char expr[2][80] = { "8 / 2 - 3 + 3 ( 3 * 2)", "1 / 2 * 4 * ( 1 / 4)" };

	printf("중위수식: %s ==> 후위수식:", expr[0]);
	infix_to_postfix(expr[0]);
	printf("중위수식: %s ==> 후위수식:", expr[1]);
	infix_to_postfix(expr[1]);
}
```

### 프로그램 3.5 : 중위 수식의 후위 표기 변환 프로그램 설명
- 문자열로 된 중위식을 읽고
  - 숫자는 바로 출력
  - 연산자는 스택에 잠깐 저장
  - 우선순위에 따라 꺼내서 출력
- 방식으로 후위식으로 변환
- 핵심 아이디어 : 피연산자는 바로 출력, 연산자는 스택으로 관리

#### precendence 함수
- 연산자 우선순위를 숫자로 바꿔주는 함수
```
int precedence(char op)
{
	switch (op)
	{
	case '(': case ')': return 0;
	case '+': case '-': return 1;
	case '*': case '/': return 2;
	}
	return -1;
}
```
- 숫자가 클수록 우선순위가 높음
  - (,)  →  0
  - +,-  →  1
  - *,/  →  2

#### 핵심 함수 : infix_to_postfix 🔥
- 중위 수식 문자열 expr[]을 받아서 후위식 형태로 화면에 출력하는 함수
```
void infix_to_postfix(char expr[])
```

#### 지역 변수
```
int i = 0;
char c, op;
```
- i=0 : 문자열 인덱스
- c : 현재 읽고 있는 문자 1개 저장
- op : 스택에서 꺼낸 연산자를 저장할 변수

#### 숫자인 경우
```
if ((c >= '0' && c <= '9'))
{
	printf("%c ", c);
}
```
- 현재 문자 숫자라면 바로 출력
  - 후위식에서는 피연산자는 순서대로 그대로 출력이기 때

 #### 닫는 괄호 ) 인 경우
```
 else if (c == ')')
{
	while (is_empty() == 0)
	{
		op = pop();
		if (op == '(')
			break;
		else printf("%c ", op);
	}
}
```
- 닫는 괄호를 만나면 스택에서 연산자를 꺼내다가 여는 괄호 ( 를 만날 때 멈춤

#### 연산자 + - * / 인 경우 🔥
```
else if (c == '+' || c == '-' || c == '*' || c == '/')
{
	while (is_empty() == 0)
	{
		op = peek();
		if (precedence(c) <= precedence(op))
		{
			printf("%c ", op);
			pop();
		}
		else break;
	}
	push(c);
}
```
- 변환 알고리즘의 핵심 부분
- peek() : 현재 스택 맨 위 연산자와 지금 읽는 연산자 c의 우선순위를 비교하기 위해
- precendence(c) <= precendence(op) : 현재 읽는 연산자의 우선순위가 스택 top 연산자보다 낮거나 같으면 스택 top을 먼저 출력
- push(c) : 앞에서 필요한 연산자들은 다 꺼냈으니 이제 현재 연산자를 스택에 넣음

#### 반복문 끝난 후
```
while (is_empty() == 0)
	printf("%c ", pop());
printf("\n");
```
- 더 이상 비교할 문자가 없으니까 남은 연산자는 전부 후위식 뒤에 붙임

#### main 함수
```
void main()
{
	char expr[2][80] = { "8 / 2 - 3 + 3 ( 3 * 2)", "1 / 2 * 4 * ( 1 / 4)" };

	printf("중위수식: %s ==> 후위수식:", expr[0]);
	infix_to_postfix(expr[0]);
	printf("중위수식: %s ==> 후위수식:", expr[1]);
	infix_to_postfix(expr[1]);
}
```
- 두 개의 중위 수식을 변환해서 출력하는 부분

##### ✍️작성자: 박지안
##### 🐧실습 환경: Visual Studio 2022
##### 🗓️ 작업일: 2026-03-29
