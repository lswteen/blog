---
title: "MSA 아키텍처 API 자동화 Pact Broker Server 구성 방법 Docker-Compose.yaml 만들기 챕터2"  
date: 2023-08-13
---

# Pact Broker Server (Contract Test) 
Pact Broker Server 는 Contract 계약관계를 등록하는 서버입니다.  
[챕터1 Pact Contract 알아보기](https://angryfullstack.tistory.com/111) 에서 설명한것 처럼 Consumer(소비자) Provider(공급자) Server To Server 테스트를 하기위한 
API 정보 및 Contract(계약) 내용을 등록하고 Broker Server 제공하는 Web UI를통해서 등록된 API에 내용 그리고 
각서버별 API 변경정보 및 연동 상태를 확인할수 있는 기능을 제공합니다.

Pact Broker Server 계약정보는 Consumer(소비자) 중심의 계약 및 검증결과를 공유하는 애플리케이션입니다.
Json으로 직렬화 될수 있는 어떤 유형의 계약에도 사용할수 있기때문에 동일한 언어가 아니라도 Contract(계약) 등록된 API정보를 Provider(공급자) 에서 자유로운 언어로 unit 테스토로 검증할수 있습니다.

예시)  
1.Node 로 구성된 Frontend Consumer Pact 계약정보(JSON)를 Broker Server Publish   
2.Java Springboot 구성된 Provider 에서 Consumer에서 등록한 Pact 계약정보(JSON)를 Unit Test Code를 이용해서 검증할수 있습니다.

서로 다른 언어로 구성된 API도 Broker Server 에서 자유롭게 검증, 테스트 가능합니다.

## Pact Server 
- Pact Stub Server
```text
Pact Stub Server는 Consumer의 입장에서 Provider의 행동을 스텁(stub)으로 모사합니다.
다시 말해, 서비스 Provider가 특정 요청에 대해 어떻게 응답해야 하는지를 정의한 'pact' 파일을 읽어, 해당 응답을 재현합니다.
이를 통해 Provider 서비스 없이도 Consumer 코드를 테스트할 수 있습니다. 이는 Provider의 실제 코드 구현이 완료되지 않은 초기 개발 단계에서 유용합니다.    
``` 
- Pact Broker
```text
Pact Broker는 Pact Contract Testing 프로세스에서 중요한 역할을 하는 중앙 저장소입니다.
Pact 파일(Consumer와 Provider간의 계약을 정의한 것)을 저장하고, 버전 관리하는 역할을 합니다.
Pact Broker를 통해 여러 서비스 간에 계약 정보를 공유하고, 각 서비스의 Pact 검증 상태를 추적할 수 있습니다.
```

## Pact Broker Server 2가지 종류
pactfoundation 이미지
```text
- Alpine에서 Puma를 사용합니다
- 크기가 작습니다
- root로 실행할 필요가 없습니다 (따라서 1024 이상의 포트에서만 실행할 수 있습니다)
- 충돌시 자동으로 재시작하지 않습니다
- Docker 컨테이너 오케스트레이션 서비스(ECS, Fargate, Kubernetes 등)에서 실행하기에 더 적합합니다
```
dius 이미지
```text
- Ubuntu에서 Passenger를 사용합니다
- 크기가 큽니다
- root로 실행해야 합니다 (따라서 1024 이하의 포트에서 실행할 수 있습니다)
- 충돌시 자식 프로세스를 재시작합니다
```


## Pact Broker(pactfoundation 이미지 사용)
dius/pact-broker 이미지에서 마이그레이션 pactfoundation/pact-broker 이미지는 dius/pact-broker 이미지의 포크 버전입니다.
크기가 작으며(Alpine Linux에서 Puma를 실행하기 때문에 더 큰 Passenger Phusion 기반 이미지보다 작습니다), 루트 권한이 필요하지 않습니다.


