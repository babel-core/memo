# Azure Serverless Hands on Lab Workshop

> 2017.11.29 13:00~

## To do list
Functions
Logic App


## 서버리스 3가지 
1. 서버 추상화
2. 이벤트 기반 / 인스턴스 Scale
3. 초 단위의 과금

- Thick 클라이언트; Thin & stateless Backend
    - Angular와 같은 SPA 기술과 함께 사용하면 좋음
    - CORS (Cross Origin Resource Sharing)활성화 필수
      : 우리의 서비스 대상들은 통신을 허용해주고, 나머지의 요청은 거부한다.
- Sync보다는 *Async* 방식이 더욱 효과적임
    - queue 기반 설계일 수록 더 탄력적(resilient)

    - 참고 : httt://www.reactivemanifesto.org


## 서버리스 응용프로그램 플랫폼 구성요소
 - Development
    - IDE
    - Visual debug history
    - Local 개발
    - Verbose debugging
 - Platform
    - Funtions
        - 개발자 도구지원
        - 바인딩과 트리거
        - 오픈소스
    - Logic Apps
        - 비주얼 디자이너
        - 170+ 커넥터
        - Functions 오케스트레이션
    - Data/ Storage
    - Messaging
    - Gateway Conection
    - Inteligence


## 응용 프로그램 예시
예 : 타이머 기반 프로세싱

- Step 1: 매 15분마다 타이머 트리거 발생
- Step 2: 유효하지 않은 데이터를 찾아서 제거
- Step 3: 정리된 테이터 처리

예 : Azure 서비스 이벤트 프로세싱

- Step 1: 로딩된 페이지에 WebHook을 호출
- Step 2: 사용자가 프로필
- Step 3: 

예 : Real-Time

- Step 1: 
- Step 2: 
- Step 3: 

예 : 모바일

- Step 1: 
- Step 2: 
- Step 3: 


## Azure Function에서 지양해야 하는 작업
- Function 내부에서 읽어들이는 과정
- 이 부분은 Input Binding으로 처리해야 함.

## Azure Function Lifecycle
디자인은 모든 것을 코드로 작성됨.
- check in/out push pull이 가능한다는




Functions 동작 방식

- Fucntions App은 여러 함수를 포함할 수 있다.
- Functions App은 0 to many 인스턴스에서 구동된다.
부하가 많이 걸리면 호스트 인스턴스를 늘려 사용하고, Dispose한다.


Functions 프로그래밍 모델,
- Functions은 반드시 "Do on thing"을 해야 한다.
- Fucntions은 반드시 멱등적(idempotent)이어야 한다.
- Fucntions은 반드시 가능한한 빨리 끝나야 한다.

Azure Function Trigger
(종류 다양 google에 Azure Trigger)

run.csx

## Platform & Scale

Dedicated & dynamic
App Service Plan

- 실행 수에 따라 과금
    - 실행 수 : 실행시간 * 사용 메모리

Scale은 플랫폼(Azure)에서 과금



## Azure 리소스 사전 준비
0. Settings 
 - Language : English

1. Resource Group
 - Name : **Az-Hol-Serverless**
 - Location : Japan west

2. App Service Plan
 - Name : ServerlessHol-Dev 
 - Location : Japan west 
 - OS : Windows
 - Pricing Tier : B1 basic

3. Storage account
 - Name : servelessstor5234
 - Replication : LRS
 - Location : japan west

4. Emotion API (Preview)
 - Name : EmotionApi-dev
 - Location : West US
 - Pricing Tier : F0

5. Text Analytics API
 - Name : TextAnalyApi-Dev
 - Location : West US
 - Pricing Tier : F0

6. Function App
 - Name : ServerlessFunc-5234
 - Hosting Plan : App Service Plan
 - App Service Plan / Location : Serverless-dev
 - Storage : serverlessstor5234

7. Storage Container 추가
 - Blob : card-input, card-output, audio
 - Queue : qtem

8. 실습 사전준비 사항
 - Copy Storage Account 연결 문자열 
 - Copy EmotionAPI URL and Key 
 - Copy TextAnalysis URL and Key
 