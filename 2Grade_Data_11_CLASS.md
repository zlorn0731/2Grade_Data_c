# 📚 2학년 데이터구조 (Data Structures)
## 10장 : 우선순위 큐

### 실생활에서의 우선순위
- 도로에서의 자동차 우선순위
- 컴퓨터
  - 운영체제에서 시스템 프로세스는 응용 프로세스보다 더 높은 우선순위를 가짐

### 우선순위 큐
- Priority queue
  - 우선순위를 가진 항목들을 저장하는 큐
  - 우선 순위가 높은 데이터가 먼저 나가게 됨
  - 가장 일반적인 큐로 생각할 수 있음
    - 스택이나 큐를 우선순위 큐로 구현할 수 있음
    - 
    | 자료구조 | 삭제되는 요소 |
    |---------|---------------|
    | 스택 | 가장 최근에 들어온 데이터 |
    | 큐 | 가장 먼저 들어온 데이터 |
    | 우선순위 | 가장 우선순위가 높은 데이터 |
  - 응용 분야
    - 시뮬레이션, 네트워크 트래픽 제어, OS의 작업 스케줄링 등

### 우선순위 큐의 추상 자료형(ADT)
- 데이터
  - 우선순위를 가진 요소들의 모음
- 연산
  - Init() : 우선순위 큐를 초기화
  - Insert(item) : 우선순위 큐에 항목 item을 추가
  - Delete() : 가장 우선순위가 높은 요소를 꺼내서 반환
  - Find() : 가장 우선순위가 높은 요소를 삭제하지 않고 반환
  - Is_empty() : 우선순위 큐가 공백상태인지를 검사
  - Is_full() : 우선순위 큐가 포화상태인지를 검사

### 우선순위 큐 구현방법
- 구현 방법 : 배열, 연결 리스트, 힙(heap)
- 배열을 이용한 구현
```
                   count↓
 [0] [1] [2] [3]  [4]  [5] [6] [7] [8] [9]
| 2 | 5 | 7 | 10 | 12 |   |   |   |   |   |
```
- 연결 리스트를 이용한 구현
```
헤드포인터→| 12 | |→| 10 | |→| 7 | |→| 5 | |→| 2 | |→NULL
```
- 힙(heap)을 이용한 구현
  - 완전이진트리의 일종
  - 우선순위 큐를 위해 만들어진 자료구조
  - 일종의 반 정렬 상태를 유지

### 우선순위 큐 구현방법 비교
| 표현 방법 | 삽입 | 삭제 |
|-----------|------|------|
| 정렬 안 된(순서 없는) 배열 | O(1) | O(n) |
| 정렬 안 된(순서 없는) 연결 리스트 | O(1) | O(n) |
| 정렬된 배열 | O(n) | O(1) |
| 정렬된 연결 리스트 | O(n) | O(1) |
| 힙 | O(logn) | O(logn) |
- (예시) n이 1024일 때,
  - O(n) : 1024초, O(logn) : 10초

### 힙(heap)이란?
- Heap
  - 더미
  - 완전이진트리
  - 여러 개의 값들 중에서 가장 큰 값이나 가장 작은 값을 빠르게 찾아내도록 만들어진 자료구조
    - 최대 힙(max heap)
      - 부모 노드의 키 값이 자식 노드의 키 값보다 크거나 같은 완전 이진 트리
      - Key(부모 노드) ≥ Key(자식 노드)
    - 최소 힙(min heap)
      - 부모 노드의 키 값이 자식 노드의 키 값보다 작거나 같은 완전 이진 트리
        - Key(부모 노드) ≤ Key(자식 노드)

### 최대 힙과 최소 힙
- 중복된 값을 허용
- 느슨한 정렬 상태
- 힙의 목적 :
  - 삭제 연산에서 가장 큰 값을 효율적으로 찾기만 하면 되는 것
