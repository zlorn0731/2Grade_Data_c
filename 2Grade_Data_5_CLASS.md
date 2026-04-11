# 📚 2학년 데이터구조 (Data Structures)
## 5장 : 포인터와 연결리스트

### 포인터(pointer)
- 포인터는 C언어에서 매우 중요하고 여려운 개념
- 컴퓨터는 메모리(memory)란 저장 공간을 사용하고, 모든 메모리는 주소(address)를 갖음
변수나 배열은 메모리의 어떤 위치에 저장
만약 주소를 알면 거기에 저장된 변수 값을 참조할 수 있음
포인터 변수(pointer variable), 포인터(pointer)는 이러한 메모리의 주소를 저장하기 위한 변수
보통 컴퓨터의 메모리는 Byte(8Bit)단위로 구성되어 있음

### 포인터 선언
- 포인터도 변수이므로 일반 변수와 같이 사용하기 전에 먼저 선언해야 함
포인터는 항상 어떤 자료형의 변수를 가리킴
가리키는 자료형을 먼저 쓰고 *를 붙인 다음 변수 이름을 씀
*는 곱셈과 아무 상관 없음
여러 개의 포인터 변수를 선언할 때 한 문장에도 가능
```
int *pi; // int 변수를 가리킬 목적의 포인터 pi를 선언
float *pf; // float 변수를 가리킬 목적의 포인터 pf를 선언
char* p, q, r; // p는 char* 변수, q와 r은 char 변수
char *p, *q, *r; // p, q, r 모두 char*형 변수
```
- 포인터는 선언만 되었지 초기화는 안되어있음
- 포인터는 사용하기 전에 반드시 초기화해야 함
```
int i = 10;
int *p;
p = &i;

printf("%p", p); // 주소 출력 → %p = 주소 출력용
printf("\n%d", *p); // 값 출력 → *p = p가 가리키는 값(즉 i)
```
- 모든 변수는 이름, 주소, 값을 갖음
  - 포인터도 마찬가지
- 포인터 p는 주소 추출 연산자 &를 사용해 구한 변수 i의 주소를 값으로 갖고 나면 사용 가능
  - 포인터가 어떤 변수를 가리키게 되는 것

### 포인터 활용
- 포인터가 가리키는 메모리의 내용을 추출하거나 변경하려면 역참조 연산자 *를 사용
  - i의 값을 변경 가능
```
int i = 10;
int* p;
p = &i;
*p = 20; // i = 20;과 동일한 문장
```
- *p는 p가 가리키는 곳의 내용을 의미하므로 결국 *p와 변수 i는 완전히 동일
- *p의 값을 변경하면 변수 i의 값도 변경
  - 곱셈 기호 *가 여러 가지 방법으로 사용되어 주의
```
c = a * b; // 곱셈 연산자로 사용된 *
int* p; // 포인터 선언에 사용된 *
*p = 20; // 포인터의 역참조 연산자로 사용된 *
```
- 비트단위 AND연산자 기호인 &도 두 가지 방법 사용되니 주의
```
c = a & b; // 비트단위 AND 연산자로 사용된 &
p = &i; // 변수의 주소추출 연산자로 사용된 &
```

#### 포인터 활용 복잡한 상황
- 경우에 따라서는 어떤 포인터가 다른 포인터를 가리켜야 하는 경우도 발생
이 경우 자료형은?
int 변수를 가리키는 포인터 p가 있을 때, 이 포인터를 가리키는 포인터 pp를 선언
pp의 자료형은 무엇이 되어야 할까?
p의 자료형에 *를 붙이면 됨
p의 자료형이 int*이므로 pp의 자료형은 int**가 됨
```
int i = 10; // i = 10
int *p = &i; // p = &i
int **pp = &p; // pp = &p
```
- 포인터는 여러 가지 자료형의 대상에 대해 선언 가능
```
void *P; // 임의 자료형의 주소를 저장하기 위한 포인터
int *pi; // int 변수의 주소를 저장하기 위한 포인터
float *pf; // float 변수의 주소를 저장하기 위한 포인터
char *pc; // char 변수의 주소를 저장하기 위한 포인터
int **pp; // 포인터 변수의 주소를 저장하기 위한 포인터
Test *ps; // Test 구조체의 주소를 저장하기 위한 포인터
void (*f)(int); // int를 매개변수로 갖고 반환이 없는 함수의 주소를 저장하기 위한 포인터
```

