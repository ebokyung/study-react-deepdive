# 02장 리액트 핵심 요소 깊게 살펴보기
책을 읽기 전에 자바스크립트에 대한 기본적인 지식을 다루는 장입니다. 이 장에서는 리액트를 사용하면서 자주 사용하는 자바스크립트 문법과 개념을 간단히 정리합니다.

## 2.1 JSX란?
- XML과 유사한 내장형 구문
- 리액트에 종속적이지 않은 독자적인 문법
- 자바스크립트 엔진이나 브라우저에 의해서 실행되는 구문이 아니기 때문에 트랜스파일러을 거쳐야함

자바스크립트 내부에서 표현하기 까다로웠던 XML 스타일의 트리 구문을 작성하는 것을 돕는 문법

### 2.1.1 JSX의 정의
4가지 컴포넌트를 기반으로 구성(JSXElement, JSXAttributes, JSXChildren, JSXStrings)

### `JSXElement`
JSX를 구성하는 가장 기본 요소로, HTML의 요소(element)와 비슷한 역할을 한다. JSXElement가 되기 위해서는 다음과 같은 형태 중 하나여야 한다.

- JSXOpeningElement
- JSXClosingElement
- JSXSelfClosingElement
- JSXFragment

#### `JSXElementName`
JSXElement의 요소 이름으로 쓸 수 있는 것

- JSXIdentifier
- JSXNamespacedName
- JSXMemberExpression

### `JSXAttributes`
JSXElement에 부여할 수 있는 속성을 의미한다. 모든 경우에서 필수값이 아니고, 존재하지 않아도 된다

- JSXSpreadAttributes: JS의 전개 연산자와 동일한 역할을 한다.
- JSXAttribute: 속성을 나타내는 키(JSXAttributeName)와 값(JSXAttributeValue)으로 짝을 이루어서 표현한다.

### `JSXChildren`
JSXElement의 자식 값을 나타낸다.

JSXChild: JSXChildren을 이루는 기본 단위로, JSXChildren은 JSXChild을 0개 이상 가질 수 있다.

- JSXText
- JSXElement
- JSXFragment
- {JSXChildExpression (optional)}

### `JSXStrings`
HTML에서 사용 가능한 문자열은 모두 JSXStrings에서도 가능하다. 이는 개발자가 HTML의 내용을 손쉽게 JSX로 가져올 수 있도록 의도적으로 설계된 부분이다.

### 2.1.2 JSX 예제
