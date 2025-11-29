---
tags:
  - DevOps/Docker
---

## Docker Basics

### Docker vs VM
- **VM**: 하드웨어 가상화 + Guest OS 포함 (무거움).
- **Docker**: 호스트 OS 커널 공유 + 프로세스 격리 (가벼움).

### Docker Compose
- 여러 컨테이너를 정의하고 실행하기 위한 도구 (`docker-compose.yml`).
- 서비스, 네트워크, 볼륨 등을 정의하여 한 번에 관리 (`up`, `down`).

### Volume
- 컨테이너 데이터의 영속성(Persistence)을 보장하기 위한 저장소.
- 컨테이너가 삭제되어도 볼륨에 저장된 데이터는 유지됨 (DB 데이터 보존에 필수).
- `postgres_data:/var/lib/postgresql/data` 와 같이 마운트.
