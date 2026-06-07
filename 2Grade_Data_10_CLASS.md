# 📚 2학년 데이터구조 (Data Structures)
## 9장 : 이진 탐색 트리

### 이진 탐색 트리
- 탐색(search)은 가장 중요한 컴퓨터 응용의 하나
- 이진 탐색 트리(BST, Binary Search Tree)
  - 이진 트리 기반의 탐색을 위한 자료 구조
  - 효율적인 탐색 작업을 위한 자료 구조

### 탐색 관련 용어
- 레코드(record)
- 필드(field)
- 테이블(table)
- 키(key)
- 주요키(primary key)
```
          필드1   필드2   필드3
        ㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡ
레코드 1 | [학번] [이름] [주소] |
레코드 2 |                     |
레코드 3 |                     |
        |                     |
        |                     |
        ㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡ
                 테이블
```

### 이진탐색트리의 정의
- 탐색 작업을 효율적으로 하기 위한 자료구조
  - key(왼쪽 서브트리) ≤ key(루트 노드) ≤ key(오른쪽 서브트리)
  - 이진 탐색을 중위순회하면 오름차순으로 정렬된 값을 얻을 수 있음
```
               루트 노드
                 (18)
                ↙   ↘
  왼쪽        (7)      (26)         오른쪽
서브트리     ↙   ↘        ↘       서브트리
          (3)    (12)      (31)
                           ↙
                         (27)

왼쪽 서브트리의 노드의 키값 < 루트 노드의 키값 < 오른쪽 서브트리 노드의 키값
```

### 이진탐색트리의 연산
- 이진탐색트리는 이진 트리의 일종이므로 기본적인 연산은 이진 트리와 동일
  - 삽입(insert), 삭제(delete), 탐색(search)
  - 이진탐색트리의 조건을 유지하면서 처리되어야 함
- 탐색 연산
  - key를 키 값으로 가진 노드를 탐색
  - 탐색은 항상 루트 노드에서 시작
  - 루트 노드와의 비교 결과
    - 비교한 결과가 같으면 탐색이 성공적으로 끝남
    - 키 값이 루트보다 작으면 → 왼쪽 자식을 기준으로 다시 탐색
    - 키 값이 루트보다 크면 → 오른쪽 자식을 기준으로 다시 탐색
  - 루트 아래의 노드에서도 같은 과정을 되풀이함

#### 알고리즘 9.1 : 이진탐색트리의 탐색 알고리즘
```
search (root, key)

  if root = NULL
    then return NULL; // 탐색 실패
  if key = KEY(root) // 탐색 성공
    then return root;
  else if key < KEY(root)
    then return search(LEFT(root), key);
    else return search(RIGHT(root), key);
```

#### 프로그램 9.1 : 이진탐색트리의 순환적인 탐색 함수
```
TNode* search (TNode *n, int key)
{
  if (n == NULL) return NULL;
  else if (key == n->data) return n;
  else if (key < n->data) return search (n->left, key);
  else return search (n->right, key);
}
```

#### 프로그램 9.2 : 이진탐색트리의 반복적인 탐색 함수
```
TNode* search_iter (TNode *n, int key)
{
  while (n != NULL) {
            if (key == n->data) return n;
            else if (key < n->data) n = node->left;
            else n = node->right;
  }
  return NULL; // 탐색 실패
}
```
- 키가 아닌 다른 필드로 탐색 가능?
  - 가능, 탐색의 효율성은 떨어짐

### 이진탐색트리의 삽입 연산
- 노드를 삽입하기 전에 먼저 탐색을 수행
  - 중복된 키 값의 노드 : 삽입 불가
  - 탐색에 실패한 위치가 바로 새로운 노드를 삽입하는 위치
  - (예시) 노드 9를 삽입
  ```
          9 탐색
           (18)
          ↙   ↘
        (7)    (26)
      ↙   ↘      ↘
    (3)    (12)     (31)
           ↙        ↙
         NULL      (27)

  (a) 탐색을 먼저 수행
  ```
  ```
            9 삽입
           (18)
          ↙   ↘
        (7)    (26)
      ↙   ↘      ↘
    (3)    (12)     (31)
           ↙        ↙
         (9)       (27)
  (b) 탐색이 실패한 위치에 9를 삽입
  ```
