# 📚 2학년 데이터구조 (Data Structures)
## 4장 : 큐

### 큐(queue)란?
- 먼저 들어온 데이터가 먼저 나가는 자료구조
  - 선입선출(FIFO : First-In First-Out)
  - 예시) 은행 대기줄, 매표소 등
 
### 큐의 구조
- 큐에서 삽입이 일어나는 곳 후단 : rear
- 큐에서 삭제가 일어나는 곳 전단 : front
  - 큐에서는 삽입과 삭제가 후단과 전단에서 각각 이루어지므로 양쪽의 위치를 기억
  - 두개의 변수가 필요 / int rear;, int front;
```
--------------------------------
  ⬅️  |  A  |  B  |  C  |  ⬅️  
--------------------------------
전단(front)              후단(rear)
```

### 큐의 연산
- 
| 연산         | 의미     |
| ---------- | ------ |
| init()     | 초기화    |
| enqueue(x) | 삽입     |
| dequeue()  | 삭제     |
| peek()     | 맨 앞 확인 |
| is_empty() | 비었는지   |
| is_full()  | 꽉 찼는지  |

```
큐의 삽입과 삭제 연산들
-----------------------------------
enqueue(A) :       [A]          ⬅️
enqueue(B) :       [A][B]       ⬅️
enqueue(C) :       [A][B][C]    ⬅️
dequeue() :  ⬅️    [B][C]              
dequeue() :  ⬅️    [C]          
```

### 큐의 활용 - 버퍼(buffer)
- 버퍼 : 잠깐 저장해두는 대기 공간
  - 어떤 데이터가 들어오는 속도와 그 데이터를 처리하는 속도가 서로 다를 때 중간에서 잠깐 받아두는 공간
- 왜 버퍼가 필요?
```
예시 1:
키보드는 사람이 입력하니까 느리게 들어옴
CPU는 엄청 빠르게 처리함
예시 2:
컴퓨터는 프린터로 데이터를 빠르게 보냄
프린터는 종이에 실제로 인쇄해야 해서 느림

문제 1: 들어오는 데이터를 바로 처리 못함
데이터가 한꺼번에 들어오는데 처리 장치가 느리면 일부 데이터가 날아갈 수 있음
문제 2: 처리 장치가 계속 기다려야 함
처리 장치가 빠른데 데이터가 천천히 오면 다믐거는 언제 오는지 모름
```
- 큐가 버퍼에 왜 딱일까?
  - 버퍼에 들어온 데이터는 보통 들어온 순서대로 처리
```
예시:
1. 문서 A 인쇄 요청
2. 문서 B 인쇄 요청
3. 문서 C 인쇄 요청
먼저 들어온 데이터가 먼저 처리되어야 함
FIFO 형식 = 큐의 방식
```

### 선형 큐 - 배열을 이용
- 정수의 1차원 배열 필요
- 삽입과 삭제 연산을 위한 변수 필요
  - 삽입 변수 : rear - 큐의 마지막 요소
  - 삭제 변수 : front - 큐의 첫 번째 요소
```
(a) 초기 상태
   [-1]  [0]   [1]   [2]   [3]   [4]
      --------------------------------
       |     |     |     |     |     | 
      --------------------------------
   ↑  ↑
front rear
```
```
(b) enqueue(3):
   [-1]  [0]   [1]   [2]   [3]   [4]
      --------------------------------
       |  3  |     |     |     |     | 
      --------------------------------
   ↑     ↑
front   rear
```
```
(c) enqueue(7):
   [-1]  [0]   [1]   [2]   [3]   [4]
      --------------------------------
       |  3  |  7  |     |     |     | 
      --------------------------------
   ↑            ↑
front          rear
```
```
(d) enqueue(5):
   [-1]  [0]   [1]   [2]   [3]   [4]
      --------------------------------
       |  3  |  7  |  5  |     |     | 
      --------------------------------
   ↑                  ↑
front                rear
```
```
(e) dequeue():
   [-1]  [0]   [1]   [2]   [3]   [4]
      --------------------------------
       |     |  7  |  5  |     |     | 
      --------------------------------
          ↑           ↑
        front        rear
```
```
(f) dequeue():
   [-1]  [0]   [1]   [2]   [3]   [4]
      --------------------------------
       |     |     |  5  |     |     | 
      --------------------------------
                ↑     ↑
              front  rear
```
- 큐 객체가 생성되면 큐는 공백 상태가 되어야 하고 front, rear의 초기 값은 -1
- enqueue() 연산을 통해 데이터가 삽입되면 먼저 rear를 하나 증가하고 그 위치에 데이터를 저장
- 삭제시에는 front를 먼저 하나 증가시키고 front가 가리키는 위치에 있는 요소를 삭제


