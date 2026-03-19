# 📚 2학년 데이터구조 (Data Structures)
## 2장 : 배열과 구조체 중 구조체
## 배열   | array
## 구조체 | struct
- 한꺼번에 많은 자료를 표현해야 하는 경우

### 구조체 
- 기존의 자료형들을 조합해 새로운 자료형을 만드는 방법
  - 배열의 차이
    - 구조체(struct) : 타입이 다른 데이터를 하나로 묶음
    - 배열(array) : 타입이 같은 데이터들을 하나로 묶음
#### (예시)
```
| A[0] |       --------
| A[1] |       | 학번 |
| A[2] |       --------
| A[3] |       | 이름 |
| A[4] |       --------
| A[5] |       | 학점 |
 -배열-        --------
               -구조체-
```

### 구조체의 정의와 선언

```
- 정의
struct Student{
	int id;
	char name[20];
	double score;
};

- 선언
struct Student a;

- 초기화
Student a = { 20230844, "박지안", 96.3 };
```
```
- 정의
typedef struct Student_t{ 
	int id;
	char name[20];
	double score;
} Student; --> Student로 대체 

- 선언
Student a; // 대체

- 초기화
Student a = { 20230844, "박지안", 96.3 };
```

### 멤버 접근
- 항목 연산자(membership operator) '.'으로 표시
#### (예시)
```
a.id = 20230844;
a.score = 92.3;
strcpy(a.name, "JIAN"); // a.name = "JIAN";은 오류 발생
```
- a.name = "JIAN"; 왜 오류인가?
    1. char name[20];의 배열 선언
    2. 배열은 주소를 변경할 수 없음
    3. 그래서 strcpy로 a.name배열 안에 copy
    4. strcpy(a.name, "JIAN"); 으로 코드 작성 해줘야 함
 
#### (예시)
```
#include <stdio.h>
#include <string.h>

struct Student {
	int id;
	char name [20];
	float score;
};

int main() {
	struct Student s;
	
	s.id = 20230844;
	strcpy(s.name, "JIAN");
	s.score = 100;

	printf("학번: %d\n", s.id);
	printf("이름: %s\n", s.name);
	printf("학점: %.1f\n", s.score);

	return 0;
}
```
```
- 결과
학번: 20230844
이름: JIAN
학점: 100.0
```

### 구조체와 연산자
- 대입 연산자만 가능
#### (예시)
```
int x, y = 10; // 변수
Student a, b = { 20230844, "박지안", 96.3 }; // 구조체
x = y; // OK: int 변수의 복사
a = b; // OK: 구조체 변수의 복사
```
- a = b;
```
a.id = b.id;
strcpy(a.name, b.name);
a.score = b.score;
이 세 코드가  a =b; 내부적으로 구성되있
```  

- 다른 연산자 사용 불가
#### (예시)
```
if (a > b) // 오류: 구조체의 비교연산 불가
      a += b; // 오류: 구조체의 다른 대입 연산도 불가
```
- 구조체는 '덩어리 데이터'라서
  - 크다 작다 비교 불 >, <, >=, <=
  - 더하기 같은 연산 정의 안되있음 +, -
- 구조체 비교하는 방법
#### (예시)
```
int compare(Student a, Student b) { // 학생 2명을 받아서 비교 결과를 정수로 반환 해줌	
	if (a.id != b.id) // a.id, b.id의 값이 다른면 return | ex) a.id = 20230844, b.id = 20230111
		return a.id - b.id;

	int name_cmp = strcmp(a.name, b.name); // 문자열 비교 함수 | strcmp --> #include <string.h> 헤더 파일 필요
	if (name_cmp != 0) // if (strcmp(a.name, b.name) != 0) 이렇게도 가능
		return name_cmp; // return strcmp(a.name, b.name; 위에를 바꾸면 아래도 바꿔주기

	if (a.score > b.score) return 1; 
	if (a.score < b.score) return -1;

	return 0;
```
- | a.id != b.id | name_cmp != 0 |
  - | | , | | 값이 같으면 0
  - | | , | | 첫 번째 값이 크면 양수
  - | | , | | 두 번째 값이 크면 음수
#### (예시)
```
strcmp("JIAN", "JIAN")   // 0
strcmp("Park", "Kim")    // 양수일 가능성 큼
strcmp("Amy", "Bob")     // 음수
```

- 문자열 함수
  - strcpy  | 문자열 복사   
  - strcmp  | 문자열 비교  
  - strlen  | 문자열 길이   
  - strcat  | 문자열 붙이기 
  - strncpy | n개만 복사
  - strncmp | n개만 비교