- 알고리즘 9.2 : 이진탐색트리의 삽입 알고리즘
```
insert (root, n)

if KEY(n) == KEY(root)
    then return;
else if KEY(n) < KEY(root) then // root보다 키가 작으면 왼쪽
  if LEFT(root) = NULL // root의 왼쪽 자식이 없으면
      then LEFT(root) <- n; // n이 왼쪽 자식
  else insert(LEFT(root), n);
else
  if RIGHT(root) = NULL
      then RIGHT(root) <- n;
      else insert(RIGHT(root), n);
```
- 프로그램 9.3 : 이진탐색트리의 삽입 함수
```
void insert (TNode* r, TNode* n)
{
  if (n->data == r->data) return 0; // 삽입 실패
  else if (n->data < r->data) {
      if (r->left == NULL) r->left = n;
    else insert (r->left, n);
  }
  else {
    if (r->right == NULL) r->right = n;
    else insert (r->right, n);
  }
  return 1; // 삽입 성공
}
```

### 이진탐색트리의 삭제 연산
- 노드 삭제의 3가지 경우
  - CASE 1 : 단말 노드를 삭제하는 겨우
  - CASE 2 : 자식이 하나인 노드를 삭제하려는 경우
    - 삭제하려는 노드가 하나의 왼쪽이나 오른쪽 서브트리 중 하나만 가지고 있는 경우
  - CASE 3 : 삭제하려는 노드가 두 개의 자식(서브 트리)를 모두 가지고 있는 경우

#### CASE 1 : 단말 노드 삭제
- 단만 노드의 부모 노드를 찾아서 연결을 끊으면 됨
  - 실제로 변경되는 것이 부모의 링크 필드
- 프로그램 9.4 : 이진탐색트리의 삭제 함수의 일부
```
void delete(TNod *parent, TNode *node) {
        TNode *child, *succ, *succp;

        // case 1 : 단말 노드 삭제
        if ((node->left == NULL && node->right == NULL)) {
              if (parent == NULL) root = NULL; // 공백 트리가 됨
              else {
                      if (parent->left == node)
                            parent->left = NULL;
                      else parent->right = NULL;
               }
         }
```

#### CASE 2 : 자식이 하나인 노드 삭제
- 노드는 삭제하고 서브 트리는 부모 노드에 붙여줌
- 삭제할 때 고려 사항
  - 삭제할 노드의 자식이
    - 왼쪽 자식일 수도 있고 오른쪽 자식일 수도 있음
  - 삭제할 노드가 부모의
    - 왼쪽 자식일 수도 있고 오른쪽 자식일 수도 있음
- 프로그램 9.4 : 이진탐색트리의 삭제 함수
```
  // case 2 : 자식이 하나인 노드 삭제
  else if (node->left == NULL || node->right == NULL) {

  // child에 node의 유일한 주식 주소를 복사
  child = (node->left != NULL) ? node->left : node->right;
            if (node == root) root = child;
            else {
                    if (parent->left == node)
                          parent->left = child;
                    else parent->right = child;
            }
  }
```

#### CASE 3 : 두 개의 자식을 가진 노드 삭제
- 가장 비슷한 값을 가진 노드를 삭제 노드 위치로 가져옴
- 후계 노드의 선택
  - 삭제되는 노드와 값이 가장 비슷한 노드
  - 다른 노드를 이동시키지 않아도 이진탐색트리가 그대로 유지
  - 각각 삭제할 노드의 바로 앞과 뒤에 방문되는 노드
  - 삭제할 노드를 대신할 적절한 후계자 노드를 찾음
  - 노드를 삭제하는 대신에 후계자 노드를 삭제 위치로 복사
    - 링크를 복사하지 않고 노드의 데이터 영역만 복사하는 것
  - 마지막으로 후계자로 사용한 노드를 삭제