#### 이중 포인터
- 이중 포인터 : 포인터도 일종의 변수이기 때문에 포인터를 가리키는 포인터도 존재함, 이를 이중 포인터 라고 함
```
int a = 5;
int *p = &a;
int **p2 = &p;
int ***p3 = &p2;
```
```
  - p3 -      - p2 -      - p -     - a -
| p2 주소 |  | p 주소 |  | a 주소 |  | 5 | 
p3 ? p2주소   p2 ? p주소   p ? a주소  a ? 5
*p3 ? p주소   *p2 ? a주소  *p ? 5
**p3 ? a주소  **p2 ? 5
***p3 ? 5

역참조 : p3 = 3번, p2 = 2번, p1 = 1번
```

### 포인터 연산
- 포인터 연산은 보통의 연산과 다른 의미
- 포인터에 대한 사칙연산
  - 포인터가 가리키는 객체단위로 계산
```
int A[5];
int *p;
p = &A[2];
p-1;
p+1;

int 배열 선언 = int A[5];
| A[0] | → p-2
| A[1] | → p-1
| A[2] | → p // p = &A[2];
| A[3] | → p+1 
| A[4] | → p+2
```
#### 포인터와 연산자
- 1. p // 포인터
- 2. *p // 포인터가 가리키는 값
- 3. *p++ // 가리키는 값을 가져온 다음, 포인터를 한 칸 증가
- 4. *p-- // 가리키는 값을 가져온 다음, 포인터를 한 칸 감소
- 5. (*p)++ // 포인터가 가리키는 값을 증가

### 포인터와 배열
- 배열의 이름
  - 사실상의 포인터와 같은 역할
  - 컴파일러가 배열의 이름을 배열의 첫 번째 주소로 대치
    배열 이름 : 상수 포인터(즉, 주소를 바꿀 수 없음)
```
배열 포인터
int A[5];

| A[0] | → A
| A[1] | 
| A[2] | 
| A[3] |  
| A[4] | 
```
```
int *p1, *p2, *p3, *p4, *p5;
int *p[5];

| 포인터 | 포인터 | 포인터 | 포인터 | 포인터 |
   p[0]    p[1]     p[2]    p[3]    p[4]

int a = 3;
p[1] = &a;
printf("%d", *p[1]);

int b = 10;
p[3] = &b;
printf("%d", *p[3]);
```

### 포인터와 구조체
```
typedef struct {
	int degree;
	float coef[MAX_DEGREE];
} Polynomial;
polynomial* top = NULL;

Polynomial a;
Polynomial* p;
p = &a;
a.degree = 5;
p->coef[0] = 1;
```
```
풀이:
a : 구조체 변수
p : Polynomial 구조체를 가리키는 포인터
p = &a; : p가 a의 주소를 저장
p는 a를 가리키게 됨
a.degree = 5; → "a라는 구조체 안의 degree 멤버에 5를 넣어라"는 뜻

구조체 포인터는 왜 '.'이 아니라 '->'를 쓰냐?
→ p는 구조체 그 자체가 아니라 구조체의 주소를 들고 있는 포인터
예시:
Polynomial *p;
p = &a;
→ 이때 p는 그냥 주소값일 뿐 그래서 p.degree라고 하면 안됨
→ degree라는 멤버는 주소 안에 있는 구조체에 속한 거지, 포인터 변수 p 자체에 있는 게 아님
→ 그래서 역참조를 해야 함
(*p).degree = 5;
→ *p : p가 가리키는 실제 구조체를 꺼냄
→ .degree : 그 구조체의 degree 멤버 접근
이런 과정이 불편해서 축약
p->degree = 5; 라고 씀
p->degree = 5; = (*p).degree = 5;
```
- 구조체 멤버의 접근
  - 구조체 : "." 연산자
  - 포인터 : "->" 연산자