- 문제 발생 🚨 : front, rear의 값이 계속 증가만 함
- 언젠가는 배열의 끝에 도달하게 되고 배열의 앞부분이 비어 있더라도 더 이상 삽입하지 못한다는 점
- 후단에 더 이상 삽입할 공간이 없으면 모든 요소들을 왼쪽으로 이동시켜야 함
- 삭제 연산은 복잡도 O(1)로 일정한 시간을 보장, 삽입 연산의 시간 복잡도는 O(n) = 비효율
  - 배열을 선형으로 생각하지 않고 원형으로 생각하면 문제 해결 = 원형 큐
```
왼쪽으로 이동시킴
   [-1]  [0]   [1]   [2]   [3]   [4]                 [-1]  [0]   [1]   [2]   [3]   [4]
      --------------------------------                  --------------------------------
       |     |     |  5  |  8  |  3  |       →           |  5  |  8  |  3  |     |     |
      --------------------------------                  --------------------------------
                ↑                 ↑                     ↑               ↑
              front              rear                 front            rear
```

### 원형 큐
- front, rear의 값이 배열의 끝인 (MAX_QUEUE_SIZE-1)에 도달하면 다음에 증가되는 값은 0
  - 그림은 데이터구조 책 참고
- front, rear의 개념 변경
  - 초기 값은 -1이 아님, front와 rear가 같은 위치를 가리키기만 하면 됨
  - front는 항상 큐의 첫 번째 요소의 하나 앞
  - rear는 마지막으로 입력된 요소를 가리킴
```
예시:
1. front, rear은 처음에 모두 0을 가리킴
2. enqueue(A) 연산은 먼저 rear를 증가시키고 증가된 위치에 데이터 A를 삽입
3. enqueue(B) 연산을 통해 이제 rear는 2가 됨
4. 삭제 연산 dequeue()에서는 먼저 front를 증가시키고 증가된 위치에 데이터를 꺼냄
5. front는 1
6. 이런 식으로 삽입과 삭제를 아무리 반복하더라도 선형 큐에서와 같은 요소들의 이동 필요 없음
```
#### 공백상태와 포화상태
- front, rear의 값이 같으면 원형 큐가 비어 있는 공백 상태
- 포화 상태 : 원형 큐에서는 하나의 자리는 항상 비워두어야 함
  - 포화 상태와 공백 상태를 구별하기 위함
  - 비워 두지 않으면 공백 상태, 포화 상태를 구분 할 수 없음
 ```
front == rear : 공백 상태
front가 rear보다 하나 앞에 있으면 : 포화 상태
```

### 큐의 연산 알고리즘
#### 큐의 is_empty() 연산
```
is_empty()

if front == rear
    return TRUE
else
    return FALSE
```
```
실제 코드
int is_empty() {
    return (front == rear);
}
```
#### 큐의 is_full() 연산
- rear의 다음 위치는 (rear+1)를 MAX_QUEUE_SIZE로 나눈 나머지
```
is_full()

if front == (rear + 1) % MAX_QUEUE_SIZE
    return TRUE
else
    return FALSE
```
```
실제 코드
int is_full() {
    return (rear + 1) % MAX_QUEUE_SIZE == front;
}
```
#### 원형큐에서의 enqueue(x) 연산
```
enqueue(x)

if is_full()
    error "overflow"
else
    rear ← (rear + 1) % MAX_QUEUE_SIZE
    data[rear] ← x
```
```
실제 코드
void enqueue(Element val) {
    if (is_full())
        error("큐 포화 상태");
    rear = (rear + 1) % MAX_QUEUE_SIZE;
    data[rear] = val;
}
```
#### 원형큐에서의 dequeue() 연산
```
dequeue()

if is_empty()
    error "underflow"
else
    front ← (front + 1) % MAX_QUEUE_SIZE
    return data[front]
```
```
실제 코드
Element dequeue() {
    if (is_empty())
        error("큐 공백 상태");
    front = (front + 1) % MAX_QUEUE_SIZE;
    return data[front];
}
```
#### 원형 큐의 init() 연산
```
init()
front ← 0
rear ← 0
```
```
실제 코드
void init_queue() {
    front = rear = 0;
}
```
#### 원형 큐의 size() 연산
```
size()
return (rear - front + MAX_QUEUE_SIZE) % MAX_QUEUE_SIZE
```
```
실제 코드
int size() {
    return (rear - front + MAX_QUEUE_SIZE) % MAX_QUEUE_SIZE;
}
```