- 선택 가능한 두 개의 후계자 중에서 어떤 노드를 선택할 것인가?
  - 어는 것을 선택해도 상관없음
  - 삭제되는 노드의 오른쪽 서브트리에서 가장 작은 값을 갖는 노드는 오른쪽 서브 트리에서 왼쪽 자식 링크를 타고 NULL을 만날때까지 계속 진행
- 구현은 더 복잡
  - 삭제를 위해 더 많은 정보가 필요함
  - (예시) 노드 18를 삭제할 때
    - 후계자 노드 22의 그 부모 노드 26을 찾음
    - 후계자 노드 22를 삭제할 노드 18에 복사
    - 후계자의 부모 26의 왼쪽 자식을 후계자 노드 22의 오른쪽 자식으로 변경함
    - 후계자로 선택된 22의 오른쪽 자식이 있을 수 있음에 유의
    ```
    if (succp->left = succ)
          succp->left = succ->right;
    else succp->right = succ->right;

    node->data = succ->data; // 데이터 멤버만 복사
    node = succ; // succ 삭제
    }
    free(node);
    ```
    ```
    // case 3 : 두개의 자식을 가진 노드 삭제
      else {
          succp = node; // 부모 노드 초기화
          succ = node->right; // 후계자 노드 초기화
          while (succ->left != NULL) { // 오른쪽 서브트리의 가장 작은 노드를 찾아감
                  succp = succ;
                  succ = succ->left;
          }
          if (succp->left == succ)
                succp->left = succ->right;
          else succp->right = succ->right;
          node->data = succ->data; // 데이터 멤버만 복사
          node = succ; // succ 삭제
          }
      free(node);
    ```
    - 프로그램 9.4 : 이진탐색트리의 삭제 함수
    ```
    void delete(TNode *parent, TNode *node) { // 부모 노드와 삭제할 노드 주소
      TNode *childe, *succ, *succp;
      // case 1
      if ((node->left == NULL && node->right == NULL)) {
            if (parent == NULL) root = NULL;
            else {
                    if (parent->left == node) parent->left = NULL;
                    else parent->right = NULL;
                    }
            }
      // case 2
      else if (node->left == NULL || node->right == NULL) {
          child = (node->left != NULL) ? node->left : node->right;
                    if (node == root) root = child;
                    else {
                              if (parent->left == node)
                                        parent->left = child;
                              else parent->right = child;
                    }
            }
      // case 3
      else {
              succp = node;
              succ = node->right;
              while (succ->left != NULL) {
                        succp = succ;
                        succ = succ->left;
              }
              if (succp->left == succ)
                    succp->left = succ->right;
              else succp->right = succ->right;

              node->data = succ->data;
              node = succ;
      }
      free(node);
    ```

### 이진탐색트리 전체 프로그램
- 쉽게 사용할 수 있는 인터페이스 함수 사용
  - search_BST(int key)
    - key를 전달하면 전체 트리에서 해당 노드를 찾음
    - search() 연산은 사용하여 실패와 성공을 각각 화면에 나타냄
  - insert_BST(int key)
    - key를 가진 노드를 생성하고 이진탐색트리에 삽입
    - insert() 연산을 사용, 화면 출력은 없음
  - delete_BST(int key)
    - key를 가진 노드를 전체 트리에서 찾아 삭제
    - delete() 연산 사용
    - 삭제할 노드가 없는 메시지 출력
- 프로그램
  - 10개의 노드 삽입한 후 여러 가지 방법으로 트리 순회
  - 중위 순회 : 정렬이 되어 있어야힘

