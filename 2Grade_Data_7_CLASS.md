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
#include <stdio.h>
#include <stdlib.h>

typedef int Element;

typedef struct DblLinkedNode {
    Element data;
    struct DblLinkedNode* prev;
    struct DblLinkedNode* next;
} Node;

Node org;

void init_list() {
    org.prev = NULL;
    org.next = NULL;
}

Node* get_head() {
    return org.next;
}

int is_empty() {
    return get_head() == NULL;
}

Node* get_entry(int pos) {
    Node* n = &org;

    for (int i = -1; i < pos; i++, n = n->next) {
        if (n == NULL)
            break;
    }

    return n;
}

void replace(int pos, Element e) {
    Node* node = get_entry(pos);

    if (node != NULL)
        node->data = e;
}

Node* find(Element e) {
    Node* p;

    for (p = get_head(); p != NULL; p = p->next) {
        if (p->data == e)
            return p;
    }

    return NULL;
}

int size() {
    Node* p;
    int count = 0;

    for (p = get_head(); p != NULL; p = p->next)
        count++;

    return count;
}

void insert_next(Node* before, Node* node) {
    if (node != NULL) {
        node->prev = before;
        node->next = before->next;

        if (before->next != NULL)
            before->next->prev = node;

        before->next = node;
    }
}

void insert(int pos, Element val) {
    Node* new_node;
    Node* prev;

    prev = get_entry(pos - 1);

    if (prev != NULL) {
        new_node = (Node*)malloc(sizeof(Node));

        new_node->data = val;
        new_node->prev = NULL;
        new_node->next = NULL;

        insert_next(prev, new_node);
    }
}

void remove_curr(Node* node) {
    if (node == NULL)
        return;

    if (node->prev != NULL)
        node->prev->next = node->next;

    if (node->next != NULL)
        node->next->prev = node->prev;

    free(node);
}

void delete(int pos) {
    Node* node = get_entry(pos);

    if (node != NULL)
        remove_curr(node);
}

void clear_list() {
    while (!is_empty())
        delete(0);
}

void print_list(char* msg) {
    Node* p;

    printf("%s\n", msg);

    for (p = get_head(); p != NULL; p = p->next)
        printf("%d ", p->data);

    printf("\n\n");
}

int main() {
    init_list();

    insert(0, 10);
    insert(0, 20);
    insert(1, 30);
    insert(size(), 40);
    insert(2, 50);

    print_list("이중연결리스트로 구현한 List(삽입x5)");

    replace(2, 90);
    print_list("이중연결리스트로 구현한 List(교체x1)");

    delete(2);
    delete(size() - 1);
    delete(0);

    print_list("이중연결리스트로 구현한 List(삭제x3)");

    clear_list();
    print_list("이중연결리스트로 구현한 List(정리후)");

    return 0;
}

- 결과
이중연결리스트로 구현한 List(삽입x5)
20 30 50 10 40

이중연결리스트로 구현한 List(교체x1)
20 30 90 10 40

이중연결리스트로 구현한 List(삭제x3)
30 10

이중연결리스트로 구현한 List(정리후)
```

### 연결 리스트의 응용 : 라인 편집기
- 헤드 포인터를 이용
```
            Line Editor
                   ↘
Contents           |   Contents  |  ↓   |
Basic              |    Basic    |  ↓   |
Linked list   ↔    | Linked list |  ↓   |
Stack              |    Stack    |  ↓   |
Queue              |    Queue    | NULL |
[문서]            [문서의 내부적인 표현]
```

### 라인 편집기 기능
- (1) 한 라인 삽입 : 행 번호와 문자열을 입력
- (2) 한 라인 삭제 : 행 번호를 입력
- (3) 한 라인 변경 : 행 번호와 문자열을 입력
- (4) 현재 내용 출력 : 현재 모든 행을 출력
- (5) 파일 입력 : 지정된 파일 읽기
- (6) 파일 출력 : 지정된 파일로 저장
```
typedef struct Line {
    char str[MAX_CHAR_PER_LINE];
} Line;

typedef Line Element;
```

#### File 관련 C 문법
```
[예시]

int main(void) {
    char name[20];
    FILE* fp;

    fp = fopen("Test.txt", "w");

    if (fp == NULL)
        printf("파일이 열리지 않습니다.!\n");

    printf("이름 입력하세요 : ");
    scanf("%s", name);

    fprintf(fp, "방가방가. 나의 이름은 %s야.", name);

    fclose(fp);
    return 0;
}
----------------------------------------------------------
FILE* fp;
fp = fopen("Test.txt", "r");
if (fp != NULL)
{
    while (fgets(line.str, MAX_CHAR_PER_LINE, fp))
        insert(size(), line);
    fclose(fp);
}
----------------------------------------------------------
FILE* fp;
fp = fopen("Test.txt", "w");
if (fp != NULL)
{
    display(fp);
    fclose(fp);
}
----------------------------------------------------------
fprintf(stderr, "%3d: ", i);
fprintf(fp, "%s", p->data.str);
----------------------------------------------------------
fgets(line.str, MAX_CHAR_PER_LINE, stdin);
```

#### 리스트를 이용한 라인 편집기 프로그램 - 과제
```
#define _CRT_SECURE_NO_WARNINGS
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

