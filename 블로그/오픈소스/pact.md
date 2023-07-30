# Contract Testing 

##
모놀리딕 서비스 플랫폼에서 마이크로 서비스 환경으로 구성되면 
자연스럽게 EndServer to EndServer 데이터, 이벤트를 주고받기 위해서는 HTTP 프로토콜을 사용하게됩니다.
Request, Response 으로 구성된 Rest API로 서로 약속된 계약관계 API를 다양한 방법으로 제공합니다.

Swagger API, Spring Doc API, Rest DOC 등 
다양한언어에서 E2E 통신을 위한 API 문서를 제공합니다. 

서버 통신을 진행하게되면 소비자(Consumer) 공급자(Provider)
소비자(Frontend), 공급자(Backend)
소비자(Backend) 공급자(Backend)

## Pact 흐름도
Pact 오픈라이브러리는 HTTP 및 메시지 통합 테스트하는코드 우선 도구 입니다.


![스크린샷 2023-07-30 오후 2 43 23](https://github.com/lswteen/blog/assets/3292892/ff1ff4f7-d414-4384-8e27-3d10cbd4da6f)


## Pact 특징