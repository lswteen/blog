#검색 SEO

검색 SEO는 Google, Naver 2개의 사용자가 많은 검색엔진에 노출하고자하는  
Domain (서비스 플랫폼, 블로그, 이커머스, 컨텐츠 등등등)을
언급한 2개의 검색 포털 가이드를 기반으로 경쟁서비스와 차별화되어 랭킹을 올릴수있는 다양한 방법을  
무수히 많은 방법으로 최적화 하는 프로세스 입니다.

## 다양한 SEO 2-3 년째 삽질하며 느낀 내용을 기반으로 정리합니다.
- [구글 검색엔진 최적화](https://developers.google.com/search/docs/fundamentals/seo-starter-guide?hl=ko)
- [네이버 검색엔진 최적화](https://searchadvisor.naver.com/guide/seo-basic-intro)

## youtube 컨텐츠 랭킹 상위 노출 
![img_4.png](img_4.png)
![img_5.png](img_5.png)
![img_6.png](img_6.png)

## Blog 컨텐츠 키워드 분석 및 상위 노출 방법
![img_7.png](img_7.png)
![img_9.png](img_9.png)
![img_10.png](img_10.png)
![img_11.png](img_11.png)

##검색엔진 최적화(SEO) 기본 가이드

참조 링크 : https://developers.google.com/search/docs/fundamentals/seo-starter-guide?hl=ko

### 직관적 URI
<img width="100%" alt="검색 SEO URI 이미지" src="https://github.com/lswteen/blog/assets/3292892/a2774700-e1ad-430b-8494-8a78cb29eed4">

```text
1) Query String으로 구성된 패턴
   https://프로이직러.co.kr/list?page=1
   https://프로이직러.co.kr/keyword?abc

2) Path Variable
   https://프로이직러.co.kr/list/1
   https://프로이직러.co.kr/keyword/abc

구글에서는 동일한 컨텐츠를제공하는 URI구성 정보라면 직관적 URI를 선호합니다.
```

### 문자, 숫자 URI
```text
티스토리에서는 아래와같이 URI를 숫자, 문자 2가지 방식으로 제공합니다.

숫자, 문자 또한 검색 SEO노출에 차이를 보이기때문에 이런부분도 고려해서 변경해보고 모니터링하면서 높아지는 우선순위 방법으로 진행하시면됩니다.

아무래도 한글 Text경우 경험상 SEO 수치가 좋아지는 느낌은 아닙니다.
물론 검색하는 End User 타켓이 한국일경우 예외입니다.

무조건 문자가 좋다 숫자가 좋다보다는 이부분도 Case를 고려해서 적용이 필요합니다.
```

### URI Depth
```text
단순 디렉토리 구조

아래 2링크모두 원픽!!
어떤링크를 주타켓 링크로 할껀지 ...

https://www.abc.co.kr/user/info
https://www.abc.co.kr/user/info?Page=1&PageSize=20&Ord=task-01

URI에 /.../.../.../... 최대한 Depth를 들어가지 않도록 전달될수있는 구성으로 최소화하여 SEO에 노출된 도메인 랭킹 상승으로
```

### Sub Domain VS Sub Folder

#### 서브도메인 예시:
- news.performars.com
- blog.performars.com 
- event.performars.com

서브 도메인은 각각의 도메인이므로 새로운 토픽을 관리할 때 사용됩니다.
SEO 점수가 호스트 도메인에 흡수가 되지 않는다는 견해도 존재합니다.
Call to action 있는 랜딩 페이지 하나만 관리하고 싶으면 서브도메인이 편리합니다. ( 예: Hubspot)
하지만 서비스 용도별로 많은 sub가 구성될경우 관리 복잡도가 증가합니다.

#### 서브폴더 예시: 
- performars.com/news
- performars.com/blog
- performars.com/event

서브 폴더는 여러 서비스들을 호스트 도메인 아래로 연결 시켜서 관리하는 구조입니다.
여러 서브 폴더는 하나의 호스트 도메인에 속해 있는 SEO 점수가 루트 도메인으로 흡수된다고 합니다.
서브 폴더의 내용은 주로 전달하고자하는 의도가 명확히 들어간 도메인과 연관성이 강하게 있으면 좋다고 합니다.
관리가 편하다 SEO ranking 을 위해서는 서브폴더가 유리할 수 있다
랭킹상승 목적이라면 서브폴더를 추천드립니다.

### Sitemap

사이트맵이란 웹사이트에서 구글이나 네이버와 같은 검색 엔진에 색인할 모든 페이지를 나열한 XML 파일로,
웹사이트에 방문하는 검색엔진 크롤러에게 지도와 같은 역할을 합니다.
다시 말해 사이트맵은 색인이 되어야 할 모든 페이지의 목록을 제공함으로써 검색엔진 크롤러가 발견하는데
어려움을 겪는 페이지도 문제없이 크롤링되고 색인될 수 있게 해주는 파일이라고 이해할 수 있습니다.

아래 URI에서 google Search Console 에 들어가는 .xml을 생성하며
만들어지는 기준은 마케팅 통계 기반데이터를 구성으로 생성되어야 합니다.

- https://www.xml-sitemaps.com/
- https://www.screamingfrog.co.uk/seo-spider/








