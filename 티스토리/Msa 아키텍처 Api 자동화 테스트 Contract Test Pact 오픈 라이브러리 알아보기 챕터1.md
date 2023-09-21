서비스 플랫폼 클라우드 환경에서 많은 서버 Http 프로토콜을 이용한 이벤트, 데이터들에 대해서 지속적인 개발 운영 변경사항에 대해서 다양한 인터페이스를 테스트하기 위해서는 많은 리소스가 들어가게 됩니다

복잡도 있는 서버 구성(마이크로 서비스 아키텍처 또는 미니서비스)에서는 Api서버에서 제공하고 있는 Request, Response들에 대한 검증 모니터링이 자동화되면 서비스 프로덕트의 신뢰와 생산성 리소스 최소화를 기대할 수 있게 됩니다

또한 BFF(Backend For Frontend)와 같은 서버 구성시 이기종 언어로 인한 검증 Mock Server이 필요하거나 개발단계에서는 자주 Request, Response 변경 및 Mock Server까지 개발자가 진행하다 보면 비즈니스 개발 이상으로 많은 리소스가 발생하며 커뮤니케이션 비용에 대한 부분을 최소화할 수 있는 방법으로 오픈라이브러리 Pact를 소개드립니다


> 일반적인 Sever Api Test

[##_Image|kage@nUyg8/btspsVOUjlk/BN1TfYBKJaw5kDxsdLmkB0/img.jpg|CDM|1.3|{"originWidth":1179,"originHeight":989,"style":"alignCenter"}_##]

일정 압박으로 프런트앤드 개발자는 언제 완성될지 모르는 Api로 인하여 Method와 URI 정보 그리고 어떻게 바뀔지 모르는 Request, Response 등을 뇌피셜로 작업하게 됩니다

[##_Image|kage@dtd850/btspFlFTYuW/COU9Ith9lsZJ5vrvmteCoK/img.jpg|CDM|1.3|{"originWidth":1179,"originHeight":1111,"style":"alignCenter"}_##]

(Api 미팅 시 정책)  
Mock Server는 누가 개발해야 되냐는 갑론을박 끝에 백앤드개발자 팀에서 Mock Sever 만들게 되었고 필드가  비즈니스 개발하면 바뀔 수 있다는 설명과 함께 프런트에 전달할 Request, Response 부족한 부분은 뇌피셜로 한다고 피드백하고 프런트개발자는 승리에 기쁨으로 OK 하게 됩니다. 회사인지 국회인지 피로가 누적됩니다

백앤드 개발자는 Mock Server 구성하고 프런트앤드에 대응하기에는 일정압박으로 다른 도메인 개발자들과 협업할 시간 없이 필드명에 대한 규칙을 각자 개발하여 Movk Server 대신 완성된 Api를 제공합니다

프런트앤드 개발자는 Request, Response 형태가 초반 미팅때와는 다르게 생각보다 달라진 형태를 보고 아연실색한 얼굴로 백앤드 개발자 멱살을 잡아야 하나 일정을 겁나 쪼으고 있는 윗대가리 욕을 해야 하나 생각하며 오늘도 그럴 수 있다는 마음으로 이력서를 업데이트하며 야근각으로 변경이 필요한 기능을 구현해야 합니다

[##_Image|kage@C9C6y/btspsFeceeF/KtalUhIii5a7sAzl3oNhkK/img.jpg|CDM|1.3|{"originWidth":493,"originHeight":462,"style":"alignCenter"}_##]


이처럼 나이스한 상황보다 애자일이라며 스프린트 단위로 진행하는 기법을 소개하면서 WBS 일정 MM 인원수와 완료 일정이 빼곡히 적힌 엑셀 파일을 들이밀며 언제 되는 거야!! 왜 안돼!! 뭐가 문제야!!라는 형태로 수많은 개발자들이 일하고 있는 안타까운 상황이 다반사입니다


> Pact Broker 용도와 설명

## Pact를 사용하기 위한 **선행조건으로 Pact Broker 서버구성**이 필요합니다


공식문서에서 언급되는 서버종류는 두 가지입니다  
자세한 사항은 아래링크를 클릭해 주세요  
[https://docs.pact.io/pact\_broker/docker\_images](https://docs.pact.io/pact_broker/docker_images)  


> Pact 사용 시 언급된 이슈해결되나요?

일반적 Server 개발방식과 다르게 Pact Broker이라는 계약, Api, 상태 테스트등을 동시에 처리할 수 있는 미들웨어를 제공하기에 Api에 필요한 Mock 데이터 및 Api행위에 대한 기능 테스트를 Consumer(front, backend) Provider(backend) 모두 진행해야 합니다

이 얼마나 민주주의적인 행위인가요!!!!  
오늘부터 둘 다 야근각이다!!!!

[##_Image|kage@mvXYt/btspFMYAY6S/UD0r1CZNArsJOnuLHZLGJk/img.jpg|CDM|1.3|{"originWidth":400,"originHeight":395,"style":"alignCenter"}_##]

(이거 도입하자고 한 쉐끼 누구냐!!!!!!!)

Api Contract(request, response)에 필요한 필드 및 행위 모두 진행하기에 개발자들 간에 소통을 최소화하며 테스트 진행이 가능합니다

물론 초반에 발생하는 학습러닝커브와 개발리소스는 추가로 들어가지만 개발이 지속되고 변경되는 Request, Response 영향도를 최소화할 수 있습니다

Contract 즉 계약에 생성되는 Request, Response, Api 행위에 대한 인터페이스는 Consumer에서 Pact Broker에 우선등록하는 구조입니다

Provider에서 MockServer 정보와 비즈니스 완성 후 Api가 달라질 수 있는 부분을 Pact에서 제공하는 기능으로 변경점을 Pact Broker에서 제공하는 Admin Ui를 통해서 인지하게 됩니다

앞으로 아연실색한 얼굴로 변하거나 Mock Server 구축에 대해서 갑론을박할 감정적 부분이 사라지게 됩니다


> Pact Stub Server

Pact Stub Server는 Consumer의 입장에서 Provider의 행동을 스텁(stub)으로 모사합니다.  
다시 말해, 서비스 Provider가 특정 요청에 대해 어떻게 응답해야 하는지를 정의한 'pact' 파일을 읽어, 해당 응답을 재현합니다.  
이를 통해 Provider 서비스 없이도 Consumer 코드를 테스트할 수 있습니다. 이는 Provider의 실제 코드 구현이 완료되지 않은 초기 개발 단계에서 유용합니다.


> Pact Broker Server

Pact Broker는 Pact Contract Testing 프로세스에서 중요한 역할을 하는 중앙 저장소입니다.  
Pact 파일(Consumer와 Provider 간의 계약을 정의한 것)을 저장하고, 버전 관리하는 역할을 합니다.  
Pact Broker를 통해 여러 서비스 간에 계약 정보를 공유하고, 각 서비스의 Pact 검증 상태를 추적할 수 있습니다.


> In a hurry   
> 빠르게 시작하고 싶다면

Server 구성에 비용과 리소스 최적화가 어려울 것 같거나 운영리소스가 부족한 상황이라면  
Pactflow 유료플랜을 이용해서 빠르게 시작해 볼 수 있습니다.  
  
OSS Pact Broker 코드의 포크버전으로 개선된 Ui, 사용자 및 팀 관리, 필드레벨 검증 결과 및 통합 로그인과 같은 추가기능이 포함되어 있습니다.

[Pactflow Contract Testing Platform

Fast, easy and reliable testing for your APIs and microservices at scale

pactflow.io](https://pactflow.io/)

> Pact Work Flow

[##_Image|kage@bMhqOT/btspNQ5ZXMm/Kl7h3WiDL5GKgLXPYp5zRk/img.jpg|CDM|1.3|{"originWidth":1179,"originHeight":894,"style":"alignCenter"}_##]

### **Pact 오픈 라이브러리의 라이프 사이클입니다**


1\. Consumer(Front 또는 Backend)에서 Pact Broker에 등록할 Unit Test에 필요한 코드레벨을 개발언어에 맞춰서 작성

2\. Unit Test를 작성하고 성공되면 DSL(Domain Specific Language). JSON 파일이 생성됩니다.  
. JSON은 API정보의 Uri, Method, Consumer, Provider, Contract(Request, Response) 내용으로 구성된 Pact Broker Server에시 인지할 수 있는 형태라고 보시면 됩니다.

3\. Contract 정보가 담긴 Pact Broker Server에 등록되어 있는 내용에 대해서 Broker에서 제공하는 Web UI로 API 목록과 행위를 확인할 수 있습니다.

4\. Consumer에서 등록한 Api 정보와 행위를 기반으로 테스트코드를 구현합니다 식별가능한 String으로는 Consumer에서 선언된 Value 그리고 Api 행위에 대한 Value는 Provider에서도 동일하게 사용해야 합니다

5\. Provider에서 Unit Test를 실행하면 Pact Broker에 등록된 Contract 정보 및 Api가 정상인지 Stats 성공실패가 Pact Broker Sever Admin에서 확인 가능합니다

여기서 가장 귀찮은 부분은 GetMethod에 Return 되는 Body구조가 복잡하거나 많을 때입니다 이건 chatGpt의 도움으로 최소화하실 수 있습니다

20달러+2달러 총 22달러 2023 8/4 달러환율  
총 28578원 한 달 기준

[##_Image|kage@3HPt6/btspTLp94QS/vX0WgAVma18365kkgjqoO1/img.jpg|CDM|1.3|{"originWidth":1179,"originHeight":1014,"style":"alignCenter"}_##]


 

[##_Image|kage@wmeME/btspFLj61AJ/AxFujnuEjnE8WBOcuFUBi0/img.jpg|CDM|1.3|{"originWidth":1179,"originHeight":1099,"style":"alignCenter"}_##]

해당 그림이 1,2,3번에 해당하는 내용입니다

[##_Image|kage@bE0YXN/btspE45j6Pp/atGKiPsNLyDoanfa970CAK/img.jpg|CDM|1.3|{"originWidth":1179,"originHeight":1091,"style":"alignCenter"}_##]

해당 그림 4번 5번에 해당하는 내용입니다

[##_Image|kage@BmcR8/btspGKrSmj0/dTMIx7epJk2M2uvNnHEmH1/img.jpg|CDM|1.3|{"originWidth":1179,"originHeight":1091,"style":"alignCenter"}_##]

1번부터 5번에 내용입니다


> Pact Contract Test

Pact 라이브러리는 계약(Request, Response)이라는 Api 인터페이스에 대해서 테스트 빌드배포 시 테스트진행하는 단계를 어느 정도 자동화 해줄 수 있는 오픈소스입니다  
서버 구축에 대한 리소스가 부족하거나 오픈라이브러리에서 제공되는 그 이상에 기능이 필요하다면 PactFlow라는 유료 플랜(Saas)도 제공합니다


> Pact 얼마나 효용성이 있을까?

모놀리딕한 서버 구성이나 수많은 서버구성이 아니라면 굳이 사용하는 건 비효율이라고 생각합니다

Request, Response Api 검증 자동화를 하기 위해서는 Pact에서 제공하는 라이브러리를 이용해서 consumer, provider에 대한 테스테 코드 그리고 Pact Broker Server에 Contract(계약)이라는 DSL json파일을 퍼블리싱해야 하는 선행조건들이 들어가게 됩니다


> 자동화 서비스를 위한용도 목적

Frontend To Backend 또는 Backend To Backend 개발에서 발생할수 있는 Mock Server구성 각 파트별 Movk Server 책임 분리 Request, Response 계약 변경으로 인한 Api 검증 테스트 등을 필수로 요구 하기때문에 개발에서 필요한 커뮤니케이션 및 비효율적 리소스를 줄일수 있게 됩니다

빌드배포 에서도 Can I Deploy 라는 Pact Borker Server에 등록된 Api가 정상적인 상태인지 체크후 배포되는 Prod에 안정성을 가져오는 devops에 필요한 기능도 제공합니다

오늘은 Pact가 무엇인지 서비스에 사용해도 되는지 효용성에 대한 공유를 드립니다

다음 챕터2에서는 Pact Broker 구성에 필요한 Docker Image구성 방법을 설명하고  
챕터3 에서는 Consumer Java + Springboot  
챕터4 에서는  Provider Java + Springboot  
챕터5 에서는 Can I deploy 빌드배포 자동화 테스트 기능을 다뤄 보겠습니다