#### 프로그램 9.5 : 이진탐색트리 테스트 프로그램
```
...
TNode* search(TNode *n, int key) {...} // 프로그램 9.1과 동일
void search_BST(int key)
{
  TNode* n = search(root, key);
  if (n != NULL)
            printf("[탐색 연산] : 성공 [%d] = 0x%x\n", n->data, n);
  else
            printf("[탐색 연산] : 실패 : No %d!\n", key);
}

int insert(TNode* r, TNode* n) {...} // 프로그램 9.3과 동일
void insert_BST(int key)
{
  TNode* n = create_tree(key, NULL, NULL);
  if(is_empty_tree())
      root = n;
  else if (insert(root, n) == 0) // 중복 발생시 새로 생성한 노드 삭제
      free(n);
}

// 이진탐색트리 삭제
int delete(TNode *parent, TNode *node) {...} // 프로그램 9.4과 동일
void delete_BST(int key)
{
  TNode *parent = NULL;
  TNode *node = root;

  if(node == NULL) return;
  while(node != NULL && node->data != key) {
          parent = node;
          node = (key < node->data) ? node->left : node->right;
  }
  if(node == NULL)
          printf(" Error : key is not in the tree!\n");
  else delete(parent, node);
}

void main() {
  // 삽입 연산 테스트
  init_tree();
  printf("[삽입 연산] : 35 18  7 26 12  3 67 22 30 99");
  init_tree();
  insert_BST(35);  insert_BST(18);
  insert_BST(7);   insert_BST(26);
  insert_BST(12);  insert_BST(3);
  insert_BST(68);  insert_BST(22);
  insert_BST(30);  insert_BST(99);

  // 트리 정보 출력
  printf("\n   In-Order : ");  inorder(root);
  printf("\n  Pre-Order : ");  preorder(root);
  printf("\n Post-Order : "); postorder(root);
  printf("\nLevel-Order : "); levelorder(root);

  printf("\n 노드의 개수 = %d\n", count_node(root));
  printf("  단말의 개수 = %d\n", count_leaf(root));
  printf("  트리의 높이 = %d\n", calc_height(root));

  // 탐색 연산 테스트
  search_BST(26);
  search_BST(25);

  // 삭제 연산 테스트
  printf("\noriginal bintree : LevelOrder : "); levelorder(root);
  delete_BTS(3);
  printf("\ncase1 : < 3> 삭제 : LevelOrder : "; levelorder(root);
  delete_BTS(68);
  printf("\ncase2 : <68> 삭제 : LevelOrder : ", levelorder(root);
  delete_BTS(18);
  printf("\ncase3 : <18> 삭제 : LevelOrder : ", levelorder(root);
  delete_BST(35);
  printf("\ncase3 : <35> root : LevelOrder : ", levelorder(root);

  // 최종 트리 정보 출력
  printf("\n 노드의 개수 = %d\n", count_node(root));
  printf("  단말의 개수 = %d\n", count_leaf(root));
  printf("  트리의 높이 = %d\n", calc_height(root));
}
```

### 이진탐색트리의 성능
- 이진 탐색 트리에서의 탐색, 삽입, 삭제 연산의 시간 복잡도는 트리의 높이를 h라고 했을 때 h에 비례
- 최선의 경우
  - 이진 트리가 균형적으로 생성되어 있는 경우 : h = log₂n
  - 시간 복잡도 : O(logn)
- 최악의 경우
  - 경사 이진 트리 : h = n
  - 시간 복잡도 : O(n)

### 이진탐색트리의 응용 : 나의 단어장
- 영어 사전
- 데이터 필드
  - 단어 부분과 의미 부분
- 키 : 단어
- 단어장 프로그램의 기능과 main() 함수
  - 이진 트리로 구현
  - 노드에 레코드가 저장
- 단어장의 기능
  - 입력(i) : 단어와 의미를 입력하여 하나의 노드 추가
  - 삭제(d) : 단어를 입력하면 해당 노드를 찾아 트리에서 제거
  - 단어 탐색(w)
    - 단어를 입력하면 해당 "단어"의 노드를 찾아 "의미"를 출력
  - 의미 탐색(m)
    - 의미를 입력하면 해당 "의미"의 노드를 찾아 "단어"를 출력
  - 사전 출력(p) : 사전의 모든 단어를 알파벳 순서대로 화면에 출력
  - 종료(q) : 프로그램을 종료