- 
| 구조체를 이용한 표현 | 구조체를 이용한 표현 | 포인터를 이용한 표현 | 포인터를 이용한 표현 |
|----------|----------|-------------|----------|
| a.degree | (&a)->degree | p->degree | (*p).degree |
| a.coef[0] | (&a)->coef[0] | p->coef[0] | (*p).coef[0] |

### 자기참조 구조체
```
typedef struct ListNode {
	char data[10];
	struct ListNode* link;
} Node;
Node* top = NULL;
```
- 포인터는 NULL로 초기화
```
int* pi = NULL;
```
- 초기화가 안 된 포인터 변수 → 접근하면 안됨
```
char* pc; // 포인터 pc는 초기화가 안 되어 있음
*pc = 'a'; // 매우 위험한 코드
```
- 포인터 사이의 변환에는 명시적인 형 변환
```
void *p;
pi = (int *) p;
```
- 포인터의 종류
```
void *p; // p는 아무것도 가리키지 않는 포인터
int *pi; // pi는 정수 변수를 가리키는 포인터
float *pf; // pf는 실수 변수를 가리키는 포인터
char *pc; // pc는 문자 변수를 가리키는 포인터
int **pp; // pp는 포인터를 가리키는 포인터
test *ps; // test 타입의 객체를 가리키는 포인터
void (*f)(int); // f는 함수를 가리키는 포인터
```

### 정적 메모리
- 정적 메모리 할당
  - 메모리의 크기는 프로그램이 시작하기 전에 결정
  - 실행 도중에 크기를 변경 ❌
  - 더 큰 입력이 들어온다면 처리하지 못함
  - 더 작은 입력이 들어온다면 메모리 공간 낭비
```
int i; // int형 변수 i를 정적으로 할당
int* p; // 포인터 변수 p를 정적으로 할당
int A[10]; // 길이가 10인 배열을 정적으로 할당
```
```
int n = 10;
int B[n]; // 컴파일 오류
```

### 동적 메모리 라이브러리 함수
- 실행 도중에 메모리를 할당 받는 것
  - 필요한 만큼만 할당을 받고 반납함
  - 메모리를 매우 효율적으로 사용가능
```
void* malloc(int size);
void* calloc(int num, int size);

void free(void* ptr)
```
- malloc(size) : size 바이트만큼 한 덩어리로 할당
  - 초기화 안됨 ❌
  - 쓰레기 값 들어있음
```
예시:
Node* p = (Node*)malloc(sizeof(Node));
→ Node 하나 공간 생성
```
- calloc(num, size) : num개 X size크기 만큼 할당
  - 초기화 됨 ✅
  - 전부 0으로 채워짐
```
예시:
int* arr = (int*)calloc(5, sizeof(int));
→ int 5개짜리 배열 생성
```
- 
| 구분 | malloc | calloc |
|-----|--------|---------|
| 인자 | size | num, size |
| 의미 | 총 바이트 | 개수 x 크기 |
| 초기화 | ❌ | ✅(0으로) |
| 속도 | 빠름 | 조금 느림 |

- 동적 메모리 할당과 해제
```
int* pi = (int*)malloc(sizeof(int)*10);
Polynomial* pa = (Polynomial*)malloc(sizeof(Polynomial));
pi[3] = 10; // 동적 할당된 메모리를 배열처럼 사용
pa->degree = 5; // 동적 할당된 다항식 구조체의 멤버 변경
...
free(pi);
free(pa);
```

