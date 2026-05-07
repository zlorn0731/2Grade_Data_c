# 📚 2학년 데이터구조 (Data Structures)
## 6장 : 리스트

### 리스트란?
- 리스트(list) 또는 선형 리스트(linear list)
  - 순서를 가진 항목들의 모임
  - 집합 : 항목간의 순서 개념이 없음
  - L = (item 1, item 2, item 3, ....., item n-1)
```
예시:

요일 : (일요일, 월요일, ....., 토요일)
한글 자음의 모임 : (ㄱ, ㄴ, ...., ㅎ)
...
```
- 구조가 단순하여 가장 널리 사용되는 기초적인 자료구조 중의 하나
  - stack, queue, tree, graph
 
### 리스트의 구조
- Stack, Queue, Deque과이 공통점과 차이점
  - 선형 자료구조 : 가장 활용이 자유로움
  - 자료의 접근 위치
  ```
           임의의 위치에서도 삽입과 삭제가 가능
  리스트(List)           ⇅
         ⇄ | A | B | C | D |.....| F | ⇄
  요소 위치   [0] [1] [2] [3]      [n-1]
  ```

### 리스트의 연산 - ADT
- 데이터 : 임의의 접근 방법을 제공하는 같은 타입 요소들의 순서있는 모임
- 연산
  - init() : 리스트를 초기화
  - insert(pos, item) : pos 위치에 새로운 요소 item을 삽입
  - delete(pos) : pos 위치에 있는 요소를 삭제
  - get_entry(pos) : pos 위치에 있는 요소를 반환
  - is_empty() : 리스트가 비어 있는지를 검사
  - is_full() : 리스트가 가득 차 있는지를 검사
  - find(item) : 리스트에 요소 item이 있는지를 살핌
  - replace(pos, item) : pos 위치를 새로운 요소 item으로 바꿈
  - size() : 리스트안의 요소의 개수를 반
 
### 리스트 구현 방법
- 배열을 이용
  - 구현이 간단
  - 삽입, 삭제 시 오버헤드
  - 항목의 개수 제한
- 연결 리스트를 이용
  - 구현이 복잡
  - 삽입, 삭제가 효율적
  - 크기가 제한되지 않음
```
리스트 ADT         배열을 이용한 구현
   A          ↗ | A | B | C |   |   |
   B               연결리스트를 이용한 구현
   C          ↘ | A | |→ | B | |→ | C | |
                                        ↳NULL
```

### 배열로 구현한 리스트
- 배열에 항목들을 순서대로 저장
  - 중간에 비어 있는 항목이 없어야 함
  - length
    - 새로운 요소가 리스트의 맨 뒤에 추가될 때 삽입되어야 하는 위치
```
                         length
data[MAX_LIST_SIZE]        ↓
|  A  | |  B  | |  C  | |     | |     | |     |
data[0] data[1] data[2] data[3] data[4] data[5]
```
```
typedef int Element;
Element data[MAX_LIST_SIZE];
int length = 0; // 초기화
```

### 리스트 : 공백 상태 / 포화 상태
- 공백 상태
  - length : 0인지 검사
```
[공백 상태]

length
  ↓
|     |     |     | ... |     |
  [0]   [1]   [2]    [MAX_LIST_SIZE-1]
```
- 포화 상태
  - length : MAX_LIST_SIZE인지 검사
```
[포화 상태]

                             length
                                ↓
|  A  |  B  |  C  | ... |  F  |
  [0]   [1]   [2]    [MAX_LIST_SIZE-1]
```

#### 단순한 연산들
- init_list()
  - length를 0으로 초기화
- size()
  - length를 반환
- get_entry(id)
  - data[id]를 반환
- replace(id, e)
  - id번째 항목을 e로 교체
  - data(id) <- e
- clear_list()
  - 리스트를 비우는 연산
  - length를 0으로 초기화

### 삽입 연산
- 삽입 위치 다음의 모든 항목들을 뒤로 한 칸씩 이동
```
void insert(int pos, Element e)
{
    int i;

    if (is_full() == 0 && pos >= 0 && pos <= length)
    {
        for (i = lengthl i > pos i--)
            data[i] = data[i - 1];

        data[pos] = e;
        length++;
    }
    else
        error("포화상태 오류 또는 삽입 위치 오류");
}

시간 복잡도 : O(n)
```

### 삭제 연산
- 삭제 위치 다음의 항목들을 이동하여야 함
```
void delete(int pos)
{
    int i;

    if (is_empty() == 0 && 0 <= pos && pos < length)
    {
        for (i = pos + 1; i < length; i++)
            data[i - 1] = data[i];

        length--;
    }
    else
        error("공백상태 오류 또는 삭제 위치 오류");
}

시간 복잡도 : O(n), 매우 비효율적
```
