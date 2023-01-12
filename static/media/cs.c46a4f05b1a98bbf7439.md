# CS Seminar Study

## ✨서버, 네트워크

### VPC**(virtual private connection)**

**특징:** 

네트워크 분리

### AWS의 **지원도구:**

1. **Route 53**
    
    DNS, Domain 설정하여 IP와 Server에 적용될 각 Domain을 관리
    
2. **Cloud Font**
    
    클라우드에 미리 caching된 소스를 받아오는 역할 CDN 소스(변경 사항이 없으면 cach를 그대로 받고, 아니면 원 서버에서 변경 사항을 업데이트한 뒤 수령)
    
    *** CDN: (content delivery network)** 
    
    지리적으로 분산된 콘텐츠 전송 기술로, 소스코드를 참조할 때 하나의 원본 소스로 트래픽이 몰리거나 먼 지역에서 소스를 가져갈 때 생기는 latency 지연 문제를 보완할 수 있다.
    

1. **AWS의** **클라우드 저장소**
    - **S3(simple storage service)**
        
        클라우드 저장소 → 저장량에 비례한 비용발생
        
    - **AWS Glacier**
        
        자주 사용하지 않는 s3의 데이터를 저장하는 용도
        
        필요할 때 접근하여 받을 수 있다. 
        
        저장되어 있던 DB의 추출 시간이 오래 걸릴 수 있지만, 저렴하다
        
        *** 주 용도:**
        
        오래되었지만 필요한 로그 데이터는 glacier에 저장
        
2. **API Gateway**
    
    API를 처리하는 통로 역할로 Service나 API단위로 별개의 서버 운영을 통해 특정 서버의 과부하를 방지할 수 있다.
    
    **** 사용 예시:***
    
    *1) 장고에 user purchase 등* 
    
    *2)각 역할별로 서버를 별개 운영* 
    
    *3) API gateway에 serverless를 붙여서 user는 DB에서 직접 받아오기*
    
3. **Lamdba**
    
    서버 없이 코드를 실행가능하도록 기능 수행
    
    *** 주 용도:**
    
    웹 소켓 관련 → 채팅 관련
    
    웹 소켓 관련이면 Lambda로 던져서 처리
    

1. **데이터베이스**
    - **DynamoDB:**
        
        noSql의 sql 
        
        원하는 데이터를 **key: value 형태로 저장**하고 바로 반환
        
        *** 특징:**
        
        1) 빠르게 엑세스, 리턴이 가능
        
        2) 설계 단계에서 모두 정의할 필요x
        
        *** 사용 용도 예시:**
        
        웹 소켓 채팅 서비스에서 이용 중
        

1. **AWS의 관리 용도**
    - **IAM(identity access Manage):**
        
        루트 계정은 문제 발생 시에만 사용
        
        개발할 때는 개인 계정으로 사용/ 관리,추적 용이
        
    - **SNS(simple notification service):**
        
        서버에 대한 모든 알림을 수신하고 관리하기 위한 용도
        
    - **SES(simple email service):**
        
        email 발송에 대한 관리(일일 발송량 제어 가능)
        
        메일 발송이 필요한 서비스
        
        * 일반 사설 메일 서비스(Gmail, naver 등)을 이용하게 되면, 일일 전송량에 제한이 존재하므로 규모가 큰 서비스에서는 메일 발송량에 따라 요금을 책정하는 서비스를 이용한다.
        
    - **Cloud Watch:**
        
        aws의 서비스를 모두 볼 수 있는 서비스(준비와 셋팅이 필요)
        
        클라우드의 서버 로그가 정리
        
        Cloud watch로 SNS 서비스에 제공
        
        * AWS의 SNS서비스와 함께 사용된다.
        
    - **Code Commit:**
        
        github와 같은 기능을 수행 + 자동으로 run하고 있는 server에 적용되어 push/pr발생 시 스스로 deploy 작업 수행
        
        * code 작성 후 branch -> merge하면 build하고 deploy한다
        
    - **Chime:**
        
        화상 회의 서비스(일본에서 개발)
        
    - **EFS(elastic file system):**
        
        사용하는 데이터의 양에 따라 데이터를 붙여쓰기가 가능
        
    - **ECR(elastic containner):**
        
        docker 컨테이너를 가지고 서비스화
        

### Virtual Machine VS Docker