### 포인터의 응용 : 연결된 표현
- Linked Representation ↔ 배열
  - 항목들을 노드(node)라고 하는 곳에 분산하여 저장
  - 다음 항목을 가리키는 주소도 같이 저장
    - 노드(node) : <항목, 주소> 쌍
    - 항목 : data / 주소 : link
  - 노드는 데이터 필드와 링크 필드로 구성 : | data | link |→
                                     데이터 필드 | 링크 필드
    - 데이터 필드 : 리스트의 원소, 즉 데이터 값을 저장하는 곳
    - 링크 필드 : 다른 노드의 주소값을 저장하는 장소(포인터)
  - 메모리안에서의 노드의 물리적 순서가 리스트의 논리적 순서와 일치할 필요 없음
    - | data | link |→ | data | link |→ | data | link |→ | data | link |→
- 연결된 표현의 장단점
  - 장점
    - 삽입, 삭제가 보다 용이함
    - 연속된 메모리 공간이 필요없음
    - 크기 제한이 없음 
  - 단점
    - 구현이 어려움
    - 오류가 발생하기 쉬움
```
삽입 연산 그림
          ↗| N |↘
| A | → | B | × | C | → | D | → | E |

삭제 연산 그림
          ↑→→→→→→→→→→→→→→→↓
| A | → | B | × | C | × | D | → | E |
```

### 연결 리스트
- 연결 리스트의 구조
  - 노드(node)
```
    | data | link |→
     ↗        ↖
데이터 필드   링크 필드
```
- 헤드 포인터(head pointer)
  - 리스트의 첫 번째 노드의 주소를 저장하는 포인터
```
↓헤드 포인터
|  A  | |→  |  B  | |→  |  C  | |→  NULL
```
- 연결 리스트의 종류
  - 단순 연결 리스트
```
헤드포인터→  |    |  |→  |    |  |→  |    |  |→  |    |  |→  NULL
```
  - 원형 연결 리스트
```
헤드포인터→  |    |  |→  |    |  |→  |    |  |→  |    |  |
            ↑←←←←←←←←←←←←←←←←←←←←←←←←←←←←←←←←←←←←←←←←←←←↓
```
  - 이중 연결 리스트
    - 각각의 노드는 선행 노드, 후속 노드를 모두 가리킴
```
헤드포인터→  | |   | |→| |   | |→| |   | |→ ...  | |   | |→  NULL
           ←| |   | |←| |   | |←| |   | |← ... ←| |   | |
데이터구조 05장포인터와연결리스트 pg21 참고
```
- 연결 리스트로 구현한 스택
  -노드 구조체
```
typedef int Element;
typedef struct LinkedNode {
	Element data;
	struct LinkedNode* link;
} Node;
```
  - 배열과 연결리스트를 이용한 스택의 비교
```
Element data[MAX_STACK_SIZE]
int top;

   4 |     |
   3 |     |
   2 |  C  | ← top
   1 |  B  |
   0 |  A  |
배열을 이용한 스택

Node* top;
      top
        ↘
      |  C  | |
           ↙
      |  B  | |
           ↙
      |  A  | |
           ↙
         NULL
연결 리스트를 이용한 스택
Node* top = NULL;

+정보 : -top은 head pointer
```

### Node* p = (Node*)malloc(sizeof(Node));
- 구조체를 동적으로 생성해서 포인터로 다루는 코드

- Node?
```
typedef struct node {
	char data[10];
	struct node* link;
} Node;
```
  - Node : 구조체 이름
  - data : 값 저장
  - link : 다음 노드 주소 저장
  - char data[10];
    struct node* link;
    | data | link | 연결리스트 한 칸(노드)을 의미
```
Node* p
```
  - p = Node 구조체를 가리키는 포인터
    - Node 하나를 가리킬 주소 변수
```
malloc(sizeof(Node))
```
  - heap 메모리에 공간을 만들어달라는 뜻
    - sizeof(Node) : Node 구조체 크기만큼
    - malloc() : 그 크기만큼 메모리 할당
      - Node 하나 들어갈 공간을 heap에 생성
```
(Node*)
```  
  - malloc은 원래 void* 타입을 반환함
    - Node* 타입으로 쓰라고 변환함
