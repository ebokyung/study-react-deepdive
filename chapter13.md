# 13장. 웹페이지의 성능을 측정하는 다양한 방법
앞서 살펴본 지표를 포함해 웹페이지의 성능을 확인할 수 있는 다양한 지표를 측정하는 방법을 살펴보자

## 13.1 애플리케이션에서 확인하기
### 13.1.1 create-react-app
reportWebVitals 함수는 웹에서 성능을 측정하기 위한 함수다. 
> CLS, FID, FCP, LCP, TTFB를 측정하는 용도로 사용된다.

PerformanceObserver API를 제공하지 않는 브라우저에서는 web-vitals의 도움을 받아 성능을 측정하기 어렵다.

### 13.1.2 create-next-app
기본적으로 Next.js는 성능 측정을 할 수 있는 메서드인 NextWebVitalsMetric을 제공한다. _app.tsx 파일에 다음과 같이 코드를 추가해서 사용해보자.

```ts
import type { AppProps, NextWebVitalsMetric } from "next/app"

export function reportWebVitals(metric: NextWebVitalsMetric) {
  console.log(metric)
}

function MyApp({ Component, pageProps }: AppProps) {
  return <Component {...pageProps} />
}

export default MyApp
```

next.js에 특화된 사용자 지표도 제공한다. 시간 단위는 `밀리초(ms)`이다.
- Next.js-hydration: 페이지가 서버 사이드에서 렌더링되어 하이드레이션하는 데 걸린 시간
- Next.js-route-change-to-render: 페이지가 경로를 변경한 후 페이지를 렌더링을 시작하는 데 걸리는 시간
- Next.js-render: 경로 변경이 완료된 후 페이지를 렌더링하는 데 걸린 시간


## 13.2 구글 라이트하우스
구글 라이트하우스로 코드 수정이나 배포, 수집 없이도 지표를 수집할 수 있다. 웹 페이지 성능 측정 도구로, 오픈소스로 운영되며 접근성, PWA, SEO등 웹 페이지를 둘러싼 다양한 요소들을 측정하고 점검할 수 있다.

### 13.2.1 구글 라이트하우스 - 탐색 모드
페이지가 로딩되면서 지표 측정

#### 성능
> FCP, LCP, CLS + TTI, Speed Index, Total Blocking Time

TTI
- 페이지에서 사용자가 완전히 상호작용할 수 있을 때까지 걸리는 시간을 측정한다.
- 3.8초 이내: 좋음
- 7.3초 이내: 보통
- 이후: 개선 필요

Speed Index
- 페이지가 로드되는 동안 콘텐츠가 얼마나 빨리 시각적으로 표시되는지를 계산한다.
- 3.4초 이내: 좋음
- 5.8초 이내: 보통
- 이후: 느림

Total Blocking Time
- 메인 스레드에서 특정 시간 이상 실행되는 작업, 즉 긴 작업(50ms 이상)이 수행될 때마다 메인 스레드가 차단된 것으로 간주한다. 이렇게 실행되는 긴 작업을 모아서 총 차단 시간이라고 하며, 이는 각각의 긴 작업의 시간에서 50ms를 뺀 다름, 이를 모두 합해 계산한다.
- FCP ~ TTI 사이의 작업만 대상으로한다.

#### 접근성
여기서 말하는 웹 접근성은 장애인 및 고령자 등 신체적으로 불편한 사람들이 일반적인 사용자와 동등하게 웹페이지를 이용할 수 있도록 보장하는 것이다.

예를 들어, 시각으로 웹페이지를 보기 어려운 경우, 스크린 리더의 툴을 활용하면 웹페이지의 내용을 직접 듣는것이 가능하다.

그러나 그림이나 사진의 경우, 스크린 리더가 읽을 수는 없으므로 대체 문자가 필요하다. 오디오나 비디오는 청각이 제한적인 경우를 위해 자막이 필요하며, 마우스를 활용할 수 없는 상황에 대비하기 위해서는 키보드만으로 모든 콘텐츠에 접근할 수 있어야 한다. 이러한 다양한 사용자를 배려하기 위해HTML과 CSS 등에 적잘한 대안을 삽입하는 것을 접근성이라 한다.


#### 권장사항
보안, 표준 모드, 최신 라이브러리, 소스맵 등 웹 사이트를 개발할 때 고려해야 할 요소들을 얼마나 지키고 있는지 확인할 수 있다.

- CSP가 XSS 공격에 효과적인지 확인
    - XSS란, Cross Site Scripting의 약자로, 개발자가 아닌 제3자가 삽입한 스크립트를 통해 공격하는 기법
    - CSP란, Content Security Policy의 약자로, 웹 사이트에서 호출할 수 있는 컨텐츠를 제한하는 정책을 말한다. 이 제한 정책에는 이미지, 스타일, 스크립트와 같은 정적 콘텐츠뿐만 아니라 주소, 도메인 등의 정보도 포함된다.
- 감지된 JavaScript 라이브러리
- HTTPS 사용
- 페이지 로드 시 위치정보 권한 요청 방지하기
- 페이지 로드 시 알림 권한 요청 방지하기
- 알려진 보안 취약점이 있는 프런트엔드 자바스크립트 라이브러리를 사용하지 않음
- 사용자가 비밀번호 입력란에 붙여넣을 수 있도록 허용
- 이미지를 올바른 가로세로 비율로 표시
- 이미지가 적절한 해상도로 제공됨
- 페이지에 HTML Doctype 있음
- 문자 집합을 제대로 정의함
- 지원 중단 API 사용하지 않기
- 콘솔에 로그된 브라우저 오류 없음
- Chrome Devtools의 Issues 패널에 문제없음
- 페이지에 유효한 소스 맵이 있음
- font-display

#### 검색엔진 최적화
문서를 크롤링하기 쉽게 만들었는지 확인하는 것부터, robots.tsx가 유효한지, 이미지와 링크에 설명 문자가 존재하는지, `<meta>`나 `<title>` 등으로 페이지의 정보를 빠르게 확인할 수 있는지 등을 확인한다.


### 13.2.2 구글 라이트하우스 - 기간 모드
실제 웹 페이지를 탐색하는 동안 지표 측정

기간 모드 시작을 누른 뒤 성능 측정을 원하는 작업을 수행한 다음, 기간 모드를 종료하면 그 사이에 일어난 작업들에 대한 지표를 다음과 같이 확인할 수 있다.

#### 흔적


#### 트리맵
페이지를 불러올 때 함께 로딩한 모든 리소스를 함께 모아서 볼 수 있는 곳이다. 웹페이지의 전체 JS리소스 중 어떤 파일이 전체 데이터 로딩 중 어느 정도를 차지했는지를 비율로 확인할 수 있으며, 실제 불러온 데이터의 크기를 확인할 수도 있다. 로딩한 리소스에서 사용하지 않은 바이트의 크기를 확인할 수도 있다.