### 어떤이미지가 현재 상황에 맞는건지 추가적인 정보가 필요하다면 아래 링크를 클릭!클릭!
https://github.com/phusion/passenger/wiki/Puma-vs-Phusion-Passenger
```text
Puma vs Phusion Passenger

Puma는 Evan Phoenix에 의해 작성되고 Mongrel을 기반으로 하는 Ruby를 위한 오픈 소스 다중 스레드 애플리케이션 서버입니다. 
우리 Phusion에서는 Puma가 Phusion Passenger Ruby 웹 서버와 어떻게 다른지에 대한 질문을 자주 받습니다. 
이 기사는 몇몇 차이점을 나열합니다.

객관성과 정확성에 대하여
지금까지 주시면서도 아실 수 있듯이, 이 기사는 Phusion Passenger의 저자들에 의해 작성되었습니다. 
따라서 몇몇 사람들은 이 기사의 객관성과 정확성에 대해 우려를 표현했습니다. 
먼저, 우리에게 상업적 이해관계가 있기 때문에 완전히 편견 없이 접근하는 것이 불가능하다는 것을 밝히고 싶습니다. 
그렇지만, 우리는 최선을 다하고 있으며, 사실에 근거하지 않는 주장을 좋아하지 않습니다. 
만약 우리가 Phusion Passenger가 X보다 나다고 말한다면, 그것은 우리가 그것을 뒷받침할 수 있기 때문이며, 
우리가 그것을 믿기 때문입니다. 

만약 여러분이 객관성이나 정확성에 대해 여전히 우려하신다면, Evan Phoenix 자신에 의한 이 응답을 읽어보시길 권장합니다:

... 추가적 내용은 해당링크에서 클릭해보세요

```

### 간단히 말하면...

Docker 이미지를 애플리케이션을 고가용성으로 만드는 관리 아키텍처(ECS, Kubernetes 등)에서 실행하려면 pactfoundation/pact-broker를 사용하세요.
Puma는 충돌시 자동으로 재시작하지 않으므로, Pact Broker가 계속 사용 가능하도록 외부 모니터링이 필요합니다.

컨테이너를 독립 실행형 인스턴스로 실행하려면, Phusion Passenger를 사용하는 dius/pact-broker 이미지가 더 나을 수 있습니다.
Passenger는 충돌한 프로세스를 재시작합니다.


# Pact Broker Server Local 구성하기 Mac 기준 (약 10 - 20분소요)
순서대로 진행하시면 Local PC에서 docker-compose.yaml을 최종적으로 실행시킨후   
http://localhost:9292 Pact Broker Server WEB UI 까지 확인 가능 합니다. 

## 1. RancherDesktop 설치
https://rancherdesktop.io/
<img width="1227" alt="스크린샷 2023-08-14 오후 5 03 56" src="https://github.com/lswteen/blog/assets/3292892/86d255ea-a8a8-43b4-a553-a4000e19a17e">
OS환경별 다운로드 진행

## 2. Docker Docker-Compose CLI 설치
```yaml
brew install docker
$ brew install docker-compose
$ brew install docker-credential-helper
```

## 3. docker-compose.yml 파일 생성
위치는 편하신 폴더에 생성하시면됩니다.

```yaml
version: '1'
services:
  pact_broker:
    image: pactfoundation/pact-broker:2.110.0-pactbroker2.107.1
    platform: linux/amd64
    ports:
      - "9292:9292"
    environment:
      PACT_BROKER_PORT: '9292'
      PACT_BROKER_LOG_LEVEL: INFO
      PACT_BROKER_SQL_LOG_LEVEL: DEBUG
      PACT_BROKER_DATABASE_ADAPTER: postgres
      PACT_BROKER_DATABASE_USERNAME: postgres
      PACT_BROKER_DATABASE_PASSWORD: ThePassword
      PACT_BROKER_DATABASE_HOST: db
      PACT_BROKER_DATABASE_NAME: pact_broker
    depends_on:
      - db

  db:
    image: postgres
    ports:
      - "5432:5432"
    volumes:
      - data:/var/lib/postgresql/data
    environment:
      POSTGRES_PASSWORD: ThePassword
      POSTGRES_DB: pact_broker
volumes:
  data: {}
```
Database RDBMS로 postgres 내부적으로 사용하게 됩니다.

