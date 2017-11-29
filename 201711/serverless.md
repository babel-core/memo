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

## Azure Function Serverless HOL

### 시나리오 #1. ServerlessFunc-5234 들어가기 

```Csharp
using System.Net;

public static async Task<HttpResponseMessage> Run(HttpRequestMessage req, TraceWriter log)
{
    log.Info("C# HTTP trigger function processed a request.");

    // parse query parameter
    string name = req.GetQueryNameValuePairs()
        .FirstOrDefault(q => string.Compare(q.Key, "name", true) == 0)
        .Value;

    // Get request body
    dynamic data = await req.Content.ReadAsAsync<object>();

    // Set name to query string or body data
    name = name ?? data?.name;

    return name == null
        ? req.CreateResponse(HttpStatusCode.BadRequest, "Please pass a name on the query string or in the request body")
        : req.CreateResponse(HttpStatusCode.OK, "Hello " + name);
}
```
```Log
2017-11-29T05:51:14.528 Function started (Id=93914579-5617-42ba-8be4-8b22db4a9b5c)
2017-11-29T05:51:14.625 C# HTTP trigger function processed a request.
2017-11-29T05:51:14.637 Function completed (Success, Id=93914579-5617-42ba-8be4-8b22db4a9b5c, Duration=108ms)
```

```Output
Hello Azure
```

#### 인자로 ExecutionContext ctx 추가
물리적인 정보를 추가적으로 알려줌 
```Csharp
public static async Task<HttpResponseMessage> Run(HttpRequestMessage req, TraceWriter log, ExecutionContext ctx)
{
    log.Info("C# HTTP trigger function processed a request.");
    log.Info("현재 폴더 : " + ctx.FunctionDirectory);
    // parse query parameter
    string name = req.GetQueryNameValuePairs()
        .FirstOrDefault(q => string.Compare(q.Key, "name", true) == 0)
        .Value;

    // Get request body
    dynamic data = await req.Content.ReadAsAsync<object>();

    // Set name to query string or body data
    name = name ?? data?.name;

    return name == null
        ? req.CreateResponse(HttpStatusCode.BadRequest, "Please pass a name on the query string or in the request body")
        : req.CreateResponse(HttpStatusCode.OK, "Hello " + name);
}
```

```Log
2017-11-29T05:50:08.262 Function started (Id=28bc2d77-ad0d-4513-b4a0-0816e83722f4)
2017-11-29T05:50:08.262 C# HTTP trigger function processed a request.
2017-11-29T05:50:08.262 현재 폴더 : D:\home\site\wwwroot\HttpCS
2017-11-29T05:50:08.262 Function completed (Success, Id=28bc2d77-ad0d-4513-b4a0-0816e83722f4, Duration=0ms)
```

```Output
Hello Azure
```


### 시나리오 #2. Azure Function CLI를 이용하여 개발하기

[https://www.npmjs.com/package/azure-functions-cli](https://www.npmjs.com/package/azure-functions-cli)

1. func init
 - funtion 초기화 
 - init

2. func new

```Bash
$ func new
Select a language: JavaScript
Select a template: HttpTrigger
Function name: [HttpTriggerJS]
Writing d:\azfunctest\HttpTriggerJS\index.js
Writing d:\azfunctest\HttpTriggerJS\sample.dat
Writing d:\azfunctest\HttpTriggerJS\function.json
```

3. visual stdio code 실행
```Bash
$ code .
```

4. code에서 코드 수정

index.js파일의 Line 7번째 라인 수정  

```javascript
body: "How are you doing " + (req.query.name || req.body.name)
```

5. host 시작

```Bash
$ func host start
```

6. vscode를 이용한 debug
```Bash
$ func host start --debug vscode
```

7. azure login 

```Bash
$ func azure login
----
```

8. azure account

```Bash
$ func azure account list
Subscription                                                           Current
------------                                                           -------
Visual Studio Enterprise ? MPN (28109cf6-d71e-4bf2-a455-8e5ba423d10c)  True
```

9. auzre account set    

```Bash
$ func azure account set 28109cf6-d71e-4bf2-a455-8e5ba423d10c
Subscription                                                           Current
------------                                                           -------
Visual Studio Enterprise ? MPN (28109cf6-d71e-4bf2-a455-8e5ba423d10c)  True
```

10. install azure-functions-pack

```Bash
$ npm i -g azure-functions-pack
[  ................] \ fetchMetadata: sill resolveWithNewModule browserify-rsa@4.0.1 checking installable status
```

11. publish on azure function (ServerlessFunc-5234)

```Bash
$ func azure functionapp publish ServerlessFunc-5234
Publish d:\azfunctest contents to an Azure Function App. Locally deleted files are not removed from destination.
Getting site publishing info...
Creating archive for current directory...
Uploading archive...
Upload completed successfully.
```




### 시나리오 #3. Azure 이미지 프로세싱 (영웅 카드 만들기)


#### 실습 사전 작업

1. Storage Container 추가
 - Blob : card-input, card-output, audio
 - Queue : qtem

2. 실습 사전준비 사항
 - Copy Storage Account 연결 문자열 
 - Copy EmotionAPI URL and Key 
 - Copy TextAnalysis URL and Key

#### 실습 시나리오





### 시나리오 #4. 

