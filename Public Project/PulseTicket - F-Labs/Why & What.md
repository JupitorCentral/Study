### 왜 Docker 를 사용하는가 ?

-> 여러 환경에서 동일하게 프로그램을 동작시키기 위함

### Docker compose 를 사용하는 이유?
-> Docker Container 가 여러 개일 경우 각 container 를 실행할 명령어를 하나로 묶을 수 있음.
-> 또한 자동 네트워크 구성, 볼륨 자동 관리
-> 내 프로젝트에서도 springboot 따로, db 따로, redis 따로 할 예정이므로 필요함


### Docker 의 볼륨 자동관리 기능이란 ?

Docker는 기본적으로 ephemeral, 즉 일시적이다. 컨테이너를 삭제하거나 재시작하면 컨테이너 내부에 저장된 모든 데이터가 사라짐

#### 볼륨이란

컨테이너 외부에 데이터를 저장하는 공간. 컨테이너 생명주기와 독립적으로 존재함.
컨테이너 내부의 특정 디렉토리를 호스트 시스템의 볼륨과 마운트하면 컨테이너가 삭제되어도 볼륨에 데이터 존재.


### Supabase ?

Firebase 의 오픈소스 대안, PostgreSQL 에 여러 기능을 추가한 서비스
PostgreSQL 을 핵심으로 하되 인증, 실시간 구독, 스토리지, api 자동생성등을 추가한 백엔드 플랫폼.

Docker 개념으로 들어가면 사실 각 컨테이너는 별개이다.
그냥 docker-compose.yml 에 supabase-postgresql 이런식으로 추가하는게 아님.
그냥 개념적으로만 포함하는 것 뿐임.

#### Supabase 도구 목록

| #   | 도구명            | 용도        | 설명                       |
| --- | -------------- | --------- | ------------------------ |
| 1   | **PostgreSQL** | 데이터베이스    | 좌석, 예약, 사용자 데이터 저장       |
| 2   | **PostgREST**  | API 자동 생성 | SQL 테이블 → REST API 자동 변환 |
| 3   | **GoTrue**     | 인증 서비스    | 사용자 가입, 로그인, 토큰 관리       |
| 4   | **Realtime**   | 실시간 동기화   | WebSocket으로 DB 변화 실시간 전달 |
| 5   | **Storage**    | 파일 저장소    | 이미지, 영상, 문서 저장 (S3 같음)   |
| 6   | **Kong**       | API 게이트웨이 | API 라우팅, 레이트 제한, 버전 관리   |
| 7   | **PgBouncer**  | 연결 풀링     | DB 연결 효율화 (동시 접속 최적화)    |
| 8   | **Prometheus** | 메트릭 수집    | CPU, 메모리, 응답시간 등 모니터링    |
| 9   | **Netdata**    | 시스템 모니터링  | 실시간 시스템 성능 시각화           |
| 10  | **pgvector**   | 벡터 데이터베이스 | 임베딩 데이터 저장 (AI 기능)       |


#### 🎯 당신의 프로젝트별 필수도

|필수도|도구|추가 시기|
|---|---|---|
|⭐⭐⭐ 필수|PostgreSQL|지금|
|⭐⭐ 중요|GoTrue, Realtime, PostgREST|1-2주 후|
|⭐ 선택|PgBouncer, Kong, Prometheus|1개월 후|
|❌ 불필요|Storage, Netdata, pgvector|추가 안 해도 됨|

docker 로 사용하는건 저렇게 따로고,
supabase cloud 를 쓰면 저 10개가 같이 딸려서 생기는 백엔드 플랫폼을 쓰게 되는 것이다
웹에서 다 관리하게 되는 것



### Kubernetes ?

여러 서버에서 수많은 컨테이어를 자동으록 관리하고 확장하는 시스템

Docker compose -> 한대의 서버에서 여러개의 컨테이너 실행
Kubernetes -> 수백~수천대의 서버에서 수많은 컨테이너

#### 중요한 기능 : auto scailing

트래픽 증가 -> kubernetes 가 자동으로 컨테이너를 추가로 실행함.
Docker compose 는 이런 auto scailing 기능이 없음


#### 자동 복구

컨테이너나 서버가 죽으면 자동 재복구


#### 멘토님이 언급한 이유 (아마도)

티켓팅 시스템 -> 트래픽 폭주 -> 일시적 컨테이너 생성으로 트래픽 처리

다만 설정 난이도가 높다.



### rate limitter ?

이건 잘 모르겠다. 내 프로젝트 스펙에 대기열 순번조회라는 기능에 이 메모를 써놓으셨던데.



### Git 의 브랜치에 관하여

- origin/main
여기서 origin 은 원격 저장소 repository 를 의미하며, origin/main 은 원격 저장소의 기본 branch 를 의미한다.

그러니까 해당 프로젝트의 branch1, branch2 가 있는데 branch1 이 해당 프로젝트의 기본 브랜치로 설정되어있으면
origin/branch1 이렇게 뜸.

이 기본 브랜치를 branch2 로 바꿀 수 있고, 이러면 origin/branch2 가 된다.

그리고 origin 은 연결시 원격 레포지토리를 의미하는 용어일뿐
원격 저장소의 별칭은 **각 로컬 저장소마다 독립적**입니다. 같은 GitHub 저장소를 여러 컴퓨터에서 연결했다면:
- **컴퓨터 A**: origin으로 설정
- **컴퓨터 B**: upstream으로 설정
- **컴퓨터 C**: github으로 설정
이렇게 각각 다른 이름으로 사용할 수 있습니다.
(upstream/main)

어쨌든 origin은 로컬에서 지칭하는 용어 이므로 원격레포지토리에서는 origin 같은 개념이 없고
main branch, default branch 라는 개념만 있음.

그냥 main 은 내 로컬 pc 에 있는 main branch 를 의미한다.


#### 로컬에서 main 과 origin/main 이 다른이유

두 브랜치의 커밋 상태가 다를 수 있기 떄문임.
로컬에서 새로운 커밋을 만들면 그게 로컬에 있는 main 이라는 branch 에는 있지만
원격 저장소에 있는 'main' 이라는 branch 에는 없을 수 있음

그리고 다른 팀원이 origin/main 에 그 팀원의 로컬에 있는 main 을 push 한다 하더라도
내 컴퓨터에 있는 main 브랜치의 내용은 아직 해당 내용이 업데이트 되어있지 않다.


### docker container 에 있는 jvm memory 설정

안에 있는 jvm 메모리 설정은 docker 에서 컨테이너 메모리설정을 통해 할 수 있음
java 명령어로 메모리 제한이 있는지는 모름
기본적으로 jvm 이 자동으로 메모리 할당량 가져가는걸로 알고 있음