아래 정보는 DEV환경 구성시 별도의 mount 파일 경로를 등록하여 초기화 되는것을 방지할수 있습니다.
volumes:
- data:/var/lib/postgresql/data



## 4. docker-compose 실행
생성한 docker-compose.yml 확인
<img width="577" alt="스크린샷 2023-08-14 오후 5 10 39" src="https://github.com/lswteen/blog/assets/3292892/296d4392-7700-4548-8d78-c10d0ebd86b2">

1.Rancher Desktop 실행
```yaml
Cannot connect to the Docker daemon at unix:///var/run/docker.sock. Is the docker daemon running?
```
Rancher Desktop 실행하지 않고 2번 "docker-compose up -d" 하려고하면 위에 오류가 발생합니다.

2.docker-compose 실행 "docker-compose up -d"
```yaml
(base) lswteen@lswteenui-iMac pact % docker-compose up -d
[+] Running 2/2
 ✔ Container pact-db-1           Started                                                                    0.4s
 ✔ Container pact-pact_broker-1  Started

```
Pact Broker Server 확인

http://localhost:9292/
- 등록된 API Consumer, Provider 정보 확인
<img width="1171" alt="스크린샷 2023-08-14 오후 5 15 59" src="https://github.com/lswteen/blog/assets/3292892/b89b4e85-87df-4c51-b4d8-c72c137d32e2">
- API 정보 Request, Response, 동작 행위 확인
<img width="1668" alt="스크린샷 2023-08-14 오후 5 16 10" src="https://github.com/lswteen/blog/assets/3292892/4b881a26-da5b-4351-86b7-4b0c189ce6bf">
- The Matrix : Consumer, Provider 연동 히스토리 목록 확인 
<img width="1158" alt="스크린샷 2023-08-14 오후 5 16 19" src="https://github.com/lswteen/blog/assets/3292892/972bc07c-d840-4992-a96d-d7c409a9898e">
- Network Graph 모니터링
<img width="1124" alt="스크린샷 2023-08-14 오후 5 16 31" src="https://github.com/lswteen/blog/assets/3292892/49a9b76b-f32b-4a33-a7d3-6c767037e55d">

이런 모든 기능을 Pact 오픈 라이브러리에서 제공하는 기능으로 사용가능합니다.  
서버 비용이나 백앤드 서버 리소스를 구성하지 않고 사용하실 목적이라면 아래 유료플랜도 고려해보세요!!

## Pact 서버 구성없이 빠르게 시작을 하고싶다면 SaaS(Software as a Service) 유료 구독 플랜 기능
Saas : 
```text
클라우드 기반의 소프트웨어 제공 모델로, 클라우드 제공업체가 클라우드 애플리케이션 소프트웨어를 개발 및 유지 관리하고,
자동 소프트웨어 업데이트를 제공하고, 인터넷을 통해 Pay-as-you-Go 방식으로 고객에게 소프트웨어를 제공
```

모든 팀에서 접근할 수 있는 Pact Broker를 시험해보고, 인프라 요청서를 작성하고 3개월 동안 기다릴 필요 없이  
pactflow.io에서 무료 시험판을 받을 수 있습니다.   
PactFlow는 OSS Pact Broker의 포크로, 개선된 UI, 필드 레벨 검증 결과, 사용자 및 팀 관리와 같은 추가 기능이 포함되어 있습니다.   
또한 완전히 지원되므로 문제가 발생하면 다른 사람이 오후에 깨어나서 문제를 해결합니다.

https://pactflow.io/
<img width="1161" alt="스크린샷 2023-08-14 오후 3 14 46" src="https://github.com/lswteen/blog/assets/3292892/c91415bf-1909-49df-9673-c54b78f7c114">
