---
tags:
  - database
  - normalization
  - cs
---

# 데이터베이스 정규화 (Database Normalization): 1NF부터 BCNF까지

데이터베이스 정규화는 관계형 데이터베이스에서 **데이터 중복을 최소화**하고 **데이터 무결성(Data Integrity)을 개선**하기 위해 테이블과 컬럼을 구조화하는 프로세스입니다. 각 정규형(Normal Form)은 이전 단계의 규칙을 포함하며 더 엄격한 규칙을 적용합니다.

---

## 제1정규형 (1NF): 원자성 보장 (Ensure Atomic Values)

1NF는 정규화의 가장 기초적인 단계로, 관계형 데이터베이스 테이블이 되기 위한 최소한의 요건입니다.

### 1NF 규칙
1.  **원자성(Atomicity):** 테이블의 모든 셀(컬럼 값)은 단일 값(Single Value)만 가져야 합니다. 리스트, 배열, 또는 반복 그룹(Repeating Groups)을 포함할 수 없습니다.
2.  **유일성:** 각 행(Row)은 유일해야 하며, 일반적으로 기본키(Primary Key)를 통해 식별됩니다.
3.  **동일 타입:** 한 컬럼의 모든 값은 동일한 데이터 타입을 가져야 합니다.

### 예시: 주문 시스템 (Non-1NF → 1NF)

다음과 같은 주문 추적 테이블이 있다고 가정해 봅시다.

**1NF 위반 테이블 (Before)**

| OrderID | CustomerName | Products |
| :--- | :--- | :--- |
| 101 | Alice | `Laptop, Mouse` |
| 102 | Bob | `Keyboard` |
| 103 | Charlie | `Monitor, Webcam, Keyboard` |

**문제점:** `Products` 컬럼이 원자적이지 않습니다. 쉼표로 구분된 리스트가 하나의 셀에 들어가 있어, "Keyboard를 산 사람을 모두 찾아라"와 같은 쿼리를 작성하기 어렵습니다.

**해결 (1NF 적용):** 각 제품마다 별도의 행을 생성하여 모든 셀이 하나의 값만 가지도록 합니다.

**1NF 적용 테이블 (After)**

| OrderID | CustomerName | Product |
| :--- | :--- | :--- |
| 101 | Alice | Laptop |
| 101 | Alice | Mouse |
| 102 | Bob | Keyboard |
| 103 | Charlie | Monitor |
| 103 | Charlie | Webcam |
| 103 | Charlie | Keyboard |

이제 모든 셀이 원자적(Atomic)이므로 1NF를 만족합니다.

---

## 제2정규형 (2NF): 부분 종속성 제거 (Eliminate Partial Dependencies)

2NF는 **복합키(Composite Primary Key)**를 가진 테이블에서 발생하는 중복을 제거하는 데 중점을 둡니다.

### 2NF 규칙
1.  테이블이 **1NF**를 만족해야 합니다.
2.  **부분 종속성(Partial Dependency)**이 없어야 합니다. 즉, 모든 키가 아닌 속성(Non-key attribute)은 기본키의 **전체**에 의존해야지, 일부분에만 의존해서는 안 됩니다.

> **참고:** 기본키가 단일 컬럼으로 구성된 테이블은 1NF라면 자동으로 2NF를 만족합니다.

### 예시: 주문 상세 (1NF → 2NF)

위의 1NF 테이블에서 각 행을 유일하게 식별하기 위해 복합키 `{OrderID, Product}`를 기본키로 설정했다고 가정해 봅시다. 그리고 몇 가지 정보를 더 추가합니다.

**2NF 위반 테이블 (Before)**

| OrderID (PK) | Product (PK) | CustomerName | OrderDate | ProductName |
| :--- | :--- | :--- | :--- | :--- |
| 101 | P01 | Alice | 2025-06-10 | Laptop |
| 101 | P02 | Alice | 2025-06-10 | Mouse |
| 102 | P03 | Bob | 2025-06-11 | Keyboard |

**문제점 (부분 종속성):**
- 기본키는 `{OrderID, Product}`입니다.
- `CustomerName`과 `OrderDate`는 오직 `OrderID`에만 종속됩니다. (키의 일부에만 종속)
- `ProductName`은 오직 `Product`에만 종속됩니다. (키의 일부에만 종속)

이로 인해 "Alice"라는 이름과 주문 날짜가 같은 주문 내의 모든 제품마다 중복 저장됩니다.

**해결 (2NF 적용):** 테이블을 분해(Decomposition)하여 부분 종속성을 제거합니다.

**결과 (3개의 테이블로 분리)**

1.  **Orders 테이블:** `OrderID`에만 종속되는 정보 (CustomerName, OrderDate)
2.  **Products 테이블:** `Product`에만 종속되는 정보 (ProductName)
3.  **Order_Details 테이블:** 두 엔티티를 연결하는 정보 (`OrderID`, `Product`)

이렇게 하면 데이터 중복이 사라지고, 고객 이름이 바뀌어도 한 곳만 수정하면 됩니다.

---

## 제3정규형 (3NF): 이행적 종속성 제거 (Eliminate Transitive Dependencies)