#define MAX_WORD 100
#define MAX_MEANING 200

typedef struct Word {
    char eng[MAX_WORD];
    char kor[MAX_MEANING];
    int favorite;   // 즐겨찾기
} Word;

typedef Word Element;

typedef struct LinkedNode {
    Element data;
    struct LinkedNode* link;
} Node;

Node* head = NULL;

void init_list(void) { head = NULL; }
int is_empty(void) { return head == NULL; }

Node* get_entry(int pos) {
    Node* p = head;
    for (int i = 0; i < pos && p != NULL; i++)
        p = p->link;
    return p;
}

int size(void) {
    Node* p = head;
    int count = 0;
    while (p) { count++; p = p->link; }
    return count;
}

void insert(int pos, Element val) {
    if (pos < 0 || pos > size()) {
        printf("삽입 위치 오류\n");
        return;
    }

    Node* new_node = (Node*)malloc(sizeof(Node));
    new_node->data = val;
    new_node->link = NULL;

    if (pos == 0) {
        new_node->link = head;
        head = new_node;
    }
    else {
        Node* prev = get_entry(pos - 1);
        new_node->link = prev->link;
        prev->link = new_node;
    }
}

void append(Element val) {
    insert(size(), val);
}

void delete_word(int pos) {
    if (is_empty()) return;

    Node* removed;

    if (pos == 0) {
        removed = head;
        head = head->link;
    }
    else {
        Node* prev = get_entry(pos - 1);
        removed = prev->link;
        prev->link = removed->link;
    }
    free(removed);
}

void display(void) {
    Node* p = head;
    int i = 0;

    printf("\n===== 단어장 =====\n");
    while (p) {
        printf("%d. %s : %s %s\n",
            i,
            p->data.eng,
            p->data.kor,
            p->data.favorite ? "★" : "");
        p = p->link;
        i++;
    }
}

void find_word(char* word) {
    Node* p = head;
    int i = 0;

    while (p) {
        if (strcmp(p->data.eng, word) == 0) {
            printf("찾음 [%d] %s : %s\n", i, p->data.eng, p->data.kor);
            return;
        }
        p = p->link;
        i++;
    }
    printf("없음\n");
}

void set_favorite(int pos) {
    Node* p = get_entry(pos);
    if (p) {
        p->data.favorite = 1;
        printf("즐겨찾기 설정 완료\n");
    }
}

void show_favorites(void) {
    Node* p = head;
    printf("\n=== 즐겨찾기 목록 ===\n");

    while (p) {
        if (p->data.favorite)
            printf("%s : %s\n", p->data.eng, p->data.kor);
        p = p->link;
    }
}

void sort_list(void) {
    Node* p;
    Node* q;
    Element temp;

    for (p = head; p != NULL; p = p->link) {
        for (q = p->link; q != NULL; q = q->link) {
            if (strcmp(p->data.eng, q->data.eng) > 0) {
                temp = p->data;
                p->data = q->data;
                q->data = temp;
            }
        }
    }
    printf("정렬 완료\n");
}

void clear_list(void) {
    while (!is_empty())
        delete_word(0);
}

int main(void) {
    char command;
    Word word;
    int pos;
    char search[MAX_WORD];

    init_list();

    do {
        printf("\n[i:위치삽입 a:자동삽입 d:삭제 p:출력 f:검색 s:정렬 k:즐겨찾기 v:즐겨찾기보기 q:종료] ");
        scanf(" %c", &command);

        switch (command) {

        case 'i':
            printf("위치: ");
            scanf("%d", &pos);

            printf("영어: ");
            scanf("%s", word.eng);

            printf("뜻: ");
            scanf("%s", word.kor);

            word.favorite = 0;
            insert(pos, word);
            break;

        case 'a':  // 자동 삽입
            printf("영어: ");
            scanf("%s", word.eng);

            printf("뜻: ");
            scanf("%s", word.kor);

            word.favorite = 0;
            append(word);
            break;

        case 'd':
            printf("삭제 위치: ");
            scanf("%d", &pos);
            delete_word(pos);
            break;

        case 'p':
            display();
            break;

        case 'f':
            printf("검색 단어: ");
            scanf("%s", search);
            find_word(search);
            break;

        case 's':
            sort_list();
            break;

        case 'k':
            printf("즐겨찾기 위치: ");
            scanf("%d", &pos);
            set_favorite(pos);
            break;

        case 'v':
            show_favorites();
            break;

        case 'q':
            printf("종료\n");
            break;

        default:
            printf("잘못된 입력\n");
        }

    } while (command != 'q');

    clear_list();
    return 0;
}
```

##### ✍️작성자: 박지안
##### 🐧실습 환경: Visual Studio 2022
##### 🗓️ 작업일: 2026-05-28
