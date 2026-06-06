# 📚 2학년 데이터구조 (Data Structures)
## 7장 : 순환

### 순환(recursion), 재귀 호출
- 어떤 알고리즘이나 함수가 수행 도중에 자기 자신을 호출하여 문제를 해결하는 기법
  - 정의자체가 순환적으로 되어 있는 경우에 적합
  - 트리 구조에서 많이 사용
  ```
  (예시)
  void recursion() {
          recursion();
  }
  ```
- 순환은 순환적인 문제나 그러한 자료구조를 다루는 프로그램에 적합
```
(예시)
factorial
```

### 순환의 예
- 팩토리업 값 구하기
- 피보나치 수열
- 이항계수
- 하노이의 탑
- 이진 탐색

#### 예시 : 팩토리얼 프로그래밍 #1
- 팩토리얼 프로그래밍 #1
  - 정의대로 구현
  - (n-1)!을 구하는 서브 함수 factorial_n_1()을 따로 제작

```
[프로그램 7.1 순환적인 팩토리얼 계산 함수]

int factorial(int n)
{
    if (n == 1) return 1; // n==1인 경우(종료 조건)
    else return (n * factorial (n - 1)); // n>1인 경우(순환 호출)
}
```

### 순환 호출 순서
- 팩토리얼 함수의 호출 순서
```
factorial(3) = 3 * factorial(2)
         = 3 * 2 * factorial(1)
         = 3 * 2 * 1
         = 3 * 2
         = 6
```
```
[factorial(3)에서 함수 호출과 복귀 순서]

factorial(3)
{
    if (3 == 1) return 1;
    else return (3 * factorial(3 - 1));
}

↓(1) ↑(4)

factorial(2)
{
    if (2 == 1) return 1;
    else return (2 * factorial(2 - 1));
}

↓(2) ↑(3)

factorial(1)
{
    if (1 == 1) return 1;
    else return (1 * factorial(1 - 1));
}
```

#### 예시 : 팩토리얼 프로그래밍 #2
- 프로그램 7.2 : 출력문이 추가된 순화적인 팩토리얼 계산 프로그램
```
int factorial(int n)
{
    printf("factorial(%d)\n", n);
    if (n == 1)
        return 1;
    else
        return (n * factorial(n - 1));
}

void main()
{
    factorial(3);
}
```

### 순환 호출의 내부적인 구현
- 함수 호출 과정
  - A() 함수에서 B() 함수 호출할 때
    - 활성 레코드(activation record)를 시스템 스택에 저장
      - A() 함수의 복귀 주소와 사용하던 지역변수 등의 자료
    - B()의 코드 시작 위치로 이동하여 처리

### 순환 알고리즘의 구조
- 순환 호출을 하는 부분
- 순환 호출을 멈추는 부분
  - 이 부분이 없다면 결국 오류 발생
```
[순환 알고리즘의 구조]

int factorial (int n)
{
  if (n == 1) return 1; // 순환을 멈추는 부분
  else return n * factorial (n - 1); // 순환호출을 하는 부분
}
```
- (예시) 종료 조건을 없애고 프로그램 실행
```
int factorial (int n)
{
  printf("factorial(%d)\n", n);
  // if(n == 1) return 1;
  // else
  return (n * factorial(n - 1));
}
```
  - 순환 호출은 반드시 순환 호출을 멈추는 문장이 포함되어야 함

### 순환 <-> 반복
- 컴퓨터에서의 되풀이 : 순환과 반복
- 반복(iteration) : for나 while을 이용
  - 문제를 간결하고 효율적으로 해결
  - 어떤 문제들은 반복을 사용하면 지나치게 복잡해지는 경우 발생
    - (예시) 트리의 순회나 노드의 삽입 연산
```
    => 순환이 매우 좋은 해결책
```
- 대부분은 순환은 반복으로 바꾸어 작성할 수 있음
- 순환(recursion)
  - 순환 호출 이용
  - 순환적인 문제에서는 자연스러운 방법
  - 실행 속도 : 반복보다 느림
    - 함수 호출의 오버헤드
- 팩토리얼의 반복적 구현
```
[팩토리얼의 순환적인 정의와 반복적인 정의]

n! = n * n(n - 1)! <-> n! = n * (n - 1) * (n - 2) * (n - 3) ... * 1
```

#### 프로그램 7.3 : 반복적인 팩토리얼 계산 함수
```
int factorial_iter(int n)
{
  int result = 1;
  for(int k = n; k > 0; k--)
    result = result * k;
  return result;
}
```