### typedef 
- 자료형의 별명 붙여준다고 생각하면 편함
- 긴 자료형의 이름을 짧게 변환하여 편하게 코딩 하기 위한 목적
#### (예시)
```
- typedef 없을 때
struct Student {
    int id;
    char name[20];
    float score;
};

struct Student s1;   // 이렇게 써야 함 (불편)
```
```
- typedef 있을 때
typedef struct Student {
    int id;
    char name[20];
    float score;
} Student; // 이름 변환
```
```
- 별칭을 정의할 경우 구조체 이름 생략 가능
typedef struct  { <-- 이름 생략
    int id;
    char name[20];
    float score;
} Student;
```

- 구조체를 포함한 구조체
#### (예시)
```
typedef struct  { // 별칭을 정의한 경우 이름 생략
    int month;
    int date;
} Birthday; // 구조체의 별칭
```
- 구조체의 배열 가능
#### (예시)
```
typedef struct {
	char name[20];
	Birthday birthday; // Firend안에 Birthday 데이터 값이 있어야되기 때문에 Birthday birthday; 변수 선언 해줌
} Freind;
```
```
int main() {
	Friend list[100]; // Friend 구조체를 100개 저장하는 배열
	list[0].birthday.month = 7; // 첫 번째 친구값 . 그 친구 생일 구조체 . 그 안의 값
	list[0].birthday.date = 5; // 첫 번째 친구값 . 그 친구 생일 구조체 . 그 안의 값
	strcpy(list[0].name, "Minyoung"); // <string.h> 헤더 파일 필요
}
```

### 구조체와 함수
- 함수의 매개 변수나 반환형으로 사용할 수 있음
  - Call by value : 변수(값)을 복사
#### (예시) - pg.58 프로그램 2.3
```
#include <stdio.h>

typedef struct { // 실수부와 허수부를 멤버로 갖는 복소수 구조체 정의
	double real; // 복소수의 실수부 --> 복소수 : z = a + bi | 실수부 : a 
	double imag; // 복소수의 허수부 --> 복소수 : z = a + bi | 허수부 : b, 허수 단위 : i
} Complex; // 타입(자료형)

// 복소수의 내용을 출력하는 함수
void print_complex(Complex c) { // Complex c = 지역 변수
	printf("%4.1f + %4.1fi\n", c.real, c.imag);
}

// 복소수의 실수부와 허수부를 모두 0으로 초기화하려는 함수
void reset_complex(Complex c) { // Complex c = 지역 변수
	c.real = c.imag = 0.0;
}

void main() {
	Complex a = { 1.0, 2.0 };
	printf("초기화 이전: ");
	print_complex(a); // 복소수 화면 출력
	reset_complex(a); // 초기화가 되지 않음
	printf("초기화 이후: ");
	print_complex(a); // 복소수 화면 출력
}
```
```
- 결과
초기화 이전: a = 1.0 + 2.0i
초기화 이후: a = 1.0 + 2.0i
```

### 배열과 구조체의 응용 : 다항식
- 다항식의 일반적인 형태
- p(x) = aₙxⁿ + aₙ₋₁xⁿ⁻¹ + ... + a₁x + a₀
  - a : 계수(coefficient)
  - 차수(exponent)
  - 다항식의 차수 : p(x)의 가장 큰 차수

- 다항식의 표현
  - 다항식의 모든 계수들을 배열에 저장하는 방법
#### (예시)
```
p(x) = 10x⁵ + 6x + 3
∇
p(x) = 10x⁵ + 0x⁴ + 0x³ + 0x² + 6x¹ + 3x⁰
```
- 계수(coefficient)의 리스트인 (10, 0, 0, 0, 6, 3)을 배열에 저장하는 방법
- 차수(degree)를 저장하는 배열이 하나 더 있어햐 함
#### (예시)
```
#define MAX_DEGREE 101 // 다항식의 최고 차수 + 1
typedef struct {
	int degree;
	float coef[MAX_DEGREE];
} Polynomial;
```
- #define MAX_DEGREE 101 : 상수(값)를 이름으로 만든 것
  - 왜 101이냐? : 최대 차수 100까지, 배열은 0부터 시작 | 0 ~ 100 -> 총 101개
  - 최대 100차 다항식

- typedef struct : 구조체 만들면서 이름(별명)도 같이 만드는 것

- int degreee; : 다항식의 최고 차수 저장
#### (예시) 
```
p(x) = 10x⁵ + 6x + 3 | degree = 5
```
- float coef[MAX_DEGREE]; : 계수들을 저장하는 배열
  - 왜 float? : 계수가 소수일 수도 있어서
  - 배열 의미 : index = 차수