1. **Virtual Machine(가상머신):**
    
    *** 특징:** 
    
    **리소스 자체를 나눠서 프로세스 동작(낭비되는 리소스 존재)**
    
    추가 운영 시 모든 리소스를 전부 셋팅해야 한다.
    
    - **VMWare:** 가상머신을 가동
    
2. **Docker(도커):**
    
    *** 특징:**
    
    **각 프로세스는 독립적으로 동작하며 영향을 주고 받지 않는다.** 
    
    리눅스의 컨테이너를 가시화되고 보고 쓸 수 있게 만들어진 기술
    
    할당된 리소스 내에서 프로세스 별로 분리
    
    가상머신 가동x
    
    *** AWS의 지원 도구:**
    
    **1) ECS:**
    
    사용자는 컨테이너(사용하는 리소스)단위로 사용하고, 해당 리소스의 양에 맞게 EC2서버를 할당**(AWS가 처리)**
    
    **2) Kubernetes(EKS)**
    
    외부 서버와 EC2 등 여러 외부 서버 및 리소스를 자유롭게 활용가능
    
    *** 특징: 사용자가 서버를 관리-> 개발자의 손이 많이 간다.**
    
    **3) GameLift**
    
    실행 가능한 응용 프로그래밍을 빌드 후 올리면 알아서 서버를 가동하는 서비스
    

---

## ✨ 프로그래밍 기법

### **Agile기법**

- **장점:**
    
    고객에게 빠르게 보여준다.
    동작, 작용이 우선
    (증분 개발이 목적)
    자발적인 것을 중시
    
- **단점:**
    
    전체적인 일정보다 조율과 개선을 통해 완성품을 만들어가기 때문에, 프로젝트 마감이나, 절대 납기일을 확정하기 어렵다.
    
    개발자가 빡세게 업무를 해야 한다.
    

- **Agile 실천의 구체화**
    
    **매일 매일 dairy scum 실행**
    
    - 문제 상황, 금일 업무의 계획 정도 체크 용도
    - 번다운: 그래프 형태로 업무의 진행척도와 계획을 체크
    - 숫자의 역수로 해야 할 업무를 정리(넉넉한 단위로 아예 1000단위 등 널널하게)
    
    ### Scrum
    
    준비 회의 / 리뷰 / 회고 -> 반복
    
    - 점진적 반복에 특화
    - splint단위로 동작(2~4주 권장) -> 우리는 1주일 단위
    - 에러, 버그 없이 무조건 동작하는 프로그램 단위(반드시 실행이 되어야 한다.)

- **agile에 사용되는 용어와 구분**
    1. **product owner : 고객주**
        
        방법:
        
        텍스트로서 전달 -> 해당 텍스트를 splint 단위로 쪼개기
        실제 동작 자체와 결과를 보여주면서 리뷰 진행
        
        특징:
        
        실패에 따른 손해 최소화가능(시간 측면)
        
    2. **scrum master: scrum에 있어 MC와 같은 업무 조율자 역할(주로 시니어 개발자)**
        
        특징:
        
        명령x, 상위 관리자가 아니다.
        팀을 보호, 장애 요소 해결
        개발자는 개발만 진행, product owner와 협상은 scrum mastar가 진행
        
    3. **product back log**
        
        특징:
        
        product owner가 텍스트로 올린다.
        
    4. **splint back log**
        
        특징:
        
        product back log가 완성된 상태에서 splint back log 진행
        
        process 자체의 개선을 목표로 splint 회고 진행
        

### 캄반 기법

**특징:**

notion처럼 zila 사용 branh가 자동 생성되고 push하면 done으로 기록[모든 로그가 쌓인다]
할 일 / 완료된 일 적기

to do  / in progress / done 의 일반적인 3단 구성

**이때, in progress의 max는 고정 -> 협력을 강조**

(한 명에게 과중한 업무가 치중되는 일을 방지)

가장 상위의 to do list를 in progress로 옮겨서 수행

**자율적으로 업무를 선택하지만, 성과가 한 눈에 보인다.**

### Pair Programming

보통 시니어의 작업을 주니어가 관찰

- 언어, 프로그램을 잘 모를 때 가장 효과적

### TDD 기법

모든 기능과 컴포넌트에 대한 Test Code를 먼저 만들고 하나씩 에러들을 해결하는 방식으로 전체적인 코드 작성 및 개발

시간이 많이 걸리지만, 퀄리티가 좋은 편