```
전체 해석:
Node 하나 들어갈 공간을 heap에 만들고,
그 주소를 p에 저장

쓰는 이유:
연결리스트 만들기 위해
p->data = 10;
p->link = NULL;
Node 하나 만들어서 값 넣고 다음 연결 준비
```

### 연결된 스택의 연산
#### is_empty() 연산
- top이 NULL값을 갖는 경우

#### is_full() 연산
- 더 이상 불필요

#### push() 연산
- 새로운 노드를 스택에 삽입
```
예시 : 새로운 D노드를 스택에 삽입
top→|  C  | |→|  B  | |→|  A  | |→NULL
    ↘   ↖
p→|  D  | |
```
```
1. 노드 D의 링크 필드가 노드 C를 가리키도록 함 : p->link = top;
2. 헤더 포인터 top이 노드 D를 가리키도록 함 : top = p;
```
```
top→|  C  | |→|  B  | |→|  A  | |→NULL
    ↘   ↖
p→|  D  | |
```
```
void push( Node *p )
{
  p->link = top;
  top = p;
}

풀이:
이미 만들어진 Node p를 스택에 넣는 함수
p->link = top; 새 노드 p가 기존 top을 가리키게 함
기존:
- top → A → B → C
p 생성됨
- p → A → B → C
- top = p; 이제 p가 맨 위가 됨
- top → p → A → B → C
```
```
void push(Element e)
{
  Node* p = (Node*)malloc(sizeof(Node));
  p->data = e;

  p->link = top;
  top = p;
}

풀이:
값 e를 받아서 → 노드를 만들고 → 스택에 넣는 함수
Node* p = (Node*)malloc(sizeof(Node)); 힙에 노드 하나 생성
데이터 넣기
- p->data = e; 값 저장
연결
- p->link = top; 기존 top을 뒤로 밀기
top 갱신
- top = p; 새 노드를 맨 위로
기존:
- top → A → B → C
push(10):
- top → [10] → A → B → C
```
- 
| 구분 | push(Node* p) | push(Element e) |
|-----|----------------|-----------------|
| 입력 | 노드 자체 | 값 |
| 역할 | 연결만 함 | 생성 + 연결 |
| 사용 상황 | 이미 노드 있을 때 | 일반적인 push |
- 연결 리스트의 is_full()
  - 배열이 아니라서 공간이 고정 ❌
  - 연결리스트는 고정된 값이 아님 그래서 is_full() 불필요
  - 필요할 때마다 malloc()으로 계속 생성 가능
  - 연결리스트의 포화는 "malloc 실패" 딱 한 경우
```
연결리스트의 push연산 최종

void push(Element e) 
{   
    Node* p = (Node*)malloc(sizeof(Node));

    if(p == NULL)
        error("메모리 할당 실패"); // 포화 체크
    p->data = e;
    p->link = top;
    top = p;
}
```

#### pop() 연산
```
1. 포인터 변수 p가 노드 C를 가리키도록 함 : p = top;
2. 헤더 포인터 top이 B를 가리키도록 함 : top = p->next;
3. 마지막으로 변수 P의 값을 반환함 : return p;
```
```
예시:
    ↑→→→→→→→→→↓
top↛|  C  | |→|  B  | |→|  A  | |→NULL
 ↗
p
```
```
Node* pop()
{
   Node* p;
   if(is_empty()) error("에러");

   p = top;
   top = p->ilnk;

   return p;
}

풀이:
메모리 해제 ❌
노드 자체를 꺼내서 반환
동작 흐름
- top → A → B → C
p = top; p가 A를 가리킴
top = p->link; top이 B로 이동
- top → B → C
- p → A
return p; A 노드를 통째로 넘김
```
```
Element pop()
{
   Node* p;
   Element e;
   if(is_empty()) error("스택공백에러");

   p = top;
   top = p->link;

   e = p->data;
   free(p); // 노드 동적 해제
   return e;
}

풀이:
메모리 해제 ✅
데이터만 꺼내고 노드는 삭제
동작 흐름
- top → A → B → C
p = top; A 잡음
top = p->link; top → B
e = p->data; 값만 복사
free(p); A 메모리 삭제
```
- 
| 구분 | Node* pop() | Element pop() |
|-----|--------------|---------------|
| 반환값 | 노드 전체 | 데이터만 |
| free | ❌ 없음 | ✅ 있음 |
| 책임 | 호출자가 free | 함수 내부에서 처리 |
| 사용 목적 | 구조 자체 다룰 때 | 일반 스택 사용 |
- Node* pop()
  - 연결리스트 자체를 조작할 때
  - 노드를 다른 곳에 재사용할 때