- 최대 힙(max heap)
  - Key(부모 노드) ≥ Key(자식 노드)
  ```
               (9)
             ↙    ↘
           (7)       (6)
         ↙  ↘      ↙  ↘
       (5)    (4)  (3)    (2)
     ↙  ↘     ↘  
   (2)   (1)    (3)
  ```
- 최소 힙(min heap)
  - Key(부모 노드) ≤ Key(자식 노드)
  ```
               (1)
             ↙    ↘
           (4)       (2)
         ↙  ↘      ↙  ↘
       (7)    (5)  (3)    (3)
     ↙  ↘     ↘  
   (7)   (8)    (9)
  ```

### 힙의 높이
- n개의 노드를 가지고 있는 힙의 높이는 O(logn)
  - 힙은 완전 이진트리
  - 마지막 레벨을 제외하고 각 레벨 i에 2ⁱ-1개의 노드 존재
```
[깊이] [노드의 개수]
  1       1=2⁰ -------------------- (9)
                                  ↙    ↘
  2       2=2¹ ---------------- (7)       (6)
                              ↙  ↘      ↙  ↘
  3       4=2² ------------ (5)    (4)  (3)    (2)
                           ↙  ↘     ↘  
  4        3 ------------ (2)   (1)    (3)
```

### 힙의 구현 : 배열을 이용
- 힙은 보통 배열을 이용하여 구현
  - 완전이진트리 → 각 노드에 번호를 붙임 → 배열의 인덱스
```
[배열을 이용한 힙의 구현]

               (9)1
             ↙    ↘
           (7)2      (6)3
         ↙  ↘      ↙  ↘
       (5)4   (4)5 (3)6   (2)7
     ↙  ↘     ↘  
   (2)8  (1)9   (3)10

0  |     |
1  |  9  |
2  |  7  |
3  |  6  |
4  |  5  |
5  |  4  |
6  |  3  |
7  |  2  |
8  |  2  |
9  |  1  |
10 |  3  |
11 |     |
```
- 부모 노드와 자식 노드의 관계
  - 왼쪽 자식의 인덱스 = (부모의 인덱스) * 2
  - 오른쪽 자식의 인덱스 = (부모의 인덱스) * 2 + 1
  - 부모의 인덱스 = (자식의 인덱스) / 2
- (예시) 정수 저장 힙
  - 저항할 항목의 자료형 : HNode
  - Key(n) : 힙 노드 n의 우선순위를 반환
```
typedef int HNode; // 힙에 저장할 항목의 자료형
#define Key(n) (n) // 힙 노드 n의 키값

HNode heap[MAX_HEAP_NODE]; // 배열을 이용해 구현한 힙(힙노드 배열)
int heap_size; // 힙의 크기
```
- i번째 부모 노드와 왼쪽 자식, 오른쪽 자식
```
#define Parent(i) (heap[i/2]) // i의 부모 노드
#define Left(i)   (heap[i*2]) // i의 왼쪽 자식 노드
#define Right(i)  (heap[i*2+1]) // i의 오른쪽 자식 노드
```
- 프로그램 10.1 : 배열을 이용한 힙의 기본 틀
```
#include <stdio.h>
#include <stdlib.h>
#define MAX_HEAP_NODE 200

void error(char str[]) {...} // 프로그램 3.1 함수와 동일
typedef int HNode;
#define Key(n) (n) // 힙 노드 n의 키 값

HNode heap[MAX_HEAP_NODE]; // 배열을 이용해 구현한 힙
int heap_size;
#define Parent(i) (heap[i/2])
#define Left(i)   (heap[i*2])
#define Right(i)  (heap[i*2+1])
void init_heap()       { heap_size = 0; }
int is_empty_heap()    { return heap_size == 0; }
// 배열 0번 요소를 사용하지 않음
int is_full_heap()     { return (heap_size == MAX_HEAP_NODE - 1); }
HNode find_heap()      { return heap[1]; }
```