3NF는 키가 아닌 속성들 간의 종속성을 제거합니다.

### 3NF 규칙
1.  테이블이 **2NF**를 만족해야 합니다.
2.  **이행적 종속성(Transitive Dependency)**이 없어야 합니다. 즉, 키가 아닌 속성(Non-key attribute)은 오직 **기본키**에만 의존해야 하며, 다른 키가 아닌 속성에 의존해서는 안 됩니다.

### 이행적 종속성이란? (Transitive Dependency)
"친구의 친구" 관계와 같습니다.
- A → B (B는 A에 의존)
- B → C (C는 B에 의존)
- 결과적으로 A → C (C는 A에 간접적으로 의존)

여기서 C가 B(키가 아닌 속성)에 의존하는 것이 문제입니다.

### 예시: 수강 신청 (2NF → 3NF)

**3NF 위반 테이블**

| EnrollmentID (PK) | StudentID | CourseID | CourseName |
| :--- | :--- | :--- | :--- |
| 101 | S1 | C10 | Intro to SQL |
| 102 | S2 | C20 | Adv. DB |
| 103 | S3 | C10 | Intro to SQL |

**문제점:**
- 기본키는 `EnrollmentID`입니다.
- `CourseName`은 `CourseID`에 의존합니다.
- 하지만 `CourseID`는 기본키가 아닙니다.
- 즉, `EnrollmentID` → `CourseID` → `CourseName`의 이행적 종속성이 존재합니다.

이 경우 "Intro to SQL" 강의명이 바뀌면 여러 행을 모두 업데이트해야 하는 **Update Anomaly(수정 이상)**가 발생합니다.

**해결 (3NF 적용):** 이행적 종속을 일으키는 컬럼을 별도 테이블로 분리합니다.

**결과**

1.  **Enrollments 테이블:** `EnrollmentID` (PK), `StudentID`, `CourseID` (FK)
2.  **Courses 테이블:** `CourseID` (PK), `CourseName`

이제 `CourseName`은 `Courses` 테이블의 기본키인 `CourseID`에 직접 종속되므로 3NF를 만족합니다.

---

## BCNF (Boyce-Codd Normal Form): 더 엄격한 3NF

BCNF는 3NF의 특수한 예외 상황을 처리하기 위한 더 강력한 정규형입니다. 종종 "3.5NF"라고 불립니다.

### BCNF 규칙
모든 **비자명한 함수적 종속성(Non-trivial functional dependency) X → Y**에 대해, 결정자 **X는 반드시 슈퍼키(Superkey)**여야 합니다.

> **슈퍼키란?** 행을 유일하게 식별할 수 있는 속성들의 집합입니다. (기본키를 포함하는 더 넓은 개념)

대부분의 3NF 테이블은 이미 BCNF입니다. 하지만 **복합키의 일부가 다른 속성에 의해 결정되는 경우** 3NF이면서 BCNF가 아닐 수 있습니다.

### 예시: 교수와 과목 (3NF이지만 BCNF 위반)

**상황:**
- 한 학생은 여러 과목을 들을 수 있습니다.
- 각 과목은 여러 교수가 가르칠 수 있습니다.
- **하지만, 각 교수는 오직 하나의 과목만 담당합니다.**

**테이블 구조**

| StudentID (PK 일부) | Course (PK 일부) | Professor |
| :--- | :--- | :--- |
| S1 | Database | Prof. Smith |
| S2 | Database | Prof. Jones |
| S1 | Math | Prof. White |

- **기본키:** `{StudentID, Course}` (학생과 과목을 알면 담당 교수가 결정됨)
- **종속성:** `Professor → Course` (교수는 하나의 과목만 맡으므로, 교수를 알면 과목이 결정됨)

**문제점:**
- `Professor → Course` 종속성이 존재합니다.
- 여기서 결정자인 `Professor`는 슈퍼키가 아닙니다. (교수 이름만으로는 학생을 식별할 수 없음)
- 따라서 BCNF를 위반합니다.

**해결 (BCNF 적용):** 테이블을 분해합니다.

1.  **Professor_Subject 테이블:** `Professor` (PK), `Course`
2.  **Student_Professor 테이블:** `StudentID` (PK), `Professor` (PK)

이제 모든 결정자가 슈퍼키가 되었습니다.

---

## 요약 비교 (Summary)

| 정규형 | 주요 목표 | 규칙 | 해결하는 문제 |
| :--- | :--- | :--- | :--- |
| **1NF** | 원자성 확보 | 모든 값은 원자적(Atomic)이어야 함. | 반복 그룹, 리스트 데이터 제거 |
| **2NF** | 부분 종속 제거 | 1NF + 키가 아닌 속성은 기본키 **전체**에 의존해야 함. | 복합키의 일부에만 의존하는 중복 데이터 제거 |
| **3NF** | 이행 종속 제거 | 2NF + 키가 아닌 속성은 기본키에만 의존해야 함. | 키가 아닌 다른 속성에 의존하는 데이터 제거 |
| **BCNF** | 결정자 슈퍼키화 | 모든 결정자는 슈퍼키여야 함. | 3NF에서 해결되지 않는 특수한 종속성 문제 해결 |