- Element pop()
  - 일반적인 스택(값만 필요할 때)
  - 제일 많이 쓰는 방식
```
pop() 연산 최종 코드

Element pop()
{
   Node* p;
   Element e;
   if(is_empty()) error("스택공백에러");

   p = top;
   top = p->link;

   e = p->data;
   free(p); // 노드 동적 해제
   return e;
}
```

#### 연결된 스택의 기타 연산들
- peek()
  - 반드시 먼저 공백상태를 검사
- destory_stack()
  - 동적 할당을 사용해서 사용이 다 끝나면 할당한 횟수와 동일하게 해제 함수를 호출해야 함
  - 스택이 공백상태가 아닐 동안 계속 pop() 연산으로 노드를 꺼내고, 해제
```
peek() 연산 코드

Element peek()
{
    if(is_empty())
        error("스택공백에러");

    return top->data;
}

풀이:
스택 맨 위 값 확인만 하고, 제거는 안 함
동작 흐름
- top → A → B → C
peek()
- A를 반환
- 스택은 그대로 유지
- top → A → B → C   (변화 없음)
```
```
destory_stack() 연산 코드

void destroy_stack()
{
    Node* p;

    while (!is_empty()) {
        p = top;
        top = p->link;
        free(p);
    }
}

풀이:
스택 전체를 싹 비우고 메모리까지 전부 해제
동작 흐름
- top → A → B → C
p = top; A 잡음
top = p->link; top → B
free(p); A 삭제
이걸 반복하면 : B 삭제 → C 삭제
- 최종 : top = NULL (빈 스택)
```
- 
| 함수 | 역할 | 스택 변화 |
|------|------|-----------|
| peek() | 값 확인 | ❌ 변화 없음 |
| destroy_stack() | 전체 삭제 | ✅ 완전히 비워짐 |

#### 전체 노드 방문 연산
- size() 함수
  - top부터 하나씩 다음 노드를 따라가면서 몇 개의 노드가 연결되었는지 확인해야 함
```
top→|  A  | |→|  B  | |→|  C  | |→|  D  | |→NULL

int size()
{
    Node *p;
    int count = 0;

    for(p = top; p != NULL; p = p->link)
        count++;

    return count;
}

풀이:
현재 스택에 들어있는 노드 개수 ( = 데이터 개수) 세는 함수
동작 원리
- 연결리스트는 배열처럼 길이를 바로 알 수 없음
- 그래서 처음부터 끝까지 순회하면서 하나씩 세야 함
포인터 준비
- Node *p; 순회용 포인터(top부터 따라갈 용도)
카운트 변수
- int count = 0; 노드 개수 저장
반복문
- for(p = top; p != NULL; p = p->link)
      count++;
  - p = top; 시작(맨 위)
  - p != NULL; 끝까지 갈 때까지 반복
  - p = p->link; 다음 노드로 이동
순회 과정
- p=A (count=1)
- p=B (count=2)
- p=C (count=3)
- p=NULL → 종료
결과 반환
- return count; 총 개수 반환
```
- 
| 구조 | size 구하는 방법 |
|------|------------------|
| 배열 스택 | 변수 하나로 바로 O(1) |
| 연결리스트 | 순회 필요 O(n) |

### 연결된 스택의 프로그램 구현
여기부터 정리 필요
