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