#### (예시)
```
p(x) = 10x⁵ + 6x + 3

coef[0] = 10;  // x⁵
coef[1] = 0;   // x⁴
coef[2] = 0;   // x³
coef[3] = 0;   // x²
coef[4] = 6;   // x¹
coef[5] = 3;   // x⁰
```

- } Polynomial; : 구조체 이름(별명) = Polynomial

### 다항식 구조체
- 배열에 계수를 저장하는 순서
### 방법1 pg.61
```
      [0] [1] [2] [3] [4] [5] [6] [7] [8] [9] 
coef | 10 | 0 | 0 | 0 | 6 | 3 |   |   |   |   |
        |    \    \    \       \     \
p(x) = 10x⁵ + 0x⁴ + 0x³ + 0x² + 6x¹ + 3x⁰
```
- 방법1 : Polynomial a = { 5, { 10, 0, 0, 0, 6, 3 } };

### 방법2 pg.61
```
      [0] [1] [2] [3] [4] [5] [6] [7] [8] [9] 
coef | 3 | 6 | 0 | 0 | 0 | 10 |   |   |   |   |
        /      /    |     \     \     \
p(x) = 10x⁵ + 0x⁴ + 0x³ + 0x² + 6x¹ + 3x⁰
```
- 방법2 : Polynomial a = { 5, { 3, 6, 0, 0, 0, 10} };

### 다항식 입출력 연산(방법1)
- 다항식의 구현
  - 입력 함수, 출력 함수, 다항식의 덧셈
 
#### (예시) pg.62 프로그램 2.4
- 다항식의 출력 함수 print_poly()
```
void print_poly(Polynomial p, char str[])
{
	int i;
	printf("\t%s", str); // 문자열은 다항식에 대한 설
	for (i = 0; i < p.degree; i++)
		printf("%5.1f x^%d + ", p.coef[i], p.degree - i);
	printf("%4.1f\n", p.coef[p.degree]);
}
```
- void print_poly(Polynomial p, char str[]) : 함수 선언
  - Polynomial p : 출력할 다항식
  - char str[] : 앞에 붙일 문자열 (예: "p(x) = ")
#### (예시)
```
print_poly(p, "p(x) = ");
```

- int i;
  - 반복문에서 사용할 변수
 
- printf("\t%s", str); : 문자열 출력
  - \t : 탭(들여쓰기)
  - %s : 문자열 출력
 
- for ( i = 0; i < p.degree; i++) : 핵심🔥
  - 최고차항부터 마지막 전 항까지 반복
  - 왜 < p.degree? : 마지막 항은 따로 출력
 
- printf("%5.1f x^%d + ", p.coef[i], p.degree - i); : printf 핵심🔥
  - %5.1f : 실수 출력 (자리 맞춤)
  - p.coef[i] : 계수
  - p.degree - i : 차수 계산
  - 왜 이렇게 계산?
```
    coef[0] = 최고차
    coef[1] = 다음
    ...
```
  - 차수 = degree - index
#### (예시)
```
degree = 5

| i | coef[i] | 출력  |
| - | ------- | ----- |
| 0 | 10      | 10x^5 |
| 1 | 0       | 0x^4  |
| 2 | 0       | 0x^3  |
| 3 | 0       | 0x^2  |
| 4 | 6       | 6x^1  |
```

- printf("%4.1f\n", p.coef[p.degree]); : 마지막 항 (상수항, x⁰)
  - 왜 따로 출력? : 마지막에는 + 붙이면 안됨

#### (예시) 방법2를 사용할 경우
```
for (i = p.degree; i > 0; i--) // for문 역순으로 생성
     printf("%5.1f x^%d + ", p.coef[i], p.degree);
printf("%4.1f\n", p.coef[0]);
```

#### (예시) pg.63 프로그램 2.5
- 다항식의 입력 함수 read_poly()
``` 
Polynomial read_poly()
{
	int i;
	Polynomial p;
	printf("다항식의 최고 차수를 입력하시오: ");
	scanf("%d", &p.degree);
	printf("각 항의 계수를 입력하시오 (총 %d개): ", p.degree + 1);
	for (i = 0; i <= p.degree; i++)
		scanf("%f", p.coef + i);
	return p;
}
```
- Polynomial read_poly() : 함수의 머리 부분
  - 함수 이름 : read_poly
  - 반환형 : Polynomial -> 함수 실행이 끝나면 Polynomial 타입의 값 하나를 돌려줌
    - 왜 Polynomial을 반환하냐? : 함수 목적 | 사용자가 입력한 다항식을 구조체에 저장해서 넘겨주는 것
