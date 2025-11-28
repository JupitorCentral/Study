


\[milestone]
→ 기본적인 티켓팅 시스템 만들기
→ Docker 설정하기
→ 성능 부하기 + 성능 모니터링 설정 + Redis 캐싱으로 성능 높이기 
→ 캐싱 스탬피드 일으켜서 LRU, LFU, FIFO, XFetch 알고리즘 성능 입증
→ 다른 캐싱스탬피드 해결책 시도해보기 (Locking 등)

- [x] docker 를 쓰는건 맞는데 멘토님이 docker-compose 를 사실상 쓰는거라고 하시던데...? 차이가 뭐지? ✅ 2025-11-01

- [x] Docker Desktop 설치 (Mac) ➕ 2025-11-01  ✅ 2025-11-01
- [x] Dockerfile 작성 (Spring Boot 애플리케이션 컨테이너화) ➕ 2025-11-01 ✅ 2025-11-04
- [x] docker-compose.yml 파일 생성 (PostgreSQL, Redis 서비스 정의) ➕ 2025-11-01 ✅ 2025-11-04
- [x] docker-compose.yml에서 PostgreSQL 서비스 설정 ➕ 2025-11-01 ✅ 2025-11-04
- [x] docker-compose.yml에서 Redis 서비스 설정 ➕ 2025-11-01 ✅ 2025-11-04
- [x] docker-compose.yml에서 Spring Boot 서비스 설정 ➕ 2025-11-01 ✅ 2025-11-04
- [x] .gitignore 파일 업데이트 ➕ 2025-11-01 ✅ 2025-11-04
- [x] docker-compose up 실행하여 모든 컨테이너 시작 ➕ 2025-11-01 ✅ 2025-11-04
- [x] Spring Boot 애플리케이션이 정상적으로 실행되는지 확인 ➕ 2025-11-01 ✅ 2025-11-04
- [x] PostgreSQL 컨테이너 연결 확인 ➕ 2025-11-01 ✅ 2025-11-04
- [x] Redis 컨테이너 연결 확인 ➕ 2025-11-01 ✅ 2025-11-04
- [x] Spring Boot 애플리케이션에서 각 서비스 접근 테스트 ➕ 2025-11-01 ✅ 2025-11-04
- [x] docker-compose.yml Git에 커밋 ➕ 2025-11-01 ✅ 2025-11-04
- [x] Dockerfile Git에 커밋 ➕ 2025-11-01 ✅ 2025-11-04
- [x] 테스트 완료 커밋 ➕ 2025-11-01 ✅ 2025-11-04

- [x] 각종 환경변수 docker application properties 에서 환경변수로 빼버리기 ✅ 2025-11-05



### **Phase 1: Infrastructure Setup**
- [x] Docker 및 Docker Compose 설정
- [x] PostgreSQL, Redis, Spring Boot 컨테이너화
- [x] 환경변수 설정
- [x] 각 컨테이너 연결 테스트 연결 테스트

### **Phase 2: Core Booking Features**
- [x] 사용자 조회 및 생성 ✅ 2025-11-21
- [x] 좌석 조회 ✅ 2025-11-21
- [x] 좌석 예약 기능 (트랜잭션 처리) ✅ 2025-11-21
- [ ] 결제 (예약 완료 처리)
- [x] 기본적인 DB 테이블 설계 (✅ 251109)
- [ ] 예약 취소 및 시간 만료 처리


- [ ] 테스트 환경을 local 은 H2, CI/CD 는 TestContainer 로 바꾸기 (docker)

### **Phase 3: Redis Caching Integration**
- [ ] Redis를 이용한 좌석 상태 캐싱
- [ ] 대기열 시스템 구현
- [ ] 캐싱 로직 테스트

### **Phase 4: Cache Stampede Prevention**
- [ ] XFetch 알고리즘 구현
- [ ] Locking 방식 구현
- [ ] 다양한 캐싱 알고리즘 (LRU, LFU, FIFO) 비교

### **Phase 5: Monitoring & Tracing**
- [ ] Prometheus + Grafana 설정
- [ ] Jaeger/Pinpoint 분산 추적
- [ ] 성능 메트릭 수집

### **Phase 6: Performance Testing & Optimization**
- [ ] k6/JMeter를 이용한 부하 테스트
- [ ] 캐싱 성능 비교 분석
- [ ] 병목 지점 최적화

### **Phase 7: Documentation & Final Report**
- [ ] 실험 결과 정리
- [ ] 기술 문서 작성
- [ ] README 완성