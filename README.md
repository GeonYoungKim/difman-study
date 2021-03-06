# 배포, CI, 로깅 스터디

```sql
select * from a;

```

1. 배포
    * 배포를 하기위해 거치는 단계
        * 개발(구현) - 테스트 - 빌드 - 배포 - 운영
        * 테스트 = unit test(각각의 모듈 단위), Integration test(모듈 전체)
        * 빌드 = 소스코드 파일을 실행가능하도록 만드는 것을 의미(컴파일의 경우 빌드 과정에 속함.)
        * 대표적 빌드 툴(자바 기준)    
            1. maven
                - 장점 = 
                    1. 편리한 Dependency를 관리
                    2. xml을 이용한 표준화된 형식 제공
                    3. 네트워크를 통한 필요한 라이브러리 사용 가능
                - 단점 = 
                    1. 태그 기반의 xml이기 때문에 복잡해질 가능성이 높음.
                    2. 플로우나 조건부 상황을 표현하기 어려움.
                       
            2. gradle
                - 장점 = 
                    1. 가독성이 좋다.
                    2. 빌드, 테스트의 경우 maven보다 빠르다.
                - 단점 = 
                    1. 기존 레거시 프로젝트에는 존재하지 않아 익숙하지 않음.
                    2. groovy 문법을 배우기 위한 시간 소요
                    
            3. ant
                - 장점 = 
                    1. 절차지향으로 인한 진입장벽의 낮음.
                    2. 유연한 구조를 가짐.
                - 단점 = 
                    1. xml을 사용하지만 절차지향과는 형태가 맞지 않음.
                    2. 큰 프로젝트의 경우 빌드 과정의 어려운 이해
                    
        * 배포 =  준비된 릴리즈를 프로덕션에 올려서 사용자가 쓸 수 있게 하는것.
            * 릴리즈 = 위에서 언급한 테스트, 빌드 등 각 스테이징을 통과한 상태.
            
2. CI
    * 사전적 의미 = 지속적인 통합
    * CI가 대두된 이유 = 한개의 프로젝트를 [협업 / 분업]시 생기는 시간 소요의 절약과 최신 한개의 소스 코드를 검증 및 반영하기 위해!!
    * CI 서버란?
        - 형상 관리 서버에 commit된 소스코드를 주기적으로 폴링하여 컴파일, 테스트, 빌드의 단계에서 더 나아가 배포까지 담당.
    * 대표적인 CI를 위한 기술 list
        * CI server = jenkins
        * 형상 관리 = git
        * build tool = ant, maven, gradle
        * test = junit
        
3. 로깅
    * 사전적 의미 = 동작중인 시스템의 상태와 작동의 기록을 남기는 것.
    * spring boot logging 기술 = logback
        * 로깅에 대해서 공부를 한 적이 없어 "spring boot logging"이라는 검색 시 많이 나오는 logback만을 정리하였습니다..ㅠ
        * 참조 = [logback guide](https://meetup.toast.com/posts/149)
        * @Slf4j 어노테이션 활용 로깅 사용가능
        * 별다른 xml파일이 아닌 application.yml 파일을 사용한 간단한 로깅 설정 가능.
        * Profile별 환경 설정 가능(dev, alpha, real 등)
            * 개인적인 생각 = 큰 프로젝트 시 로깅 설정이 많아 각 profile 별 로깅 정의 파일을 만들어 맞는 파일을 참조하도록 구성하는것 같습니다.
            * 코드의 중복은 생기지만 가독적인 측면에서 효율적이라고 생각됩니다.

 * jenkins 사용 경험 참조 = [창천향로](https://jojoldu.tistory.com/290)
     * systemctl을 이용한 jenkins 서비스 running(ec2 최상단의 경우 sudo service jenkins start 명령어 사용 - systemctl 세팅을 하지 않은 버전이기때문..)
     * 배치성 서버를 개발하고 있어 github에 hook을 걸어 지속적 배포 역할의 job과 스케쥴링이 있는 job들을 사용하고 있습니다.
         * 형상관리를 위해서 쉘스크립트 파일을 사용 추천(해당 부분은 후에 결국 Dockerfile 사용 예상)
         * xml이 아닌 java로 spring batch job 실행 시 com.skuniv.bigdata.batch.실행클래스 실행 메소드 사용
    * 위에서 언급한 배포와 로깅은 jenkins만은로도 구현 가능 
        * gradle의 경우 build 시 test 코드 실행 이점
        * 각 젠킨스 job의 build에서 console output을 통한 로깅 가능
    * 개인적인 생각
        * 젠킨스의 경우 hook과 trigger의 역할로 사용하고 배포는 결국 container기반으로 가야될 듯 합니다.
        * 개인적인 요즘 프로세스로는 아래처럼 하는것 같습니다.!!! (해당 부분은 공부 및 경험이 없어... 지극히 개인적인 생각입니다...)
            * jenkins = 지속적인 형상관리 코드의 trigger로 git pull과 dockerhub에 업로드? 
            * 쿠베로 도커이미지 내려받아 컨테이너를 pod에 띄워 사용.

### 감사합니다.!!!(결혼식으로 참석이 힘들지만 카톡으로라도 얘기나누면 감사드리겠습니다ㅠㅠ)

