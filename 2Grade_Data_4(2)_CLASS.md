# 📚 2학년 데이터구조 (Data Structures)
## 4장 : 덱

### 덱(deque)이란?
- 덱(deque)은 double-ended-queue의 줄임말
- 큐의 전단(front)와 후단(rear)에서 모두 삽입과 삭제가 가능한 큐를 의미
- 중간에 삽입하거나 삭제하는 것은 허용하지 않음
```
덱의 구조
            add_Front ↘  -----------------------------  ↙ add_Rear(enqueue
delete_Front(dequeue) →       |  A  |  B  |  C  |        ←  delete_Rear
            get_Front ↗  -----------------------------  ↖  get_Rear
                     전단(front)                    후단(rear)
```

### 덱의 연산
- 
| 연산             | 의미     |
| -------------- | ------ |
| init()         | 초기화    |
| add_front(x)   | 앞에 삽입  |
| add_rear(x)    | 뒤에 삽입  |
| delete_front() | 앞에서 삭제 |
| delete_rear()  | 뒤에서 삭제 |
| get_front()    | 앞 확인   |
| get_rear()     | 뒤 확인   |
| is_empty()     | 비었는지   |
| is_full()      | 꽉 찼는지  |
| size()         | 개수     |
```
예시:
초기 상태 [ ]
add_front(A) [ A ]
add_rear(B) [ A B ]
add_front(C) [ C A B ]
delete_rear() [ C A ]
delete_front() [ A ]
앞/뒤 자유롭게 조작 가능
```
- delete_rear(), add_front()에서는 원형 큐에서와 다르게 반대방향 회전이 필요
- front, rear 감소 시켜야 하는데, 만약 음수가 되면 MAX_QUEUE_SIZE를 더해줘야 함
```
front ← (front -1 + MAX_QUEUE_SIZE) % MAX_QUEUE_SIZE;
rear ← (rear -1 + MAX_QUEUE_SIZE) % MAX_QUEUE_SIZE;
```

### 프로그램 4.2 : 원형 덱 프로그램
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
void init_deque() { init_queue(); }
void add_rear(Element e) { enqueue(e); }
Element delete_front() { return dequeue(); }
Element get_front() { return peek(); }

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

Element add_front(Element e)
{
	if (is_full())
		error("덱 포화 에러");
	data[front] = e;
	front = (front - 1 + MAX_QUEUE_SIZE) % MAX_QUEUE_SIZE;
}

Element delete_rear()
{
	int prev = rear;
	if (is_empty())
		error("덱 공백 에러");
	rear = (rear - 1 + MAX_QUEUE_SIZE) % MAX_QUEUE_SIZE;
	return data[prev];
}

Element get_rear()
{
	if (is_empty())
		error("덱 공백 에러");
	return data[rear];
}

void print_deque(char msg[])
{
	print_queue(msg);
}

void main()
{
	int i;

	init_deque();
	for (i = 0; i < 10; i++) {
		if (1 % 2)
			add_front(i);
		else
			add_rear(i);
	}
	print_deque("원형 덱 홀수-짝수 ");
	printf("\tdelete_front() --> %d\n", delete_front());
	printf("\tdelete_front() --> %d\n", delete_front());
	printf("\tdelete_front() --> %d\n", delete_front());
	print_deque("원형 덱 삭제-홀짝홀");
}
```
### 프로그램 4.2 : 원형 덱 프로그램 설명
- 원형 큐 코드를 바탕으로 원형 덱(deque) 기능을 추가한 코드

#### 덱용 이름 바꾼 함수들
```
void init_deque() { init_queue(); }
void add_rear(Element e) { enqueue(e); }
Element delete_front() { return dequeue(); }
Element get_front() { return peek(); }
```

### 큐의 응용 : 은행 시뮬레이션 
- 시뮬레이션 : 모의실험
- 은행 시뮬레이션
  - 큐잉 이론에 따라 시스템의 특성을 시뮬레이션하여 분석하는데 이용
  - 큐잉 이론 : 대기자 수와 대기 시간의 관계를 수학적으로 분석한 이론
  - 고객이 들어와서 서비스를 받고 나가는 과정을 시뮬레이션
    - 고객들이 기다리는 평균 대기 시간을 계산
- 조건
  - 대기 행렬은 큐로 구현
    - 큐의 고객들은 순서대로 서비스를 받음
  - 한 고객의 서비스가 끝나면 다음 고객이 서비스를 받음
  - 가정 : 은행 창구는 하나
    - 서비스 인원(은행원) : 1명
- 입력
  - 시뮬레이션 할 최대 시간 (예시: 10 [단위시간])
  - 단위시간에 도착하는 고객 수 (예시: 0.5 [고객수/단위시간])
  - 한 고객에 대한 최대 서비스 시간 (예시: 5 [단위시간/고객])
- 출력
  - 고객들의 평균 대기 시간
 
### 프로그램 4.3 : 은행 시뮬레이션 프로그램
```
#define _CRT_SECURE_NO_WARNINGS
#include <stdio.h>
#include <stdlib.h>
#include <time.h>
#define MAX_QUEUE_SIZE 100

