# 📚 2학년 데이터구조 (Data Structures)

## Computer Science Data Structures Study Repository

이 저장소는 **컴퓨터공학과 2학년 데이터구조 수업 내용을 정리한 GitHub 저장소입니다.**
C언어를 사용하여 다양한 **자료구조와 알고리즘을 직접 구현하고 학습 내용을 기록**합니다.

---

# 🧠 학습 내용

* Pointer (포인터)
* Struct (구조체)
* Algorithm (알고리즘 기초)
* Stack (스택)
* Queue
* Tree
* Graph
* Sorting / Searching

---

# 1️⃣ Pointer (포인터)

포인터는 **변수의 메모리 주소(address)**를 저장하는 변수입니다.

```c
#include<stdio.h>

int main() {

    int A;
    A = 1004;

    printf("-------------------\n");
    printf("%d\n주소=%p", A, &A);
    printf("\n-------------------");

    return 0;
}
```

### 핵심 개념

* `&A` → 변수 A의 메모리 주소
* `%p` → 주소 출력

---

# 2️⃣ Struct (구조체)

구조체는 **여러 데이터를 하나의 자료형으로 묶는 사용자 정의 자료형**입니다.

```c
#define _CRT_SECURE_NO_WARNINGS
#include <stdio.h>
#include <string.h>

struct student {
    char name[15];
    int grade;
};

int main() {

    struct student s;

    strcpy(s.name, "JIAN");
    s.grade = 100;

    printf("%s\n%d", s.name, s.grade);

    return 0;
}
```

---

# 3️⃣ Struct Initialization (구조체 초기화)

구조체 생성과 동시에 값을 초기화할 수 있습니다.

```c
#include <stdio.h>

struct student {
    char name[15];
    float grade;
};

int main() {

    struct student s1 = { "홍길동", 99.9 };

    printf("이름 : %s\n성적 : %.1f", s1.name, s1.grade);

    return 0;
}
```

### 핵심

* `{ "홍길동", 99.9 }` → 구조체 초기화
* `%.1f` → 소수점 자리 지정

---

# 4️⃣ scanf 입력

사용자로부터 데이터를 입력받는 예제입니다.

```c
#include <stdio.h>

struct student {
    char name[15];
    int grade;
};

int main() {

    struct student s;

    scanf("%s", s.name);
    scanf("%d", &s.grade);

    printf("%s %d", s.name, s.grade);
}
```

---

# 5️⃣ Algorithm Example (최대값 찾기)

배열에서 **가장 큰 값을 찾는 알고리즘**입니다.

```c
#include <stdio.h>

int main() {

    int score[5] = { 100, 80, 70, 60, 50 };
    int i, tmp;

    tmp = score[0];

    for (i = 1; i < 5; i++) {
        if (score[i] > tmp) {
            tmp = score[i];
        }
    }

    printf("%d", tmp);

    return 0;
}
```

### 알고리즘 설명

1. 배열의 첫 번째 값을 최대값으로 설정
2. 배열을 순회하면서 값을 비교
3. 더 큰 값이 발견되면 최대값을 변경
4. 최종 최대값 출력

---

# 6️⃣ Stack 개념

스택(Stack)은 **LIFO 구조**를 가진 자료구조입니다.

```
Push (입력)
1 → 2 → 3

Pop (출력)
3 → 2 → 1
```

즉 **Last In First Out** 구조입니다.

---

# 🖥 Development Environment

* Language : **C**
* IDE : **Visual Studio 2022**

---

# ✍️ Author

작성자 : **박지안**