- 일반적으로는 납기일을 우선 선택할 수 밖에 없다.

**테스트 코드가 쌓이고, 하나씩 테스트 코드의 오류를 최소한의 단위로 해결하는 방식**
소스코드의 품질은 최상급 생성 가능

### CICD

**test code :** 업데이트 및 작업 시 test의 자동화를 위해 test code 존재
서버에서 자동으로 test code를 돌려서 merge 시 오류 검사가 가능

---

## ✨HTTP와 HTML

[[HTTP 기초_1] 헤더 (요청(Request) 헤더, 응답(Response)헤더)](https://goddaehee.tistory.com/169)

[[Network] HTTP 헤더의 종류 및 항목 - Heee's Development Blog](https://gmlwjd9405.github.io/2019/01/28/http-header-types.html)

[Http 버전별 차이점](https://woong-programing.tistory.com/152)

### HTML과 HTTP의 약자와 의미

http: html을 위한 통신규약

html: 초기에는 link기능인 a태그가 핵심적인 hyper text의 특성

### POST방식

Header의 content에 server로 보낼 정보가 저장

**요청:**

메소드 요청주소 http버전
인증토큰을 헤더에 넣기
POST 파일첨부 시 header에 bite가 많이 들어간다.

**응답:**

content-type

### HTTP 버전의 진화

1. **HTTP1.0 : header의 탄생**
    
    header가 생겼다.(현재도 많이 사용)
    요청,응답 후 connetion 종료
    서버 확인 후 request요청
    이에 따라 overhead가 발생
    리소스 부하가 발생
    
2. **HTTP1.1 : keepalive 기능 추가**
    
    서버 확인을 하지 않고, 한번 연결된 이후에 connection 유지
    메타정보에 connection 타입이 존재
    사용자가 요청할 때만 서버가 반응
    
    대부분 HTTP1.1을 많이 사용한다.
    **단점:** 순차적으로 처리하기 때문에 작업이 오래걸리면 전체적인 작업이 지연된다.
    
3. **HTTP2.0 : pipelining 모델 도입**
    
    동시에 요청 보낸 뒤, 처리되는 대로 응답 전송
    일부 컨텐츠가 먼저 랜더링되는 페이지는 2.0을 사용
    server push기능이 추가(서버가 클라이언트에 먼저 요청을 보낼 수 있다.)
    
4. **HTTP3.0: UDP 기반의 HTTP통신**
    
    기존의 TCP와 달라지므로 널리 사용하지는 않는다.
    

- **HTTP 버전 확인 방법:**
    
    header를 통해 확인가능
    
- **HTTP는 응답/요청을 반복 수행**
    
    만약 응답을 서버로부터 받지 못하면, 브라우저가 응답을 대신 처리
    

### HTML의 진화

[- HTML 버전 종류와 선언](https://roydest.tistory.com/entry/DOCTYPE-HTML-%EB%B2%84%EC%A0%84-%EC%84%A0%EC%96%B8)

- **XML의 쇠퇴 이유:**
    
    "태그" 규칙의 가독성과 편리성에 의해 만들어졌지만, 결국에는 데이터 비효율성으로 인하여 사장되었다.
    
    **XML의 예시:** SMI(자막 파일)
    
    **대체제:** JSON이나 YMAL 파일
    

---

## ✨**FIGMA**

### 특징:

화상으로 소통가능 (헤드셋 아이콘)

올인원 디자인툴

웹 기반이라 실시간으로 공유 및 협업 가능

무료버전은 공유파일, page 도 모두 3개까지만 한 프로젝트에서 생성가능

폰트적용은 웹 버전에서 사용 불가능

배수를 늘려 export해도 해상도가 깨지지 않는다.

### 장점:

협업 용이, 효율적

- **컴포넌트:**
    
    복제, 공통기능 확장이 용이하다.(상황에 맞게 쉽게 수정 가능)
    
- **버전관리:**
    
    enterprise 버전은 branch단위로 쪼개어 실험적인 디자인 테스트 및 병합도 가능하다.
    
- **Figma Mirror**
    
    버튼 및 ui에 적용한 action을 스마트폰에서 바로 확인할 수 있다.
    
    - 단, 스마트폰 별 해상도는 보장할 수 없음

### 조작 및 단축키:

**alt버튼 + 컴포넌트:** 컴포넌트 간의 길이

**Scale툴:** 컴포넌트 내부의 소스도 비율을 유지하며 축소/확대 가능

**Slice툴:** 드래그하여 선택한 영역만 export할 수 있게하는 캡쳐도구

**t버튼:** text 생성 단축키

**Command툴:** 말풍선 남기는 기능(원하는 특정 위치에 클릭 후 comment남길 수 있는 기능)

로그도 확인가능

**Assets툴:** 검색어를 입력하면 해당하는 이름의 컴포넌트 조회가능

**Fill툴:** 내부의 색 뿐만 아니라, 질감/그라데이션 등 채우기의 대부분 기능을 조정가능

**Stroke툴:** 선을 div의 원하는 위치에 생성할 수 있는 기능(눈 모양 아이콘으로 조정가능)

**Selection Color툴:** 선택한 영역의 색을 한번에 수정할 수 있다.

**Effect툴:** 그림자 및 화려한 효과 적용가능

### 주의사항:

**Group:** 각 요소들을 그저 묶기만 한 단위

**Frame:** frame내부의 요소들을 종속할 수 있는 기능

(변경 시 내부 요소도 정렬과 크기가 자동으로 맞춰진다.)

💡**contraint:** 제약을 거는 기능으로서, group의 요소를 선택하면 contraint가 비활성화된다.

내부 요소에서 외부 틀이 내부 요소를 고정하는 축을 직접 지정할 수 있다.

### 스타일

css나 word 파일의 global style과 유사하게 전역적으로 스타일을 지정할 수 있는 기능

**컴포넌트 선택 + 우클릭 → style → sytle name 지정 :** 스타일 생성 가능

**/** 를 사용하면 figma에서 자동으로 스타일 서식이 child component로 분리된다.

→ css로 따지면 class들을 중첩해서 사용할 수 있는 기능과 유사

→ style의 속성을 바꾸면 한번에 수정가능하다.(css와 유사)

특정 컴포넌트의 style 해제: 컴포넌트 선택 후 detach버튼을 누르면 style이 해제된다.

### 라이브러리

동일한 프로젝트 디렉토리 내에 존재하면,(팀프로젝트가 동일) 서로 다른 파일의 스타일을 라이브러리 형태로 삽입하여 전역적으로 사용할 수 있다

### 🔎Auto Layout

단축키: sfhit + a 

css의 flex와 동일하게 화면 크기에 따라 내부 콘텐츠가 자동으로 화면 크기에 맞추어 조정된다.

### 🔎Component

원본 객체를 생성하고 필요에 따라 복제하여 사용하는 기능

컴포넌트 단위로 객체를 만들어두면, 스타일을 적용한 것처럼 component를 수정하여 전역적으로 style관리를 쉽게 할 수 있다.

**하나의 컴포넌트도 여러가지 값으로 지정할 수 있다.**

💡**property값으로 true/ false, on/off로 지정하면 해당 컴포넌트의 인스턴스를 사용할 때 속성 탭의 toggle로서 쉽게 활용할 수 있다.**

### 🔎Plugin

Amima(유료)

개인 프로젝트 시 주로 사용하게 된다.

*** 단축키**

**Shift +1 : 전체 화면으로 보기** 

**Shift + 2: 선택한 컴포넌트(Frame)보기**

---

## ✨HTML & CSS

### HTML의 기본구조

[HTML 기본 구조,웹 문서 구조(HTML 문서 구조), HTML문서 확장자, HTML 문서의 기본구조, HTML의 구성요소](https://codedragon.tistory.com/3286)

**UTF-8:** Unicode Translate Format

**meta data:** 정보에 대한 정보 → 개발자가 의도적으로 선언

*** script 태그에도 async를 부여하여, 효율성을 증대시킬 수 있다.**

*** 최근에는 div / span을 크게 구분하지 않는다.**

→ display속성으로 수정하기 때문

→ 오히려 혼재하면 헷갈릴 수 있다.

**따라서, flex/grid 사용할 때 모두 div 태그를 사용하고 classname으로 구분**

**외부 컨테이너:** container → div로 사용했던 역할

**행:** row  → div로 사용했던 역할

**열:** col  → span으로 사용했던 역할

## CSS기초

[CSS 프로그래밍 기초](https://dinfree.com/lecture/frontend/122_css_1)

**css의 우선순위: (우선 순위가 높을수록 강력)**

1) 브라우저 디자인 정의 

2) 외부 스타일시트 

3) 내부 스타일시트 

4) 인라인 스타일시트

**부가사항:**

class의 우선순위도 css 적용 시 개발 효율성 향상에 도움이 된다.

vh가 중요(scroll로 인해 전체 화면을 조정하기 어렵다)

**가상 엘리먼트(pseudo element)**

선택된 요소의 특정 위치에 동작하는 셀렉터로 예를 들어 특정 요소의 앞,뒤,첫 글자,첫 줄 등에 동작하는 스타일을 지정하기 위해 사용

| --- | --- | --- |

**css에서 start - end 형식으로 왼/오를 구분하는 이유:**

Arab language(오 → 왼)으로 타 언어와 읽는 방식이 다르기 때문

→ 아랍어로 전환하면 자동으로 start/end의 방향이 전환된다.

🔎**Tip:** 

1. **디자인안 받을 때, 줄의 개수 변화/ 사이즈 변화도 미리 파악하면 차후에 불필요한 소통을 미연에 방지할 수 있다.**
2. **Figma는 정적인 요소 위주로 표현하기 때문에, Animation 부분은 명확한 기준과 각 요소 별 변화 지점을 파악해야 한다.**

---

## ✨**CBD component 기반 프로그래밍 방법론**

- **CBD 개발 방법론 개략 소개**
    
    [CBD 개발방법론](http://wiki.hash.kr/index.php/CBD_%EA%B0%9C%EB%B0%9C%EB%B0%A9%EB%B2%95%EB%A1%A0)
    

- **COMPONENT / SERVICE / OBJECT의 차이**
    
    어노테이션 차이
    
    [[카카오 면접] @Service,@Controller,@Component 차이](https://baek-kim-dev.site/64)
    
    **@Component**
    
    Spring에서 관리되는 객체임을 표시하기 위해 사용하는 가장 기본적인 annotation이다. 즉, scan-auto-detection과 dependency injection을 사용하기 위해서 사용되는 가장 기본 어노테이션이다.
    
    **@Controller**
    
    Web MVC 코드에 사용되는 어노테이션이다. @RequestMapping 어노테이션을 해당 어노테이션 밑에서만 사용할 수 있다.
    
    **@Repository**
    
    이것은 Annotation based Configuration, @Repository의 작업은 플랫폼 별 예외를 잡아서 Spring의 통합 검사되지 않은 예외 중 하나로 다시 던지는 것입니다. 이를 위해 PersistenceExceptionTranslationPostProcessor이 제공되며 다음과 같이 Spring의 애플리케이션 컨텍스트에 추가해야합니다.
    
    **@Service**
    
    ✨비즈니스 로직이나 respository layer 호출하는 함수에 사용된다. 다른 어노테이션과 다르게 @Component에 추가된 기능은 없다. 하지만 나중에 Spring 측에서 추가적인 exception handling을 해줄 수도 있으니 비즈니스 로직에는 해당 어노테이션을 사용하자.
    
    ---
    
    ## ✨Web RTC
    
    Web RTC 원리 및 응용 소개
    
    [](https://wormwlrm.github.io/2021/01/24/Introducing-WebRTC.html)
    
    ---
    
    ## ✨객체지향과 컴포넌트
    
    ### 절차 지향 → 객체 지향
    
    **절차지향 프로그래밍의 한계 발생:**
    
    순차적으로 프로그램을 실행하다보니, 불필요한 중복코드 발생
    
    - 재사용성 문제
    - 코드 리뷰 어려움
    - 중복코드 제거
    
    | 객체 | 클래스 | 개체 |
    | --- | --- | --- |
    | object | class | instance |
    | 추상적 개념 | object를 소스코드로 옮겨둔 상태 | class를 memory에 올리면 instance |
    
    ### class vs instance
    
    - **class:** 소스코드 자체 (찍어내는 틀)
    - **instance:** 메모리에 올라가 사용되고 있는 class (결과적으로 산출된 값)
    
    | Service | Component | Class |
    | --- | --- | --- |
    | 큰 범위의 기능 | 부속품(like 레고) | 완전한 하나의 기능을 수행하지 못할 수도 있다. |
    | frontend: 서비스를 서버로부터 받아옴
    backend: 실제로 연산이 이루어져, 핵심적인 기능을 반환 
    * 따라서 주로 server가 service를 제공한다. | 컴포넌트 자체로도 Service가 될 수도 있다.
    그러나 일반적으로 Service의 범위가 더 크다.
    부속품인만큼, 컴포넌트 1개만으로도 충분히 부속품의 기능을 수행해야한다. | 단위가 작을수록 효율성이 증가 |
    | 하나의 큰 service 내에 작은 여러 service가 조합될 수 있다. | 컴포넌트는 작아질수록 재사용성 증가
    컴포넌트가 커질수록 재사용하기 어렵다 |  |
    
    *** 각각의 단위는 작을수록 재사용성이 높아진다.**
    
    ### Service Oriented Architecture
    
    서비스들이 모여서 하나의 서비스가 구축되는 개발의 구조
    
    ### Component Based Development
    
    컴포넌트 기반의 개발 구조
    
    ### Micro Service Architecture
    
    작은 서비스 단위로 개별 서버를 두어, 필요에 따라 유동적으로 서버의 크기를 조정한다.
    
    서버 자원 절약, 효율적 운영이 가능
    
    ex) 넷플릭스가 좋은 예시
    
    사용량, 접속량을 예측/관리해야 하는 부담 존재
    
    ---
    
    ## ✨Redis
    
    **도입 배경:**
    
    자주 DB에 접속해야 하는 데이터들을 처리할 때, 매번 DB에 접근할 경우 서버의 부하가 발생
    
    **장점:**
    
    in-memory 기반의 no-sql이라 빠르다
    
    다양한 자료구조를 지원
    
    편의성 좋고, 난이도는 낮아 도입하기 용이하다
    
    **동작:**
    
    1. 데이터가 캐시에 없는 경우
        
        client → server → redis → server → DB → derver → client → redis
        
    2. 해당 데이터가 캐시에 있는 경우(가장 일반적인 경우 + 가장 빠른 처리 속도)
        
        client → redis → server → client
        
    
    1. 캐시에 저장하는 데이터를 업데이트하는 경우
        
        client → server → redis → server → DB → server → client
        
    

---

## ✨KafKa

apache kafka는 실시간으로 기록 스트림을 게시, 구독, 저장 및 처리할 수 있는 분산 데이터 스트리밍 플랫폼

**도입배경:**

MSA구조에서 각 컴포넌트들이 loose coupling되며, 컴포넌트 간의 메시지 전달 및 약해진 의존성을 보완하기 위한 방법으로 대용량 트래픽 및 메시지 처리를 구현하기 위한 스트리밍 플랫폼

분리된 API서버들의 처리를 비동기식으로 처리 필요

장애 발생에 따른 미처리된 메시지들의 순차적 처리 필요

**구조:**

broker: 메시지를 처리하는 주체

producer: 메시지(이벤트)를 게시

consumer: 메시지(이벤트)를 구독

## Open Search Dashboard

---

## ✨아토믹 디자인

[Atomic Design](https://bradfrost.com/blog/post/atomic-web-design/#atoms)

---

## ✨API vs Middleware

[What is the difference between API and middleware?](https://www.peerspot.com/questions/what-is-the-difference-between-api-and-middleware)

---

## ✨JWT(Json Web Token)

JWT의 정의

[[WEB] 📚 JWT 토큰 인증 이란? - 💯 이해하기 쉽게 정리](https://inpa.tistory.com/entry/WEB-%F0%9F%93%9A-JWTjson-web-token-%EB%9E%80-%F0%9F%92%AF-%EC%A0%95%EB%A6%AC)

---

## ✨Django Transaction

### transaction 특성:

1. 원자성: 반드시 어떠한 형태로든 끝이 나야 한다.(commit 또는 rollback 중 하나가 반드시 수행)
2. 일관성: 결과와 무관하게 반드시 DB에 저장
3. 독립성: 외부의 영향과 무관하게 동작
4. 지속성: log 혹은 그외 방법을 통해 지속적으로 관리할 수 있어야 한다.

### Atomic transaction

atomic transaction은 DB를 단일 프로세스가 점유하여 해당 코드 내부에 선언된 작업을 수행

이때 외부의 간섭이 발생하지 않는 것이 atomic transaction의 기본 개념인데, try-except 등의 다른 코드가 개입하게 되면 외부의 간섭이 발생하기 때문에 atomic구조 본질 자체가 훼손된다.

따라서 atomic transaction 내부에서는 예외처리 등의 부수적인 작업은 지양해야 한다.

예외 발생 시, 해당 atomic transaction 외부에 try-except를 포함한 예외처리 코드를 작성하여 트러블 슈팅을 해야 버그 없이 올바른 코드가 실행될 수 있다.

---

## ✨Docker

**container :** 프로세스, container image가 실행(run)되고 있는 상태를 container라고 지칭

**container image :** file (exe 실행 파일의 형태)

docker build  → image 가 생성(보통 docker image라고 부르는 것이 container image) 

* Image라고 하지만, 실질적으로는 build된 파일의 결과물 file을 지칭한다.

---

## ✨Flutter

### Flutter 개요

cross platform을 위해(android, ios를 동시 개발)

성능 이슈로 인해 native로 개발. 그 외에 속도 자체는 cross platform 개발이 가능한 Flutter가 유리

개발 효율이 native보다 20%개발 소요가 필요

but, 양쪽 플랫폼 모두 개발한다는 가정 하에 200% → 120%으로 가능

구글은 Flutter로 Kotlin을 대체하려는 목적

### Cross Platform의 필요성

네이티브 개발 시, 기획/디자인도 2가지가 필요하며 개발 인력 및 소요시간도 감축 가능

### Dart로 언어 도입 이유

- 두가지 컴파일 방법을 지원(실시간 + 컴파일)
- 핫 리로드가능(수정하는 순간 바로 수정사항이 적용) → 실시간 개발, 제품 출시 시 컴파일 가능
- 초당 60프레임의 애니메이션 속도
- 선제적 스케툴링
- lock없이 개비지스토어 사용가능

### Dart의 주요 특징

- null safety
    
    null point error 해결을 위해
    
    자료형 뒤에 ?가 없으면 null자체가 선언되지 않는다.
    
- js의 var 변수와 동일한 데이터 형이 존재
    
    그렇지만 js와 달리 var의 데이터 형을 식별하고 연산 시 컴파일러가 이를 감지한다.
    
- null safety의 데이터 캐스팅 방법
    
    nullable과 un-nullable 간의 연산, 대입 시 캐스팅이 필요하다.
    
- late 키워드 존재
    
    null 값이더라도 추후에 값이 할당될 수 있음을 컴파일러에게 선언
    

협업 시, null safety로 인해 개발 시 에러나 개발의 어려움이 존재할 수 있다.

- 자료형 특징
    
    num 자료형이 존재(int, double의 개념을 포괄)
    
    var, dynamic 자료형이 존재(선언의 형태를 보고 컴파일러가 자료형을 추정 및 산정)
    
    - var: 선언 시기에 자료형을 지정
    - dynamic: js의 let처럼 자유롭게 자료형 및 내용 수정 가능
    
    final, const,  dynamic이 존재
    
    - final: runtime 중에만 수정 가능, 런타임에 변수가 선언되는 순간 그 이후에 수정 불가능
    - const: compile 단계에서부터 실제 실행까지 모두 수정 불가능

컴파일러가 최적화를 위해 동적으로 바뀌지 않는 변수, 값은 메모리에 값 그 자체를 박아 넣어버린다.

Flutter는 객체지향으로 각 위젯들이 객체역할을 수행한다.

메모리에 실제 올라간 위젯, 객체가 매칭되어 올라간다.

- 언어의 특징
    
    _function()으로 private function을 지정한다.
    
    json형태의 데이터를 파싱하면 map객체를 그대로 받아올 수 있다.(js와 유사한 형태)
    
    const로 선언된 함수나 객체 내의 값이 동적으로 수정되면(내부 parameter나 value가 수정되면) error를 낸다.
    
    interface 존재
    
    함수 내에 optional, named parameter가 존재
    

### Flutter의 특징

- 위젯의 수정사항 존재 시 다시 build 여부 지정
    
    statelessWidget
    
    statefulWidget
    
    이를 위해 getX와 같은 상태관리 라이브러리를 사용
    
    → 변경된 값만을 추적하여 해당 위젯만을 re-build 처리하여 성능을 개선
    
    1) GetBuilder로서 build처리: 내부에서 지정된 데이터가 변경되는 것만을 추적하여 화면을 리랜더링
    
    2) Obx()로 선언된 데이터만을 변경내역 추적
    
- 웹 개발 처럼 router로서 각 페이지와 위젯이동이 처리된다.
    
    initialRoute로 uri를 지정
    

- theme가 별도로 존재
    
    android는 각 버전별로 theme가 다름
    
- child와 children의 형태로 각 위젯을 상속받고 내부에 그리는 형태 - 태그와 유사
    
    row, column은 children 내부에 사용
    
    child 내부에는 단 하나의 widget만 선언 가능
    

- column, row는 웹의 flex와 유사하다.
    
    column → flex-direction: row
    
    row → flex-direction: column
    
    각 함수 내부에 선언된 child는 웹의 flex내부의 태그들과 동일한 움직임을 가진다.
    

- 크기 조절 시, 디바이스마다 화면의 크기가 다르기 때문에 웹처럼 특정 pixcel 단위로 크기를 조정하지는 않는다.
    
    

### 라이브러리

pub로 Flutter 파일을 managing한다.

pub.dev에서 검색하여 찾을 수 있다.

pubspec.yaml에서 설치된 의존성 관계 및 라이브러리, 환경을 확인할 수 있다.

### 상태관리

statefulWidget(동적), statelessWidget (정적) 상태 관리를 지정하여 위젯의 유형을 지정할 수 있다.

(긱스로프트에서는 statefulWidget(동적)으로 주로 개발한다.)

정적상태로 widget 지정 시, 데이터가 변경되면 전체 화면이 새로 랜더링된다.

getX 상태관리 라이브러리를 가장 대중적으로 이용한다.(22.08 기준)

statelessWidget인 경우에만 getX를 사용한다.

getX의 상태관리 방식

- 단순상태관리
    
    update()라는 함수가 실행될 때만 데이터의 변화를 인지
    
- 반응형 상태관리
    
    RxInt, RxString 등… Rx가 포함된 데이터 타입 선언
    
    사용 시 .value로 접근해야 화면에 랜더링이 가능하다.
    

getX를 사용할 경우 getXXXXX 형태로 get으로 시작하는 메소드를 사용한다.

### 디자인 패턴 특성

위젯에 따라 개별적인 controller를 매칭하여 사용할 수 있다.

mvvm의 형태로 사용할 수 있다.(각 view에 맞는 controller를 개별적으로 지정하여 타 컴포넌트나 페이지에 재사용할 때 별도의 데이터 바인딩 없이 해당 view(컴포넌트) 단위로 쉽게 사용할 수 있다.)

---

# ✨Architecture Pattern

* 이름 자체는 controller, presenter, view model등 다양할 수 있다. 이때 단순히 이름만으로 디자인 패턴을 판단해서는 안되고 유사한 역할을 할 수 있다는 것정도만 인지하고 디자인 패턴을 파악해야 한다.

* 복잡한 서비스가 될수록 백엔드에서 MC를 담당하지만, V안에서도 MVC가 존재하는 경우가 많다. 따라서 프론트엔드 개발 시에도 최소한의 MVC패턴을 적용하면 재사용성 및 가독성을 높일 수 있다.

### MVC패턴

view(화면, 사용자의 반응도 포함)

controller(view의 변경 내역을 인지하여 데이터 제공을 위한 조절 기능)

model(데이터나 클래스 등 다방면적인 데이터 처리를 위한 공간, 데이터를 다시 view로 전달)

* view와 model이 긴밀하게 연계

(django → MTV패턴 template이 view 역할, view가 controller 역할 수행)

### MVP패턴

presenter가 controller의 역할을 대신하며, model에서 DB가 변경되어도 model에서 view로 바로 데이터를 보내는 것이 아니라 presenter가 다시 중계하여 view에 전달

* view와 model은 서로 직접 연결되지 않고 반드시 presenter를 통해 사용한다.

* view와 presenter가 1대1관계로 연계되어 있다.

### MVVM패턴

view model이 presenter와 controller의 역할을 대신하며, 

view가 자신에게 해당하는 view model을 찾는다.

- frontend에서 보여주는 화면은 동일하지만 내부의 데이터와 로직이 다른 경우에 각 각에 맞는 viewmodel을 찾아서 model이 적합한 로직을 실행한다.

model이 view의 업데이트를 처리

### MVI패턴

model, veiw, intent 각각의 연계가 단방향&순환 구조로 데이터를 처리, 상호 간의 의존성 없음

intent 사용자를 관찰하고 view에서 특정 값이 변경되거나 변경사항이 존재하면 intent가 직접 이를 인지한다.

**참고소스**

[MVC, MVP, MVVM 을 예제와 함께 알아보자 - android](http://gitsu.tistory.com/38)