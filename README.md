# TicketRail

### Kubernetes 기반 티켓 예매 서비스의 하이브리드 클라우드 확장 및 성능 검증

TicketRail은 Kubernetes 기반 티켓 예매 서비스를 구축하고, 온프레미스 환경에서 발생한 병목을 분석하여 Hybrid Cloud 및 Full AWS 환경으로 확장한 팀 프로젝트입니다.

대기열, 좌석 선점, 예매, 결제 및 알림 기능을 구현하고, 인프라 고가용성과 환경별 처리 성능을 검증했습니다.

## 1. 프로젝트 개요

| 구분 | 내용 |
|---|---|
| 프로젝트 | TicketRail |
| 프로젝트 유형 | 팀 프로젝트 |
| 구성 환경 | On-Premises → Hybrid Cloud → Full AWS |
| 주요 목표 | 고가용성 확보, 병목 개선, 클라우드 확장 및 성능 검증 |
| 핵심 기술 | Kubernetes, AWS, ROSA, Terraform, Ansible, Redis, PostgreSQL, Kafka |

## 2. 주요 서비스 기능

- **대기열 관리:** Redis 기반 대기열을 통한 사용자 요청 제어
- **좌석 선점:** 동시 예매 요청에 대한 좌석 선점 처리
- **티켓 예매:** PostgreSQL을 활용한 예매 데이터 관리
- **비동기 결제:** Kafka 기반 결제 이벤트 처리
- **알림 처리:** 결제 이후 알림 이벤트 처리

### 서비스 처리 흐름

사용자 요청 → 대기열 진입 → 좌석 선점 → 예매 처리 → Kafka 결제 이벤트 → 알림 처리

## 3. 프로젝트 구성

### 1차 프로젝트 — On-Premises

온프레미스 Kubernetes 환경에 티켓 예매 서비스를 구축하고, 외부 트래픽 처리 경로와 장애 대응 구성을 검증했습니다.

- Kubernetes 기반 애플리케이션 배포
- MetalLB 및 NGINX Ingress를 통한 외부 접근 구성
- Ingress Controller 이중화
- Ingress Pod 및 Worker Node 장애 테스트
- Prometheus, Grafana, k6를 활용한 모니터링 및 성능 검증

[1차 프로젝트 아키텍처 보기](docs/images/architecture.png)

### 2차 프로젝트 — Hybrid Cloud

기존 온프레미스 환경과 AWS를 연결하여 일부 처리 자원을 클라우드로 확장했습니다.

- Tailscale 기반 온프레미스–AWS 사설 네트워크 연결
- AWS ROSA(OpenShift) 환경 구성
- Terraform 기반 클라우드 인프라 구성
- Consumer 처리 자원 확장 및 성능 검증

[Hybrid Cloud 논리 아키텍처 보기](docs/images/hybrid-logical.png)

[Hybrid Cloud 물리 아키텍처 보기](docs/images/hybrid-physical.png)

### 2차 프로젝트 — Full AWS

서비스 구성 요소를 AWS 환경으로 확장하고, 온프레미스 및 하이브리드 환경과 처리 성능을 비교했습니다.

- AWS ROSA 기반 애플리케이션 운영 환경
- Amazon RDS for PostgreSQL
- Amazon ElastiCache for Valkey
- Amazon MSK
- HPA 및 KEDA 기반 자동 확장 구성
- 환경별 부하 테스트 및 처리 성능 비교

[Full AWS 논리 아키텍처 보기](docs/images/full-aws-logical.png)

[Full AWS 물리 아키텍처 보기](docs/images/full-aws-physical.png)

## 4. 주요 기술 스택

| 구분 | 기술 |
|---|---|
| Container / Orchestration | Docker, Kubernetes, ROSA(OpenShift) |
| Cloud / Network | AWS, VPC, EC2, Tailscale, NGINX Ingress, MetalLB |
| Database / Cache | PostgreSQL, Redis, Amazon RDS, Amazon ElastiCache for Valkey |
| Messaging | Apache Kafka, Amazon MSK |
| Automation / IaC | Ansible, Terraform |
| CI/CD | GitHub Actions, GHCR |
| Monitoring / Test | Prometheus, Grafana, k6 |
| Autoscaling | HPA, KEDA |

## 5. 저장소 구조

```text
Project-Ticketrail/
├── .github/workflows/       # CI/CD 관련 설정
├── ansible/                 # Ansible 자동화 자료
├── Data_Monitoring_playbooks/
├── docs/                    # 아키텍처 및 검증 문서
├── failure-tests/           # 장애 테스트 관련 자료
├── hybrid-cloud/            # Hybrid / AWS 인프라 구성
│   ├── monitoring/
│   └── terraform/
├── k8s_playbooks/           # Kubernetes 배포 설정
├── kubernetes/              # Kubernetes 구성 자료
├── metallb/                 # MetalLB 설정
├── playbooks/               # 자동화 Playbook
├── src/                     # 애플리케이션 소스 코드
├── test/                    # 테스트 코드
├── ticketing-consumer-mock/
├── docker-compose.local.yml
├── Dockerfile
├── package.json
└── README.md
```

## 6. 상세 문서

- [개인 기여 범위](docs/contribution.md)
- [고가용성 검증 결과](docs/validation.md)

---

본 저장소는 팀 프로젝트에서 개발한 애플리케이션 및 인프라 코드를 포함하며, 각 구성 요소는 팀원들이 역할을 분담하여 구현했습니다.