typedef struct BankCustomer {
    int id;         // 고객 번호
    int tArrival;   // 도착 시간
    int tService;   // 서비스에 필요한 시간
} Customer;

typedef Customer Element;

Element data[MAX_QUEUE_SIZE];
int front;
int rear;

int nSimulation;        // 시뮬레이션 시간
double probArrival;     // 단위시간에 도착하는 평균 고객 수
int tMaxService;        // 한 고객에 대한 최대 서비스 시간

int totalWaitTime;      // 전체 대기 시간
int nCustomers;         // 전체 고객 수
int nServedCustomers;   // 서비스 받은 고객 수

void error(char str[])
{
    printf("%s\n", str);
    exit(1);
}

void init_queue()
{
    front = rear = 0;
}

int is_empty()
{
    return (front == rear);
}

int is_full()
{
    return ((rear + 1) % MAX_QUEUE_SIZE == front);
}

int size()
{
    return (rear - front + MAX_QUEUE_SIZE) % MAX_QUEUE_SIZE;
}

void enqueue(Element val)
{
    if (is_full())
        error("큐 포화 에러");

    rear = (rear + 1) % MAX_QUEUE_SIZE;
    data[rear] = val;
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

double rand0to1()
{
    return rand() / (double)RAND_MAX;
}

void insert_customer(int arrivalTime)
{
    Customer a;

    a.id = ++nCustomers;
    a.tArrival = arrivalTime;
    a.tService = (int)(tMaxService * rand0to1()) + 1;

    printf("고객 %d 방문(서비스 시간:%d분)\n", a.id, a.tService);

    enqueue(a);
}

void read_sim_params()
{
    printf("시뮬레이션 할 최대 시간 (예:10) = ");
    scanf("%d", &nSimulation);

    printf("단위시간에 도착하는 고객 수 (예:0.5) = ");
    scanf("%lf", &probArrival);

    printf("한 고객에 대한 최대 서비스 시간 (예:5) = ");
    scanf("%d", &tMaxService);

    printf("=============================================\n");
}

void run_simulation()
{
    Customer a;
    int clock = 0;
    int serviceTime = -1;   

    init_queue();
    nCustomers = 0;
    totalWaitTime = 0;
    nServedCustomers = 0;

    while (clock < nSimulation) {
        clock++;
        printf("현재시각=%d\n", clock);

        if (rand0to1() < probArrival)
            insert_customer(clock);

        if (serviceTime > 0) {
            serviceTime--;
        }
        else {
            if (is_empty())
                continue;

            a = dequeue();
            nServedCustomers++;
            totalWaitTime += (clock - a.tArrival);

            printf("고객 %d 서비스 시작 (대기시간:%d분)\n",
                a.id, clock - a.tArrival);

            serviceTime = a.tService - 1;
        }
    }
}

void print_result()
{
    printf("=================================================\n");
    printf("서비스 받은 고객수 = %d\n", nServedCustomers);
    printf("전체 대기 시간 = %d분\n", totalWaitTime);

    if (nServedCustomers > 0)
        printf("서비스고객 평균대기시간 = %-5.2f분\n",
            (double)totalWaitTime / nServedCustomers);
    else
        printf("서비스고객 평균대기시간 = 0.00분\n");

    printf("현재 대기 고객 수 = %d\n", nCustomers - nServedCustomers);

	printf("\n\n 20230844 박지안");
}

int main()
{
    srand((unsigned int)time(NULL));

    read_sim_params();
    run_simulation();
    print_result();

    return 0;
}
```

### 프로그램 4.4 : 미로 탐색 프로그램
```
#include <stdio.h>
#include <stdlib.h>

#define MAX_QUEUE_SIZE 100
#define MAZE_SIZE 6

typedef struct {
    int x;
    int y;
} Location2D;

typedef Location2D Element;

// 미로
char map[MAZE_SIZE][MAZE_SIZE] = {
    { '1', '1', '1', '1', '1', '1' },
    { 'e', '0', '1', '0', '0', '1' },
    { '1', '0', '0', '0', '1', '1' },
    { '1', '0', '1', '0', '1', '1' },
    { '1', '0', '1', '0', '0', 'x' },
    { '1', '1', '1', '1', '1', '1' },
};

// 원형 덱용 배열과 front, rear
Element data[MAX_QUEUE_SIZE];
int front, rear;

// 에러 처리
void error(char str[])
{
    printf("%s\n", str);
    exit(1);
}

// 원형 큐/덱 기본 함수
void init_queue() { front = rear = 0; }
int is_empty() { return front == rear; }
int is_full() { return (rear + 1) % MAX_QUEUE_SIZE == front; }
int size() { return (rear - front + MAX_QUEUE_SIZE) % MAX_QUEUE_SIZE; }

// 큐 연산
void enqueue(Element val)
{
    if (is_full())
        error("큐 포화 에러");
    rear = (rear + 1) % MAX_QUEUE_SIZE;
    data[rear] = val;
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

// 덱 연산
void init_deque() { init_queue(); }
void add_rear(Element val) { enqueue(val); }
Element delete_front() { return dequeue(); }
Element get_front() { return peek(); }

void add_front(Element val)
{
    if (is_full())
        error("덱 포화 에러");
    data[front] = val;
    front = (front - 1 + MAX_QUEUE_SIZE) % MAX_QUEUE_SIZE;
}

Element delete_rear()
{
    Element ret;
    if (is_empty())
        error("덱 공백 에러");
    ret = data[rear];
    rear = (rear - 1 + MAX_QUEUE_SIZE) % MAX_QUEUE_SIZE;
    return ret;
}

Element get_rear()
{
    if (is_empty())
        error("덱 공백 에러");
    return data[rear];
}

// 좌표 생성 함수
Location2D getLocation2D(int x, int y)
{
    Location2D p;
    p.x = x;
    p.y = y;
    return p;
}

// 갈 수 있는 위치인지 확인
int is_valid(int x, int y)
{
    if (x < 0 || y < 0 || x >= MAZE_SIZE || y >= MAZE_SIZE)
        return 0;
    else
        return map[y][x] == '0' || map[y][x] == 'x';
}

// DFS: 덱을 스택처럼 사용
int DFS()
{
    int x, y;
    Location2D here;

    init_deque();
    add_rear(getLocation2D(0, 1));   // 입구

    printf("DFS: ");
    while (!is_empty()) {
        here = delete_rear();        // 스택의 pop처럼 사용
        printf("(%2d,%2d) ", here.x, here.y);

        x = here.x;
        y = here.y;

        if (map[y][x] == 'x')
            return 1;
        else {
            map[y][x] = '.';

            if (is_valid(x - 1, y)) add_rear(getLocation2D(x - 1, y));
            if (is_valid(x + 1, y)) add_rear(getLocation2D(x + 1, y));
            if (is_valid(x, y - 1)) add_rear(getLocation2D(x, y - 1));
            if (is_valid(x, y + 1)) add_rear(getLocation2D(x, y + 1));
        }
    }
    return 0;
}

// BFS: 덱을 큐처럼 사용
int BFS()
{
    int x, y;
    Location2D here;

    init_deque();
    add_rear(getLocation2D(0, 1));   // 입구, enqueue

    printf("BFS: ");
    while (!is_empty()) {
        here = delete_front();       // dequeue
        printf("(%2d,%2d) ", here.x, here.y);

        x = here.x;
        y = here.y;

        if (map[y][x] == 'x')
            return 1;
        else {
            map[y][x] = '.';

            if (is_valid(x - 1, y)) add_rear(getLocation2D(x - 1, y));
            if (is_valid(x + 1, y)) add_rear(getLocation2D(x + 1, y));
            if (is_valid(x, y - 1)) add_rear(getLocation2D(x, y - 1));
            if (is_valid(x, y + 1)) add_rear(getLocation2D(x, y + 1));
        }
    }
    return 0;
}

// 미로 원상복구용
void reset_map()
{
    char original[MAZE_SIZE][MAZE_SIZE] = {
        { '1', '1', '1', '1', '1', '1' },
        { 'e', '0', '1', '0', '0', '1' },
        { '1', '0', '0', '0', '1', '1' },
        { '1', '0', '1', '0', '1', '1' },
        { '1', '0', '1', '0', '0', 'x' },
        { '1', '1', '1', '1', '1', '1' },
    };

    int i, j;
    for (i = 0; i < MAZE_SIZE; i++)
        for (j = 0; j < MAZE_SIZE; j++)
            map[i][j] = original[i][j];
}

int main()
{
    reset_map();
    printf("-> %s\n", DFS() ? "성공" : "실패");

    reset_map();
    printf("-> %s\n", BFS() ? "성공" : "실패");

    return 0;
}
```

##### ✍️작성자: 박지안
##### 🐧실습 환경: Visual Studio 2022
##### 🗓️ 작업일: 2026-03-31
