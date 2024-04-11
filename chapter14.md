# 14장. 웹사이트 보안을 위한 리액트와 웹페이지 보안 이슈

- [14.1 리액트에서 발생하는 크로스 사이트 스크립팅(XSS)](#14.1-리액트에서-발생하는-크로스-사이트-스크립팅(XSS))
    - [14.1.1 dangerouslySetInnerHTML prop](#14.1.1-dangerouslySetInnerHTML-prop)
    - [14.1.2 useRef를 활용한 직접 삽입](#14.1.2-useRef를-활용한-직접-삽입)
    - [14.1.3 리액트에서 XSS 문제를 피하는 방법](#14.1.3-리액트에서-XSS-문제를-피하는-방법)
- [14.2 getServerSideProps와 서버 컴포넌트를 주의하자](#14.2-getServerSideProps와-서버-컴포넌트를-주의하자)
- [14.3 태그의 값에 적절한 제한을 둬야 한다](#14.3-태그의-값에-적절한-제한을-둬야-한다)
- [14.4 HTTP 보안 헤더 설정하기](#14.4-HTTP-보안-헤더-설정하기)
    - [14.4.1 Strict-Transport-Security](#14.4.1-Strict-Transport-Security)
    - [14.4.2 X-XSS-Protection](#14.4.2-X-XSS-Protection)
    - [14.4.3 X-Frame-Options](#14.4.3-X-Frame-Options)
    - [14.4.4 Permissions-Policy](#14.4.4-Permissions-Policy)
    - [14.4.5 X-Content-Type-Options](#14.4.5-X-Content-Type-Options)
    - 14.4.6 Referrer-Policy
    - 14.4.7 Content-Security-Policy
    - 14.4.8 보안 헤더 설정하기
    - 14.4.9 보안 헤더 확인하기
- 14.5 취약점이 있는 패키지의 사용을 피하자
- 14.6 OWASP Top 10
- 14.7 정리

## 14.1 리액트에서 발생하는 크로스 사이트 스크립팅(XSS)
웹 애플리케이션에서 가장 많이 보이는 취약점 중 하나로, 제 3자가 웹사이트에 악성 스크립트를 삽입해 실행할 수 있는 취약점을 의미한다. 이 취약점은 주로 게시판 같이 사용자가 입력할 수 있고, 입력을 다른 사용자에게 보여줄 수 있는 경우에 발생한다.

예를들어 사용자가 다음과 같은 글을 올린다고 가정하자
```
<p>사용자가 글을 작성했습니다.</p>
<script>
  alert('CSS)
</script>
```
위의 글을 방문했을 때 별도의 조치가 없다면 script도 같이 실행돼서 alert가 실행될 것이다. script가 실행될 수 있다면 웹사이트 개발자가 할 수 있는 모든 작업을 수행할 수 있으며, 쿠키를 획득해 사용자의 로그인 세션 등을 탈취하거나 사용자의 데이터를 변경하는 등의 위험성이 있다.

리액트에서 XSS이슈가 발생하는 경우를 알아보자

### 14.1.1 `dangerouslySetInnerHTML prop`
특정 부라우저 DOM의 innerHTML을 특정한 내용으로 교체할 수 있는 방법이다. 일반적으로 사용자나 관리자가 입력한 내용을 브라우저에 표시하는 용도로 사용된다
```
function App() {
  return <div dangerousltSetInnerHTML={{ __html: "First &middot; Second" }} />;
}
```

dangerousltSetInnerHTML prop의 __html 키를 통해 받는 문자열에는 제한이 없기 때문에 사용에 주의를 기울여야 하며 여기에 넘겨주는 문자열 값은 한 번 더 검증이 필요하다.

### 14.1.2 useRef를 활용한 직접 삽입
dangerousltSetInnerHTML과 비슷한 방법으로 DOM에 직접 내용을 삽입할 수 있는 방법으로 useRef가 있다. useRef를 활용하면 직접 DOM에 접근할 수 있으므로 innerHTML에 보안 취약점이 있는 스크립트를 삽입하면 동일한 문제가 발생한다.

### 14.1.3 리액트에서 XSS 문제를 피하는 방법
제3자가 삽입할 수 있는 HTML을 **안전한 HTML 코드로 한 번 치환**하는 것이다. 이러한 과정을 새니타이즈(sanitize)나 이스케이프(escape)라고 한다. 새니타이즈를 직접 구현해 사용할 수 있지만 라이브러리를 사용하는 것이 가장 확실한 방법이다.
- DOMputify
- sanitize-html: 허용할 태그와 목록을 일일히 나열하는 이른바 허용 목록 방식(allow list)
- js-xss

단순히 보여줄 때뿐만 아니라 사용자가 콘텐츠를 저장할 때도 한번 이스케이프 과정을 거치는 것이 더 효율적이고 안전하다. 이러한 치환 과정은 되도록 서버에서 수행하는 것이 좋다. 일반적인 사용자라면 클라이언트에서만 수행해도 문제가 되지 않겠지만, POST 요청을 스크립트나 curl 등으로 직접 요청하는 경우에는 스크립트에서 실행하는 이스케이프 과정을 생략하고 바로 저장될 가능성이 있다.

(dangerouslySetInnerHTML가 별도로 존재하는 이유는, 리액트에서 HTML에 직접 표시되는 값에 대해서는 기본적으로 이스케이프 작업을 해주는데, 개발자의 활용도에 따라 원본 값이 필요할 수 있기 때문에 dangerouslySetInnerHTML나 props로 넘겨받는 경우 이러한 작업이 수행되지 않도록 하기 위해서이다.)

## 14.2 `getServerSideProps`와 서버 컴포넌트를 주의하자
SSR과 서버 컴포넌트는 성능 이점과 서버의 개발 환경을 프론트엔드 개발자가 쥐게 됐다. 서버에는 일반 사용자에게 노출되면 안 되는 정보들이 담겨있기 때문에 클라이언트, 즉 브라우저에 정보를 내려줄 때는 조심해야 한다.

getServerSideProps가 반환하는 props 값은 모두 사용자의 HTML에 기록되고, 또한 전역 변수로 등록되어 보안 위협에 노출되는 값이 된다. 예를 들어 cookie 정보에 따라 리다이렉트 하는 경우, 클라이언트에서 필요한 값만 제한적으로 반환하고, 이 값이 없을 때 예외 처리(리다이렉트)도 서버에서 처리하는 것이 보안과 성능면에서 좋다.

## 14.3 `<a>` 태그의 값에 적절한 제한을 둬야 한다
`<a>` 태그의 href 내에 javascript:로 시작하는 자바스크립트 코드를 넣어둔 경우를 본 적이 있을 것이다. href 내에 자바스크립트 코드가 존재하면 이를 실행하기 때문에 해당 URL 페이지 이동을 막거나 별도 이벤트 핸들러만 작동시키기 위한 용도로 주로 사용된다.
```
return <a href="javascript:alert('hi');" onClick={handleClick}>링크</a>;
```
XSS에서 다룬 내용과 비슷하게, href에 사용자가 입력한 주소를 넣는 등의 보안 이슈를 대비해 href로 들어갈 수 있는 값을 제한해야 한다. 그리고 피싱 사이트로 이동하는 것을 막기 위해 가능하다면 origin도 확인해 처리하는 것이 좋다.

## 14.4 HTTP 보안 헤더 설정하기
HTTP 보안 헤더란 브라우저가 렌더링하는 내용과 관련된 보안 취약점을 미연에 방지하기 위해 브라우저와 함께 작동하는 헤더를 의미한다. HTTP 보안 헤더만 효율적으로 사용할 수 있어도 많은 보안 취약점을 방지할 수 있다.

### 14.4.1 Strict-Transport-Security
모든 사이트가 HTTPS를 통해 접근해야 하며, 만약 HTTP로 접근하는 경우 모든 시도는 HTTPS로 변경되게 한다.

### 14.4.2 X-XSS-Protection
비표준 기술로, 현재 사파리와 구형 브라우저에서만 제공되는 기능이다. 이 헤더는 페이지에서 XSS 취약점이 발견되면 페이지 로딩을 중단하는 헤더다.

### 14.4.3 X-Frame-Options
페이지를 frame, iframe, embed, object 내부에서 렌더링을 허용할지를 나타낼 수 있다. 

네이버와 비슷한 주소를 가진 페이지에서 네이버를 iframe으로 렌더링한다고 가정해보자. 사용자는 이 페이지를 진짜 네이버로 오해할 수도 있고, 공격자는 사용자의 개인정보를 탈취할 수 있다.

### 14.4.4 Permissions-Policy
웹 사이트에서 사용할 수 있는 기능과 사용할 수 없는 기능을 명시적으로 선언하는 헤더다.

[MDN에서 제어할 수 있는 기능 확인하기](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Permissions-Policy#browser_compatibility)
[기능 선택해 헤더 만들기](https://www.permissionspolicy.com/)

### 14.4.5 X-Content-Type-Options
Content-type 헤더에서 제공하는 MIME 유형이 브라우저에 의해 임의로 변경되지 않게 하는 헤더다. 즉, 웹서버가 브라우저에 강제로 이 파일을 읽는 방식을 지정하는 것이다.

MIME(Multipurpose Internet Mail Extensions)은 메일을 전송할 때 사용하던 인코딩 방식이었지만 현재는 content-type의 값으로 대표적으로 사용되고 있다. jpg, CSS, JSON 등 다양하다.