### 분할 정복(divide and conquer)
- 순환은 문제를 나누어 해결하는 분할 정복 방법을 사용
```
factorial(int n)
{
  if(n == 1) return 1;
  else return(n * factorial(n - 1));
}
```
- 분할 정복
  - 어떤 문제를 더 작은 동일한 문제들로 분해하여 해결하는 방법
  - 순환 호출을 할 때마다 : 문제가 반드시 작아져야 함
- 문제가 순환적으로 정의되어 있는 순환 알고리즘
  - 팩토리얼 함수 계산, 피보나치 수열, 이항 계수, 각종 이진트리 알고리즘, 이진 탐색, 하노이의 탑 등

### 순환 알고리즘의 성능
- 반복과 순환의 성능 분석
  - (예시) 팩토리얼
    - 반복 : for문, O(n)
    - 순환
      - 한 번 호출할 때마다 1번의 곱셈, n번의 순환 호출 : O(n)
  - 순환 알고리즘
    - 이해하기 쉽고, 쉽게 프로그램 가능
    - 기억 공간과 실행 시간에서 비효율적인 경우 많음

### 거듭제곱의 값의 계산
- 순환적인 방법이 반복적인 방법보다 더 효율적인
- 숫자 x의 n제곱값인 xⁿ을 구하는 문제
- 방법 1 : 반복문 사용

#### 프로그램 7.4 : 반복적인 거듭제곱 계산 함수
```
double slow_power(double x, int n)
{
  int i;
  double result = 1.0;
  for(i = 0; i < n; i++)
    result = result * x;
  return result;
}
```

### 순환적인 거듭제곱 함수
- 방법 2 : 순환적인 호출
- 알고리즘 7.1 : 순환적인 거듭제곱 계산
```
power(x, n)

if n = 0
  then return 1;
else if n이 짝수
  then return power(x², n/2);
else if n이 홀수
  then return x * power(x², (n-1)/2);
```
```
[else if n이 짝수]

power(x, n) = power x², n/2);
            = (x²)ⁿ/²
            = x²(ⁿ/²)
            = xⁿ
```
```
[else if n이 홀수]

power(x, n) = power(x², (n-1) / 2)
            = x * (x²)(ⁿ-¹)/²
            = x * xⁿ-¹
            = xⁿ
```

### 거듭제곱을 구하는 순한 호출의 예
- 순환적으로 함수 호출할 때마다 문제의 크기 : 절반씩 줄어듦
```
n -> n/2 -> n/4 -> ....
```

#### 프로그램 7.5 : 순환적인 거듭제곱 계산 함수
```
double power(double x, int n)
{
  if(n == 0) return 1;
  else if((n % 2) == 0)
    return power(x * x, n / 2); // n이 짝수인 경우
  else
    return x * power(x * x, (n - 1) / 2); // n이 홀수인 경우
}
```

### 복잡도 분석
- 시간 복잡도
  - 순환적인 함수 프로그램 7.5 : O(logn)
  - 반복적인 함수 프로그램 7.4 : O(n)
- 순환적인 방법의 시간 복잡도
  - n이 100일 경우 : 100 → 50 → 25 → 12 → 6 → 3 → 1
  - n이 2의 거듭제곱 값의 하나인 2k라고 가정
    - n = 2k
    - log₂n = k

### 피보나치 수열의 계산
- 순환 호출을 사용하면 비효율적인 예
- 피보나치 수열
  - 앞의 두 숫자를 더해서 뒤의 숫자를 만든다
  - 0, 1, 1, 2, 3, 5, 8, 13, 21, ...
- 순환적인 구현
  - 단순하고 이해하기 쉽게 구현되었으나 실행 시간과 기억 공간에서 매우 비효율적

#### 프로그램 7.6 : 순환적인 피보나치 수열 계산 함수
```
int fib(int n) {
  if(n == 0) return 0;
  if(n == 1) return 1;
  return (fib(n - 1) + fib(n - 2));
}
```

### 순환적인 피보나치의 비효율성
- 같은 항이 중복해서 계산됨
  - n이 커지면 더욱 심각
    - (예시) n이 25 : 거의 25만 번의 호출
      - n이 30 : 약 300만 번의 함수 호출

### 반복적이 피보나치 수열 함수
- 반복이 훨씬 효율적

#### 프로그램 7.7 : 반복적인 피보나치 수열 계산 함수
```
int fibIter(int n)
{
  if(n < 2) return n;
  else
  {
    int i, tmp, cunrrent = 1, last = 0;
    for (i = 2; i <= n; i++) {
                tmp = current;
                current += last;
                last = tmp;
    }
    return current;
}
```
- 시간 복잡도 : O(n)