#### (예시)
```
Polynomial p;
p = read_poly();
```

- int i; : 반복문에서 사용할 변수
- 계수 여러 개를 입력 -> 몇 번째 계수를 입력받는지 세기 위함

- Polynomial p;
- p : Polynomial 타입 변수를 하나 만듦
  - 이 변수 안에 p.degree, p.coef[] 있음
 
- scanf("%d", &p.degree); : 최고 차수 입력받기
- & : p.degree는 정수값이 들어갈 변수 | &p.degree는 그 변수의 주소

- printf("각 항의 계수를 입력하시오 (총 %d개): ", p.degree + 1); : 계수 입력 안내

- for (i = 0; i <= p.degree; i++)
- 계수를 하나씩 입력받기 위한 반복문
- 왜 0부터 p.degree까지냐?
```
배열에 저장되는 위치
coef[0]
coef[1]
coef[2]
coef[3]
coef[4]
coef[5]
```

- scanf("%f", p.coef + i); : 핵심🔥
  - c.oef가 뭐냐? : 계수 배열
#### (예시)
```
p.coef[0]
p.coef[1]
p.coef[2]
...
``` 
  - p.coef + i 무슨 뜻이냐?
    - &p.coef[i] 같은 뜻
    - i번째 계수를 저장할 위치의 주소를 의미
#### (예시)
```
i = 0
scanf("%f", p.coef + 0);
→ p.coef[0]에 저장

i = 1
scanf("%f", p.coef + 1);
→ p.coef[1]에 저장

i = 2
scanf("%f", p.coef + 2);
→ p.coef[2]에 저장
```
    - p.coef + i = &p.coef[i] 같은 뜻 둘다 가능
```
scanf("%f", &p.coef[i]);
scanf("%f", p.coef + i);
```

- return p;
  - 입력받아서 완성된 다항식 p를 함수 밖으로 돌려줌
  - 함수가 끝나면 p전체가 반환
 
### 다항식 덧셈 연산
- 단순화 방법 : p = a + b -> (1단계) p = a; (2단계) p += b;
#### (예시)
```
Polynomial add_poly(Polynomial a, Polynomial b)
{
	int i;
	Polynomial p;
	if (a.degree > b.degree) { // 차수가 높은 다항식을 먼저 복사
		p = a;
		for (i = 0; i <= b.degree; i++)
			p.coef[i + (p.degree - b.degree)] += b.coef[i];
	}
	else {
		p = b;
		for (i = 0; i <= a.degree; i++)
			p.coef[i + (p.degree - a.degree)] += a.coef[i];
	}
	return p;
}
```
- Polynomial add_poly(Polynomial a, Polynomial b) : 함수 선언
  - 함수 이름 : add_poly
  - 매개변수 : 다항식 2개 a, b
  - 반환형 : Polynomial
- 다항식 a와 b를 더해서 그 결과를 Polynomial 형태로 돌려준다는 뜻

- int t; : 반복문에서 사용할 변수
  - 계수들을 하나씩 더하기 위해 필요
 
- Polynomial p; : 결과를 저장할 다항식 변수
  - a + b의 결과를 p에 담고 마지막에 return p;로 돌려줌
 
- if (a.degree > b.degree) : 어느 다항식의 차수가 더 큰지 비교
  - 왜 비교? : 배열에 저장된 계수의 시작 위치가 차수에 따라 달라서 정렬 맞춰서 더해야 함
 
- p = a; : 결과 다항식 p를 일단 a로 복사
  - p.degree = a.degree, p.coef[] : a의 계수들로 한 번에 복사
  - 구조체는 =로 통째 복사 가능
- 왜 먼저 복사?
  - 차수가 더 큰 다항식을 먼저 결과로 잡아두면 편함
```
예를 들어
a(x) = 3x⁴ + 2x³ + 1
b(x) = 5x² + 7
이면 결과도 당연히 최고차수 4
-> 큰 차수인 a를 먼저 복사해 놓고, 거기에 b를 더하는 방식으로 만듦
```

- for (i = 0; i <= b.degree; i++)
  - b의 모든 계수를 하나씩 보겠다는 뜻
  - b.degree = 2 -> b.coef[0] | b.coef[1] | b.coef[2] 총 3개를 처리

- p.coef[i + (p.degree - b.degree)] += b.coef[i]; : 가장 핵심🔥
  - 의미 : b의 계수를 p의 올바른 위치로 이동해서 더함
  - 왜 필요하냐 : 다항식은 같은 차수끼리 더해야 함
  - 다항식 형태 : p(x) = aₙxⁿ + aₙ₋₁xⁿ⁻¹ + ... + a₁x + a₀
  - 덧셈 원리 : aₖxᵏ + bₖxᵏ = (aₖ + bₖ)xᵏ
