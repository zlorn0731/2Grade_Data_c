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
init스택의 구현 부터 작성 요함