### 프로그램 4.1 : 원형 큐의 프로그램
```
#include <stdio.h>
#include <stdlib.h>
#define MAX_QUEUE_SIZE 100

typedef int Element;
Element data[MAX_QUEUE_SIZE];
int front;
int rear;


void error(char str[])
{
	printf("%s\n", str);
	exit(1);
}

void init_queue() { front = 0; rear = 0; }
int size() { return (rear - front + MAX_QUEUE_SIZE) % MAX_QUEUE_SIZE; }
int is_empty() { return front == rear; }
int is_full() { return front == (rear + 1) % MAX_QUEUE_SIZE; }

Element enqueue(Element e)
{
	if (is_full())
		error("큐 포화 에러");
	rear = (rear + 1) % MAX_QUEUE_SIZE;
	data[rear] = e;
}

Element dequeue()
{
	if (is_empty())
		error("큐 공백 에러");
	front = (front + 1) % MAX_QUEUE_SIZE;
	return data[front];
}

Element peek()
{
	if (is_empty())
		error("큐 공백 에러");
	return data[(front + 1) % MAX_QUEUE_SIZE];
}

void print_queue(char msg[])
{
	int i, maxi = rear;
	if (front >= rear)maxi += MAX_QUEUE_SIZE;
	printf("%s[%2d]= ", msg, size());
	for (i = front + 1; i <= maxi; i++)
		printf("%2d", data[i % MAX_QUEUE_SIZE]);
	printf("\n");
}

void main()
{
	int i;

	init_queue();
	for (i = 1; i < 10; i++)
		enqueue(i);
	print_queue("원형큐 enqueue 9회");
	printf("\tdequeue() --> %d\n", dequeue());
	printf("\tdequeue() --> %d\n", dequeue());
	printf("\tdequeue() --> %d\n", dequeue());
	print_queue("원형큐 dequeue 3회");

	printf("\n\n20230844 박지안");
}
```
### 프로그램 4.1 : 원형 큐 프로그램 설명
- 원형 큐를 배열로 구현한 코드

#### 전단 후단 선언
```
int front;
int rear;
```
- int front; : 첫 번째 원소 바로 앞 위치를 가리키는 변수
- int rear; : 마지막 원소 위치를 가리키는 변수

#### init 함수
```
void init_queue { front = 0; rear = 0; }
```
- 큐를 빈 상태로 만듦

#### size 함수
```
int size() { return (rear - front + MAX_QUEUE_SIZE) % MAX_QUEUE_SIZE; }
```
- 현재 큐 안에 들어있는 데이터 개수를 반환

#### is_empty() 함수
```
int is_empty() { return front == rear; }
```

#### is_full() 함수
```
int is_full() { return front == (rear + 1) % MAX_QUEUE_SIZE; }
```

#### enqueue(e) 함수
```
Element enqueue(Element e)
{
	if (is_full())
		error("큐 포화 에러");
	rear = (rear + 1) % MAX_QUEUE_SIZE;
	data[rear] = e;
}
```

#### dequeue() 함수
```
Element dequeue()
{
	if (is_empty())
		error("큐 공백 에러");
	front = (front + 1) % MAX_QUEUE_SIZE;
	return data[front];
}
```

#### peek() 함수
```
Element peek()
{
	if (is_empty())
		error("큐 공백 에러");
	return data[(front + 1) % MAX_QUEUE_SIZE];
}
```

#### print_queue 함수
```
void print_queue(char msg[])
{
	int i, maxi = rear;
	if (front >= rear)maxi += MAX_QUEUE_SIZE;
	printf("%s[%2d]= ", msg, size());
	for (i = front + 1; i <= maxi; i++)
		printf("%2d", data[i % MAX_QUEUE_SIZE]);
	printf("\n");
}
```
- 큐의 현재 상태를 출력
- int i, maxi = rear;
  - 반복문용 변수 i
  - maxi는 출력 끝 위치를 잡기 위한 변수
- if (front >= rear) maxi += MAX_QUEUE_SIZE; 🔥
  - 원형 큐에서는 rear가 front보다 작을 수 있음
```
예시:
front = 95;
rear = 3;
```
  - 이런 경우엔 rear가 앞쪽으로 돌아온 상태
  - 이때 그냥 for(i = front + 1; i <= rear; i++) 하면 반복이 안됨
  - 그래서 rear에 MAX_QUEUE_SIZE를 더해서 쭉 이어진 것처럼 만들어줌
- for(i = front + 1; i <= maxi; i++)
  - front 다음 칸부터 rear까지 출력
- data[i % MAX_QUEUE_SIZE] 🔥
  - 만약 i가 100,101처럼 넘어갔을 때
```
예시:
100 % 100 = 0
101 % 100 = 1
```
  - 이렇게 다시 처음 칸으로 돌아가게 됨
  - 즉 원형 구조를 출력에서도 똑같이 적용

#### main 함수
```
void main()
{
	int i;

	init_queue();
	for (i = 1; i < 10; i++)
		enqueue(i);
	print_queue("원형큐 enqueue 9회");
	printf("\tdequeue() --> %d\n", dequeue());
	printf("\tdequeue() --> %d\n", dequeue());
	printf("\tdequeue() --> %d\n", dequeue());
	print_queue("원형큐 dequeue 3회");
}
```
- init_queue();
  - 큐 초기화
  - front = 0, rear = 0
- for (i = 1; i < 10; i++) enqueue(i);
  - 1부터 9까지 넣음
```
큐 상태:
1 2 3 4 5 6 7 8 9
```

##### ✍️작성자: 박지안
##### 🐧실습 환경: Visual Studio 2022
##### 🗓️ 작업일: 2026-03-31
