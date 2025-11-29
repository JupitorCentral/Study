---
tags:
  - DevOps/Monitoring
---

## Monitoring Stack (LGP Stack)

Docker 환경에서 Spring Boot 애플리케이션의 로그와 메트릭을 수집/시각화하는 스택.

### 1. Promtail (Log Collector)
- 로그 수집 에이전트.
- 컨테이너나 파일의 로그를 꼬리물기(tailing)하여 수집.
- Loki로 **Push** 방식으로 전송.

### 2. Loki (Log Aggregation System)
- 로그 저장 및 쿼리 엔진 (Like Prometheus, but for logs).
- 로그 전체를 인덱싱하지 않고 **메타데이터(라벨)**만 인덱싱하여 가볍고 효율적.
- LogQL 쿼리 언어 사용.

### 3. Grafana (Visualization)
- 데이터 시각화 대시보드.
- Loki, Prometheus 등을 데이터 소스로 연결하여 로그와 메트릭을 한 곳에서 조회.