```
배열 저장 방식
coef[0] → xⁿ
coef[1] → xⁿ⁻¹
...
coef[n] → x⁰
```
#### (예시) 
```
a(x) = 10x⁵ + 6x + 3
b(x) = 2x² + 4x + 1

배열 상태
a: [10, 0, 0, 0, 6, 3]   (degree = 5) [0][1][2][3][4][5]
b: [2, 4, 1]             (degree = 2) [0][1][2]

문제 발생 : index 그대로 더하면 틀림
b의 index 0은 x²인데
a의 index 0은 x⁵
a.coef[0] + b.coef[0]
→ 10x⁵ + 2x² (❌)

해결 방법 : 차이를 계산해서 위치 맞춤
b를 오른쪽으로 밀어서 맞춰야 함
shift = p.degree - b.degree
      = 5 - 2
      = 3 // 3칸 밀어야 
	  
index 이동
b.coef[0] → p.coef[3]
b.coef[1] → p.coef[4]
b.coef[2] → p.coef[5]
-----------------------------
index : 0   1   2   3   4   5
a     : 10  0   0   0   6   3
b     : 0   0   0   2   4   1
-----------------------------
이제 같은 차수끼리 위치가 맞음

공식
k = i + (p.degree - b.degree)
k = i + shift

코드와 연결
p.coef[i + (p.degree - b.degree)] += b.coef[i];

수식 형태
k = i + (p.degree - b.degree)
p.coef[k] = p.coef[k] + b.coef[i]

실제 계산
a(x) = 10x⁵ + 6x + 3
b(x) = 2x² + 4x + 1
|-------------------------------|
| index : 0   1   2   3   4   5 |
| a     : 10  0   0   0   6   3 |
| b     : 0   0   0   2   4   1 |
|-------------------------------|
- i = 0 (b의 x²)
k = i + shift
k = 0 + 3 = 3
x² 위치는 index 3
p.coef[3] += 2

- i = 1 (b의 x¹)
k = i + shift
k = 1 + 3 = 4
x¹ 위치는 index 4
p.coef[4] += 4

- i = 2 (b의 x⁰)
k = i + shift
k = 2 + 3 = 5
상수항 위치는 index 5
p.coef[5] += 1
```
#### 전체 다항식 프로그램 코드 pg.65 프로그램 2.7

### 희소 다항식(Sparse Polynomial)
- 대부분 항의 계수가 0인 다항식
  - 10x¹⁰⁰ + 6 다항식에서 차수가 100으로 매우 크지만 계수가 0이 아닌 항은 2개뿐
  - 이와 같은 대부분 항의 계수가 0인 다항식을 희소 다항식이라 칭함
  - 앞에서 구현한 다항식 프로그램으로 희소 다항식을 구현하려면 메모리 낭비 심함
  - 101개의 항의 계수를 저장하기 위한 공간 중에서 실제로 2개만 사용되고 나머지는 0으로 채워짐

#### (예시)
```
(계수, 지수)로 표현
10x⁵ + 6x + 3 -> ((10, 5), (6, 1), (3, 0))
10x¹⁰⁰ + 6 -> ((10, 100), (6, 0))
```

- 희소 다항식 처리를 위한 구조체
  - 2개의 구조체 필요
  - 항의 정보를 (계수, 지수)의 형태로 저장하는 구조체 정의 필요
#### (예시)
```
typedef struct { // 하나의 항을 표현하는 클래스
	int expon; // 항의 지수
	float coeff; // 항의 계수
} Term;
```
  - 이 구조체는 하나의 항만을 표현
    - 전체 희소 다항식을 나타내는 구조체를 추가로 정의 필요
#### (예시)
```
typedef struct { // 희소 다항식 클래스
	int nTerms; // 계수가 0이 아닌 항의 개수
	Term term[MAX_TERMS]; // 계수가 0이 아닌 항의 배열
} SparsePoly;
```
- 이것을 SparesPoly라고 하자
- 희소 다항식 구조체는 다음과 같이 Term의 배열을 갖는 구조가 되어야함
- 계수가 0이 아닌 항의 개수를 위한 멤버 변수도 필요
- 되도록이면 함수 구현이 편한 Polynomial 사용 권장
- 만약 메모리의 제한이 큰 시스템이나 심한 희소 다항식에 대해서는 SparesPoly 효율 좋음

##### ✍️작성자: 박지안
##### 🐧실습 환경: Visual Studio 2022
##### 🗓️ 작업일: 2026-03-19