- 구조채 : 변경할 부분
```
typedef struct DicRecord {
  char word[MAX_WORD_SIZE]; // 단어 : 키
  char meaning[MAX_MEANING_SIZE]; // 의미
} Record;
typedef Record TElement; // 노드에 저장되는 데이터
typedef struct BinTrNode {
  TElement data;
  struct BinTrNode* left;
  struct BinTrNode* right;
}  TNode;
TNode* root = NULL;
```
- 프로그램 9.10 : 단어장 프로그램의 main() 함수
```
// 이진 탐색 트리를 사용하는 영어 사전 프로그램
void main()
{
  char command, word[80], meaning[200];
  do {
        printf("[사용법] i-추가 d-삭제 w-단어검색 m-의미검색 p-출력 q-종료 =>");
        command = getche(); // 명령 입력
        printf("\n");

        switch(command) {
            case 'i':                // 새로운 단어-의미 입력
                      printf("  > 삽입 단어 : "); gets(word);
                      printf("  > 단어 의미 : "); gets(meaning);
                      insert_word(word, meaning);
                      break;
            case 'd':
                      printf(" > 삭제 단어 : "); gets(word);
                      delete_word(word);
                      break;
            case 'p':
                      print_dic(); // 전체 단어장 출력
                      break;
            case 'w':                       // 단어로 검색
                      printf("  > 검색 단어 : "); gets(word);
                      search_word(word);
                      break;
            case 'm':                        // 의미로 검색
                      printf("  > 검색 의미 : "); gets(word);
                      search_meaning(word);
                      break;
            }
    } while (command != 'q');
}
```
- 단어장을 위한 탐색 연산(단어로 탐색)
  - 키가 문자열 비교 : strcmp(str1, str2)
  - 앞에서 탐색 비교 연산자 : int 형
- 프로그램 9.11 : 단어장을 위한 탐색 연산(단어로 탐색)
```
TNode* search(TNode *n, char *key)
{
  if (n == NULL)
          return NULL;
  else if (strcmp(n->data.word, key) == 0)
            return n;
  else if (strcmp(n->data.word, key) < 0)
            return search(n->left, key);
  else
            return search(n->right, key);
}
```
- 프로그램 9.12 : 단어장을 위한 삽입 연산
  - 레코드의 "단어"부분을 비교하여 삽입할 위치를 찾도록 수정
```
void insert(TNode* r, TNode* n)
{
  int ret = strcmp(r->data.word, n->data.word);

  if (ret == 0) return;
  if (ret < 0) {
          if (r->left == NULL) r-left = n;
          else insert(r->left, n);
  }
  else {
          if (r->right == NULL) r->right = n;
          else insert(r->right, n);
  }
}
```
  - 삭제 연산은 그대로 사용
- 프로그램 9.13 : 단어장을 위한 탐색 연산(의미로 탐색)
  - "의미 탐색" : 트리의 모든 노드를 방문하면서 하나씩 비교
  - 어떤 순회 알고리즘을 사용해도 무방
```
static TNode* search1(TNode *n, char *meaning)
{
  TNode* m;
  if (n == NULL) return NULL;
  if strcmp(n->data.meaning, meaning) == 0 return n;

  m = search1(n->left, meaning);
  if ( m != NULL)
        return m;
  else
        return search1(n->right, meaning);
}
```
- 프로그램 9.14 : 단어장의 삽입 인터페이스 함수
```
TNode* insert_word(char* key, char* val)
{
  TNode* n;
  Record r;
  strcpy(r.word, key);
  strcpy(r.maening, val);
  n = create_tree(r, NULL, NULL);

  if (is_empty_tree()) root = n;
  else insert(root, n);
  return root;
}
```
- 삭제 인터페이스 함수 : delete_word(word)
  - 단어를 전달받아 해당 노드를 트리에서 삭제하는 함수
  - delete_BTS()와 유사하나, 키의 비교 방법이 다름