### 하노이 탑 문제
- 하노이의 탑(The Tower of Hanoi)
  - 고대 인도의 전설
- 막대 A에 쌓여있는 원판 n개를 막대 C로 옮기는 문제
  - 조건
    - 한 번에 하나의 원판만 이동할 수 있음
    - 맨 위에 있는 원판만 이동할 수 있음
    - 크기가 작은 원판 위에 큰 원판이 쌓일 수 없음
    - 중간의 막대를 임시로 이용할 수 있으나 앞의 조건들을 지켜야 함

### 남아있는 문제?
- n - 1개의 원판을 옮기는 문제를 순환적으로 해결
  - 순환이 일어날수록 문제 크기는 작아져야 함
- n - 1개의 원판을 A에서 B로 옮기는 문제나 B에서 C로 옮기는 문제는 동일
  - A에서 B로 옮기는 문제 : C를 임시로 사용
  - B에서 C로 옮기는 문제 : A를 임시로 사용
- 어떻게 n - 1개의 원판을 A에서 B로,  또 B에서 C로 이동하는가?
  - 순환을 이용
  ```
  void hanoiTower(int n, char from, char tmp, char to)
  {
    if (n == 1) {
        from에서 to로 원판을 옮김
    }
    else {
    (1) from의 맨 밑의 원판을 제외한 나머지 원판들을 tmp로 옮김
    (2) from에 있는 한 개의 원판을 to로 옮김
    (3) tmp의 원판들을 to로 옮김
    }
  }
  ```
  - (1) : to를 사용하여 from에서 tmp로 n-1개의 원판을 이동하는 문제
  - (3) : from을 사용하여 tmp에서 to로 n-1개의 원판을 이동하는 문제
- (1) from의 맨 밑의 원판을 제외한 나머지 원판들을 tmp로 옮김
  - : to를 사용하여 from에서 tmp로 n-1개의 원판을 이동하는 문제
  ```
  => hanoi_tower(n-1, from, to, tmp)
  ```
- (3) tmp의 원판들을 to로 옮김
  - : from를 사용하여 tmp에서 to로 n-1개의 원판을 이동하는 문제
  ```
  => hanoi_tower(n-1, tmp, from, to)
  ``

### 하노이의 최종 프로그램
- 프로그램 7.8 : 하노이의 탑 문제 프로그램
```
#include <stdio.h>

void honoiTower(int n, char from, char tmp, char to)
{
  if(n == 1) printf("원판 1을 %c에서 %c에서 옮긴다.\n", from, to);
  else {
          honoitTower(n-1, from, to, tmp);
          printf("원판 %d를 %c에서 %c으로 옮긴다.\n", n, from, to);
          honoiTower(n-1, tmp, from, to);
        }
}

void main() {
      hanoiTower(4, 'A', 'B', 'C');
}
```

### 순환의 응용 : 미로 탐색
- 덱(deque) : 미로 탐색(DFS, BFS)
```
#define MAZE_SIZE 6
char map[MAZE_SIZE][MAZE_SIZE] = {
  { '1', '1', '1', '1', '1', '1' },
  { 'e', '0', '1', '0', '0', '1' },
  { '1', '0', '0', '0', '1', '1' },
  { '1', '0', '1', '0', '0', 'x' },
  { '1', '1', '1', '1', '1', '1' },
};
int xEixt = 5, yEixt = 4, done = 0;

int is_valid(int x, int y) {
  if (x < 0 || y < 0 || x >= MAZE_SIZE || y >= MAZE_SIZE)
    return 0;
  else
    return map[y][x] == '0' || map[y][x] == 'x';
}

void search_recur(int x, int y)
{
  if (done) return;
  printf( "(%d,%d) ", x, y);
  if ( x == xExit && y == yExit) {
    done = 1;
    return;
  }
  map[y][x] = '5';
  if(is_valid(x - 1, y) search_recur(x - 1, y);
  if(is_valid(x + 1, y) search_recur(x + 1, y);
  if(is_valid(x, y - 1) search_recur(x, y - 1);
  if(is_valid(x, y + 1) search_recur(x, y + 1);
}

void main()
{
  search_recur(0, 1);
  if(done) printf("\n ==> 출구가 탐지되었습니다.\n");
  else printf("\n ==> 출구를 찾지 못했습니다.\n");
}
```

##### ✍️작성자: 박지안
##### 🐧실습 환경: Visual Studio 2022
##### 🗓️ 작업일: 2026-06-06