### 힙의 연산 : 삽입 연산
- 삽입 연산
- Up-heap
  - 회사에서 신입 사원이 들어오면 일단 말단 위치에 앉힘
  - 신입 사원의 능력을 봐서 위로 승진시킴
  - (1) Heap에 새로운 요소가 들어 오면, 일단 새로운 노드를 heap의 마지막 노드에 이어서 삽입
  - (2) 삽입 후에 새로운 노드를 부모 노드들과 교환해서 heap의 성질을 만족 -> up-heap

### Up-heap
- 삽입된 노드에서 루트까지의 경로에 있는 노드들을 비교/교환
- 힙의 성질을 복원
  - 키 k가 부모노드보다 작거나 같으면 upheap는 종료
- 알고리즘 10.1 : 최대 힙에서의 삽입 알고리즘
```
insert(node)

heapSize ← heapSize + 1;
i ← heapSize;
heap[i] ← node;
while i ≠ 1 and KEY(heap[i]) > KEY(Parent(i)) do
      heap[i] ↔ Parent(i);
      i ← Parent(i);
```
- 프로그램 10.2 : 최대 힙 트리의 삽입 함수
```
void insert_heap(HNode n)
{
  int i;
  if (is_full_heap()) return;
  i = ++(heap_size);
  while (i != 1 && Key(n) > Key(Parent(i))) {
          heap[i] = Parent(i);
          i /= 2; // 한 레벨 증가
  }
  heap[i] = n;
}
```

### 삭제 연산
- 최대 힙에서의 삭제 → 항상 루트가 삭제됨
  - 가장 큰 키 값을 가진 노드를 삭제하는 것
- 방법 : down-heap
  - 루트 삭제
  - 회사에서 사장의 자리가 비게 됨
  - 말단 사원을 사장 자리로 올림
  - 능력에 따라 강등 반복

### Downheap 
- 알고리즘 10.2 : 힙 트리에서의 삭제 알고리즘
```
remove()

item ← A[1];
A[1] ← A[heapSize];
heapSize ← heapSize - 1;
i ← 2;
while i ≤ heapSize do
      if i < heapSize and A[LEFT(i)] > A[RIGHT(i)]
            then largest ← LEFT(i);
            else largest ← RIGHT(i);
      if A[PARENT(largest)] > A[largest]
            then break;
      A[PARENT(largest)] ↔ A[largest];
      i ← LEFT(largest); return item;
```
- 프로그램 10.3 : 최대 힙 트리의 삭제 함수
```
HNode delete_heap()
{
  HNode hroot, last;
  int parent = 1, child = 2;
  if (is_empty_heap()) error("힙 트리 공백 에러");
  hroot = heap[i]; // root 반환
  last = heap[heap_size--];
  while (child <= heap_size) {
      // 현재 노드의 자식들 중에서 더 큰 자식을 찾음
      if (child < heap_size && Ket(Left(parent)) < Key(Right(parent)))
              child++; // 더 큰 자식의 인덱스
      if (Key(last) >= Key(heap[child]))
              break;
      heap[parent] = heap[child];
      parent = child;
      child *= 2; // 한 단계 아래로 이동
  }
  heap[parent] = last; // 마지막 노드를 최종 위치에 저장
  return hroot;
}
```

### 전체 프로그램
- 프로그램 10.4 : 최대 힙 트리 테스트 프로그램
```
      .... // 프로그램 10.1 추가
void insert_heap(HNode n) {...} // 프로그램 10.2 함수와 동일
HNode delete_heap() {...} // 프로그램 10.3 함수와 동일

void print_heap()
{
  int i, level;
  for (i = 1, level = 1; i <= heap_size; i++) {
          if (i == level) {
                  printf("\n");
                  level *= 2;
          }
          printf("%2d", Key(heap[i]));
  }
  printf("\n-----------");
}

void main()
{
  init_heap();
  insert_heap(2);    insert_heap(5);
  insert_heap(4);    insert_heap(8);
  insert_heap(9);    insert_heap(3);
  insert_heap(7);    insert_heap(3);
  print_heap();

  delete_heap();     print_heap();
  delete_heap();     print_heap();
  printf("\n");
}
```

