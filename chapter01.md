# 01장 리액트 개발을 위해 꼭 알아야 할 자바스크립트 

- [1.1 자바스크립트의 동등 비교](#1.1-자바스크립트의-동등-비교)
    - [1.1.1 자바스크립트의 데이터 타입](#1.1.1-자바스크립트의-데이터-타입)
    - 1.1.2 값을 저장하는 방식의 차이
    - 1.1.3 자바스크립트의 또 다른 비교 공식, Object.is
    - 1.1.4 리액트에서의 동등 비교
- 1.2 함수
    - 1.2.1 함수란 무엇인가?
    - 1.2.2 함수를 정의하는 4가지 방법
    - 1.2.3 다양한 함수 살펴보기
    - 1.2.4 함수를 만들 때 주의해야 할 사항
- 1.3 클래스
- 1.4 클로저
    - 1.4.1 클로저의 정의
    - 1.4.2 변수의 유효 범위, 스코프
    - 1.4.3 클로저의 활용
    - 1.4.4 주의할 점
- 1.5 이벤트 루프와 비동기 통신의 이해
    - 1.5.1 싱글 스레드 자바스크립트
    - 1.5.2 이벤트 루프란?
    - 1.5.3 태스크 큐와 마이크로 태스크 큐
- 1.6 리액트에서 자주 사용하는 자바스크립트 문법
    - 1.6.1 구조 분해 할당
    - 1.6.2 전개 구문
    - 1.6.3 객체 초기자
    - 1.6.4 Array 프로토타입의 메서드: map, filter, reduce, forEach
    - 1.6.5 삼항 조건 연산자
- 1.7 선택이 아닌 필수, 타입스크립트
    - 1.7.1 타입스크립트란?
    - 1.7.2 리액트 코드를 효과적으로 작성하기 위한 타입스크립트 활용법
    - 1.7.3 타입스크립트 전환 가이드


## 1.1 자바스크립트의 동등 비교
리액트의 가상 DOM과 실제 DOM의 비교, 리액트 컴포넌트가 렌더링할지를 판단하는 방법, 벼수나 함수의 메모이제이션 등 모든 작업은 자바스크립트의 동등 비굘르 기반으로 한다.
### 1.1.1 자바스크립트의 데이터 타입
자바스크립트의 데이터 타입은 `원시 타입`과 `객체 타입`으로 나뉜다.

#### 원시 타입
1. Undefined 
    - 선언한 후 값을 할당하지 않은 변수
    - 값이 주어지지 않은 인수에 자동으로 할당되는 값
2. Null 
    - 아직 값이 없거나 비어 있는 값
    - typeof null 은 'object'가 출력된다는 것 주의 (호환성 이유)
> undefined는 선언되었지만 할당되지 않은 값이고, null은 명시적으로 비어있음을 나타내는 값으로 사용
3. Boolean
    - 참(true), 거짓(false)만을 가질 수 있는 데이터타입
    - 조건문 내부에서 마치 true, false처럼 취급되는 truthy, falsy값이 존재
        ```
        // falsy: false, 0, -0, 0n, 0x0n, NaN, “”, ‘’, `` (공백이 없는 빈 문자열), null, undefined
        // truthy: falsy외에 모든 것이 truthy (객체와 배열은 내부 값 존재 여부와 상관없이 truthy임 주의)
        ```
4. Number 
    - `-(2^53-1)`과 `2^53-1` 사이의 값 저장
    - 진수별로 값을 표현해도 모둔 10진수로 해석되어 동일한 값으로 표시
5. Bigint 
    - Number가 다룰 수 있는 숫자 크기의 제한을 극복하기 위해 등장
    - 끝에 n을 붙이거나 BigInt 함수를 사용
6. String 
    - 백틱을 사용해서 표현한 문자열을 템플릿 리터럴이라고 하는데, 같은 문자열을 반환하지만 줄바꿈이 가능하고, 문자열 내부에 표현식을 쓸 수 있다는 차이가 있음
    - 문자열은 원시 타입이므로 변경 불가능 (index 접근은 가능하지만 값을 수정할 수는 없음)
7. Symbol 
    - ES6, 중복되지 않는 고유한 값
    - 심벌 함수 `Symbol()`을 사용해야 만들 수 있음

#### 객체 타입
- 7가지 원시 타입 이외의 모든 것이 객체타입이다. 즉 자바스크립트를 이루고 있는 대부분의 타입이 객체 타입에 속한다.
- 배열, 함수, 정규식, 클래스 등이 여기에 포함된다.


### 1.1.2 값을 저장하는 방식의 차이
### 1.1.3 자바스크립트의 또 다른 비교 공식, Object.is
### 1.1.4 리액트에서의 동등 비교
## 1.2 함수
### 1.2.1 함수란 무엇인가?
### 1.2.2 함수를 정의하는 4가지 방법
### 1.2.3 다양한 함수 살펴보기
### 1.2.4 함수를 만들 때 주의해야 할 사항
## 1.3 클래스
## 1.4 클로저
### 1.4.1 클로저의 정의
### 1.4.2 변수의 유효 범위, 스코프
### 1.4.3 클로저의 활용
### 1.4.4 주의할 점
## 1.5 이벤트 루프와 비동기 통신의 이해
### 1.5.1 싱글 스레드 자바스크립트
### 1.5.2 이벤트 루프란?
### 1.5.3 태스크 큐와 마이크로 태스크 큐
## 1.6 리액트에서 자주 사용하는 자바스크립트 문법
### 1.6.1 구조 분해 할당
### 1.6.2 전개 구문
### 1.6.3 객체 초기자
### 1.6.4 Array 프로토타입의 메서드: map, filter, reduce, forEach
### 1.6.5 삼항 조건 연산자
## 1.7 선택이 아닌 필수, 타입스크립트
### 1.7.1 타입스크립트란?
### 1.7.2 리액트 코드를 효과적으로 작성하기 위한 타입스크립트 활용법
### 1.7.3 타입스크립트 전환 가이드

## 1.1 자바스크립트의 동등 비교