- 프로그램 9.15 : 단어장의 삭제 인터페이스 함수
```
void delete_word(char* key)
{
  TNode *parent = NULL;
  TNode *n = root;
  int ret;

  while (n != NULL) {
            ret = strcmp(n->data.word, key);
            if (ret == 0) break;
            parent = n;
            n = (ret < 0) ? n->left : n->right;
  }
  if (n == NULL)
          printf(" Error : key is not in the tree!\n");
  else delete(parent, n); // 3가지 case
}
```
```
void delete (TNode *parent, TNode *node) // 프로그램 9.4
{
  TNode *child, *succ, *succp;
          ...
}
```
- 프로그램 9.16 : 단어장의 탐색 인터페이스 함수들
```
void search_word(char *word)
{
  TNode *n = search(root, word);
  if (n != NULL)
  {
      printf("    >> ");
      printf(" %12s : %-40s\n", n->data.word, n->data.meaning);
  }
  else printf("    >> 등록되지 않은 단어 : %s\n", word);
}

void search_meaning(char *m)
{
  TNode *n = search1(root, m);
  if (n != NULL)
  {
      printf("    >> ");
      printf(" %12s : %-40s\n", n->data.word, n->data.meaning);
  }
  else printf("    >> 등록되지 않은 의미 : %s\n", m);
}
```
- 출력 인터페이스 : print_dic()
  - 단어장의 모든 단어의 의미를 화면에 출력하는 인터페이스 함수
  - 모든 노드 방문을 위해 중위순회 사용
- 프로그램 9.17 : 단어장의 출력 인터페이스 함수들
```
void inorder(TNode *n)
{
  if (n != NULL)
  {
    inorder(n->left);
    printf(" %12s : %-40s\n", n->data.word, n->data.meaning);
    inorder(n->right);
  }
}

void print_dic()
{
  printf("    >> 나의 단어장 : \n");
  if (root != NULL) inorder(root);
}
```

#### 프로그램 9.18 단어장 프로그램
```
#include <stdio.h>
#include <stdlib.h>
#include <conio.h>
#include <string.h>
#define MAX_WORD_SIZE 40
#define MAX_MEANING_SIZE 200

typedef struct DicRecord {
  char word[MAX_WORD_SIZE];
  char meaning[MAX_MEANING_SIZE];
} Record;

typedef Record TElement;
typedef struct BinTrNode {
  TElement data;
  struct BinTrNode* left;
  struct BinTrNode* right;
} TNode;
Tnode* root = NULL;

int is_empty_tree() {...}
TNode* create_tree(TELement val, TNode* l, TNode* r) {...}
TNode* search(TNode *n, char* key) {...}
voidinsert(TNode*r,TNode*n) {...}
voiddelete(TNode*parent,TNode*node) {...}
Tnode* search1(TNode*n,char*key) {...}
void inorder(Tnode*n) {...}
TNode*insert_word(char*key,char*val) {...}
void delete_word(char*key) {...}
void search_word(char*word) {...}
void search_meaning(char*m) {...}
void print_dic() {...}

void main()
{
  char command, word[80], meaning[200];
  do {
        printf("[사용법] i-추가d-삭제w-단어검색m-의미검색p-출력q-종료=>");
        command = getche(); //명령입력
        printf("\n");

        switch (command) {
              case 'i': //새로운단어-의미입력
                    printf(" >삽입단어: "); gets(word);
                    printf(" >단어의미: "); gets(meaning);
                    insert_word(word, meaning);
                    break;
              case 'd': printf(" >삭제단어: "); gets(word); //기존단어삭제
                    delete_word(word);
                    break;
              case 'p': print_dic(); //전체단어장출력
                    break;
              case 'w': printf(" >검색단어: "); gets(word); //단어로검색
                    search_word(word);
                    break;
              case 'm': printf(" >검색의미: "); gets(word); //의미로검색
                    search_meaning(word);
                    break;
                  }
        } while (command != 'q');
}
```

##### ✍️작성자: 박지안
##### 🐧실습 환경: Visual Studio 2022
##### 🗓️ 작업일: 2026-06-07