### 힙의 복잡도 분석
- 삽입 연산에서 최악의 경우
  - 루트 노드까지 올라가야 하므로 트리의 높이에 해당하는 비교 연산 및 이동 연산이 필요
    - O(log₂n)
- 삭제 연산 최악의 경우
  - 가장 아래 레벨까지 내려가야 하므로 역시 트리의 높이 만큼의 시간이 걸림
    - O(log₂n)

### 힙 정렬
- 힙을 이용하면 정렬 가능 : 힙 정렬
  - 먼저 정렬해야 할 n개의 요소들을 최대 힙에 삽입
  - 한번에 하나씩 요소를 힙에서 삭제하여 저장하면 됨
  - 삭제되는 요소들은 값이 증가되는 순서(최소 힙의 경우)
- 시간 복잡도 : O(nlog₂n)
  - 하나의 요소의 삽입 삭제가 O(log₂n)
  - 요소의 개수가 n개 →O(nlog₂n)
  - 대부분의 간단한 정렬 알고리즘 : O(n²)
- 특히 유용한 경우
  - 전체의 정렬이 아니라 가장 큰 값 몇 개만 필요할 때임
- 프로그램 10.5 : 힙 정렬 프로그램
```
- 배열을 오름차순으로 정렬

void insert_heap(HNode n) {...} // 프로그램 10.2 함수와 동일
HNode delete_heap() {...} // 프로그램 10.3 함수와 동일

void print_array(int a[], int n, char* msg)
{
  int i;
  printf("%10s: ", msg);
  for (i = 0; i < n; i++)
          printf("%3d", a[i]);
  printf("\n");
}

void main()
{
  int i, data[10];
  for (i = 0; i < 10; i++)
      data[i] = rand() % 100; // 난수 배열 생성
  print_array(data, 10, "정렬 전");

  init_heap();

  for (i = 0; i < 10; i++) 
       insert_heap(data[i]); // 배열 항목들을 힙에 넣음

  for (i = 9; i >= 0; i--)
       data[i] = Key(delete_heap()); // 힙에서 꺼내 배열에 다시 저장

  print_array(data, 10, "정렬 후");
}
```

### 허프만 코드
- 이진 트리는 각 글자의 빈도가 알려져 있는 메시지의 내용을 압축하는데 사용될 수 있음
- 이런 종류의 이진트리 → 허프만 코딩 트리
- 압축
  - 높은 빈도수의 글자
    - 적은 비트수 부여
  - 낮은 빈도수의 글자
    - 많은 비트수 부여
- (예시) e와 z만 표현
  - 모든 문자를 7비트로 표현 : 7bit* (e 123회 + z 1회)
    - 868 bits
  - e는 2비트로, z를 20비트로 표현 : 2bit (e 123회) + 20bit* (z 1회)
    - 266bits
- ASCII 코드
  - 모든 문자를 동일한 비트수로 표현

