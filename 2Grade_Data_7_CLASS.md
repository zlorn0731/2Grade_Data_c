# 📚 2학년 데이터구조 (Data Structures)
## 6장 : 리스트 31pg부터 끝까지

### 헤드 포인터와 헤드 노드
- 맨 앞 노드의 까다로운 처리
- 단순화 방법
  - 헤드 노드(head node)
  ```
  Node org; // 헤드 노드 org. 실제 헤드 포인터는 org.link가 됨
  ```
  ```
  (a) 헤드 포인터를 사용하여 구현한 리스트

  헤드포인터(head)
                →| A | |→| B | |→| C | |→| D | |→NULL
  ----------------------------------------------------
  (b) 헤드 노드를 사용하여 구현한 리스트

  헤드 노드(org)
             | ? | |→| A | |→| B | |→| C | |→| D | |→NULL
              ↗     ↖
  낭비되는 데이터필드  실제 해드 포인터 = org.link
  ```
- 헤드 노드를 사용
  - 모든 노드가 선행 노드를 가짐
    - insert(pos, e), delete(pos) 연산이 간단해짐
    - get_entry(-1) 연산
      - org 주소를 반환하도록 수정
```
// 이중 연결리스트를 이용한 리스트 프로그램
Node org; // head node
Node* get_head() { return org.next; }
Node* get_entry(int pos) {
    Node* n = &org;
    int i = -1; // 단순 연결리스트는?
    for (i = -1; i < pos; i++, n = n->next) // p=p->link;
        if (n == NULL) break;
    return n;
}
```

#### 이중 연결 리스트를 이용한 리스트 프로그램
```
void insert_next(Node* before, Node* node)
{ ... }

void insert(int pos, Element val)
{
    Node* new_node, * prev;

    prev = get_entry(pos - 1);
    if (prev != NULL) {
        new_node = (Node*)malloc(sizeof(Node));
        new_node->data = val;
        new_node->prev = NULL;
        new_node->next = NULL;

        insert_next(prev, new_node);
    }
}
```

### 원형 연결 리스트
- Circular Linked List
  - 리스트의 마지막 노드의 링크가 첫 번째 노드를 가리키는 연산
  - 장점
    - 한 노드에서 다른 모든 노드로의 접근 가능
    - 하나의 노드는 결국 모든 노드를 거쳐 자기 자신으로 되돌아 올 수 있다
    - 리스트의 끝에 노드를 삽입하는 연산 : 단순 연결리스트보다 효율적
  - 구조
  ```
  [원형 연결 리스트의 구조]
  헤드포인터→|   | |→|   | |→|   | |→|   | |
  ```
  ```
  head
   | 40 | |→| 10 | |→| 20 | |→| 30 | |
   ↑←←←←←←←←←←←←←←←←←←←←←←←←←←←←←←←←↓
  ```
- 분야
  - 하나의 CPU를 여러 응용 프로그램이 이용하여 실행할 대
    - 프로세스(process)
      - 실행 중인 프로그램
    - 고정된 시간 슬롯
    - 시분할 시스템
      - (time sharing system)
    - 멀티 플레이어 게임
    - 원형 큐를 만드는데 사용
- 변형된 원형 연결 리스트
  - 헤드 포인터
    - 마지막 노드를 가리킴
  - 리스트의 처음이나 마지막에 노드를 삽입하는 연산이 단순 연결 리스트에 비하여 용이
```
[변형된 연결 리스트]
         ↗→→→→→→→→→→→→→→→→→→→→→→→→↓
헤드포인터→/|   | |→|   | |→|   | |→|   | |
           ↑←←←←←←←←←←←←←←←←←←←←←←←←←←←←↓
```
```
                            head
| 40 | |→| 10 | |→| 20 | |→| 30 | |
↑←←←←←←←←←←←←←←←←←←←←←←←←←←←←←←←←↓
```

### 이중 연결 리스트
- Doubly Linked List
  - 양방향 링크 필드 : 양방향 검색이 가능
- 선행 노드를 알 수는 없을까?
```
[단순 연결 리스트에서의 선행 노드를 찾는 것이 어렵다.]

헤드포인터→|   | |→|   | |→|   | |→ ``` |   | |→NULL
```
```
[이중 연결 리스트의 구조]

헤드포인터 ⇄| | | |⇄| | | |⇄| | | |→ ``` ←| | | |→NULL
```

### 이중 연결 리스트 구조
```
[이중 연결 리스트에서의 노드 구조]

            링크 필드
            ↙     ↘
          prev     next
선행 노드 ←| | data | |→ 후속 노드
     데이터 필드↗
```
```
typedef int Element
typedef struct DblLinkedNode {
    Element data;
    struct DblLinkedNode* prev;
    struct DblLinkedNode* next;
} Node;
```
- 단점
  - 공간을 많이 차지
  - 복잡한 코드
- 실제 응용
  - 이중 연결 리스트와 원형 연결 리스크를 혼합한 형태가 많이 사용
  ```
  헤드포인터 ⇄| | | |⇄| | | |⇄| | | |→ ``` ←| | | |→NULL
  ```
- 다음 관계가 항상 성립
  - p == p->next->prev == p->prev->next
    - p->next->prev : 자기 자신을 가리킴
    - p->prev->next : 자기 자신을 가리킴
  ```
  헤드포인터 ⇄| | | |⇄| | | |⇄| | | |
  ```
  - 공백 리스트에서도 성립

### 삽입 연산
- 어떤 노드(before) 다음에 새로운 노드 N을 추가할 경우

#### 이중 연결리스트의 노드 삽입 과정
```
void insert_next(Node* before, Node* n)
{
    if (n != NULL) {
        n->prev = before;
        n->next = before->next;
        if (before->next != NULL)
            before->next->prev = n;
        before->next = n;
    }
}
```

### 삭제 연산
- 모든 노드는 자신의 선행 노드를 알 수 있다.
  - 삭제 함수 이름이 remove_next()일 필요가 없다.

### 이중 연결리스트의 노드 삭제 과정
```
void remove_curr(Node* n)
{
    if (n->prev != NULL)
        n->prev->next = n->next;
    if (n->next != NULL)
        n->next->prev = n->prev;
}
```

#### 이중 연결리스트를 이용한 리스트 프로그램
```
