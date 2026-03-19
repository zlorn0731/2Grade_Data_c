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
### 방법 1 pg.61
```
      [0] [1] [2] [3] [4] [5] [6] [7] [8] [9] 
coef | 10 | 0 | 0 | 0 | 6 | 3 |   |   |   |   |
        |    \    \    \       \     \
p(x) = 10x⁵ + 0x⁴ + 0x³ + 0x² + 6x¹ + 3x⁰
```
### 방법 2 pg.61
```
      [0] [1] [2] [3] [4] [5] [6] [7] [8] [9] 
coef | 3 | 6 | 0 | 0 | 0 | 10 |   |   |   |   |
        /      /    |     \     \     \
p(x) = 10x⁵ + 0x⁴ + 0x³ + 0x² + 6x¹ + 3x⁰
```