### 문자의 빈도수
- 빈도수가 알려진 문자에 대한 고정길이코드와 가변길이코드의 비교
- 
| 글자 | 빈도수 | 고정길이코드 |  |  | 가변길이코드 |  |  |
|------|------:|------|------:|------:|------|------:|------:|
|      |       | 코드 | 비트수 | 전체 비트수 | 코드 | 비트수 | 전체 비트수 |
| A | 17 | 0000 | 4 | 68 | 00 | 2 | 34 |
| B | 3 | 0001 | 4 | 12 | 11110 | 5 | 15 |
| C | 6 | 0010 | 4 | 24 | 0110 | 4 | 24 |
| D | 9 | 0011 | 4 | 36 | 1110 | 4 | 36 |
| E | 27 | 0100 | 4 | 108 | 10 | 2 | 54 |
| F | 5 | 0101 | 4 | 20 | 0111 | 4 | 20 |
| G | 4 | 0110 | 4 | 16 | 11110 | 5 | 20 |
| H | 13 | 0111 | 4 | 52 | 010 | 3 | 39 |
| I | 15 | 1000 | 4 | 60 | 110 | 3 | 45 |
| J | 1 | 1001 | 4 | 4 | 11111 | 5 | 5 |
| **합계** | **100** |  |  | **400** |  |  | **292** |
- (예시) 코드 읽기, "FACE"
```
                 F      A      C      E
고정길이코드 : | 0101 | 0000 | 0010 | 0100 |

                 F     A     C     E
가변길이코드 : | 0111 | 00 | 0110 | 10 |
```
- 코드 읽고 쓰기
  - 고정 길이 코드 : 4비트씩 끊어서 적거나 읽기
  - 가변 길이 코드
    - 한 비트씩 읽으면서 코드 테이블에 코드가 있으면 한 문자로 처리
    - 코딩된 비트열은 정확히 하나의 코드만일치 -> 이런 종류의 코드를 허프만 코드
   
### 허프만 코드 생성 절차
- 1단계 : 각 문자별로 노드를 생성, 노드의 값은 빈도수가 됨
- 2단계 : 가장 작은 빈도수의 루트 2개를 묶어 이진트리를 구성
  - 이 때 루트의 값 : 자식 노드의 값의 합
- 3단계 : 남은 트리에서 가장 작은 빈도수의 루트를 2개 찾아 묶어 이진트리를 구성
- 4단계 : 남은 트리에 대해 동일한 처리
- 5단계 : 마지막으로 최종 허프만 트리 1개가 됨
- 6단계 : 코드 할당, 왼쪽 간석 : 1, 오른쪽 간선 : 0

### 허프만 코딩 트리 생성 프로그램
- 최소 힙 사용
  - 여러 문자의 빈도수들 중에서 가장 작은 2개를 찾는 과정
- 과정
  - make_tree()
    - 각 문자의 빈도수를 입력 받아 모든 노드를 힙에 삽입
  - 다음
    - 현재 힙에서 최소 노드 2개를 뽑고 이들을 묶어 하나의 노드를 다시 힙에 삽입하는 과정을 반복
- 프로그램 10.6 : 허프만 코딩 트리 생성 프로그램
```
            ..... // 프로그램 10.1 : 코드 추가, 배열을 이용한 힙의 기본 틀
void insert_heap(HNode n)
{
  int i;
  if (is_full_heap()) return;
  i = ++(heap_size);
  while (i != 1 && Key(n) < Key(Parent(i))) {
          heap[i] = Parent(i);
          i /= 2;
}

HNode delete_heap()
{
  HNode hroot, last;
  int parent = 1, child = 2;
  if (is_empty_heap())
        error("힙 트리 공백 에러");

  hroot = heap[1];
  last = heap[heap_size--];
  while (child <= heap_size) {
          if (child < heap_size && Key(Left(parent)) > Key(Right(parent)))
                      child++;
          if (Key(last) <= Key(heap[child]))
                      break;
          heap[parent] = heap[child];
          parent = child;
          child *= 2;
  }
  heap[parent] = last;
  return hroot;
}

void make_tree(int freq[], int n)
{
  HNode e1, e2;
  int i;
  init_heap();
  for (i = 0; i < n; i++)
        insert_heap(freq[i]); // 빈도수
  for (i = 1; i < n; i++) {
        e1 = delete_heap();
        e2 = delete_heap();
        insert_heap(Key(e1) + Key(e2));
        printf("  (%d+%d)\n", Key(e1), Key(e2));
  }
}

int main()
{
  char label[] = { 'A', 'B', 'C', 'D', 'E' };
  int freq[] = { 15, 12, 8, 6, 4 };
  make_tree(freq, 5);
}
```

##### ✍️작성자: 박지안
##### 🐧실습 환경: Visual Studio 2022
##### 🗓️ 작업일: 2026-06-07
