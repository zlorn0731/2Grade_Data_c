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

9pg부터 시작
