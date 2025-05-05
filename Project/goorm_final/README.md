# 구름 쿠버네티스 전문가 양성 과정 - 파이널 프로젝트

## 프로젝트 개요
이 프로젝트는 AWS EKS를 기반으로 한 클라우드 네이티브 애플리케이션 배포 및 운영 환경을 구축한 것입니다. 마이크로서비스 아키텍처를 채택하여 애플리케이션을 구성하고, Istio를 통한 서비스 메시 구현, 모니터링, CI/CD 파이프라인 구축 등을 포함합니다.

## 주요 기술 스택
- **인프라**: AWS EKS (Kubernetes 1.25.6)
- **서비스 메시**: Istio
- **로드 밸런싱**: AWS ALB + Istio Ingress Gateway
- **모니터링**: Prometheus, Grafana, Alertmanager
- **CI/CD**: Jenkins, Argo Rollout
- **도메인 관리**: AWS Route53
- **인증서 관리**: cert-manager, AWS ACM

## 프로젝트 구조
```
goorm_final/
├── argo rollout/    # Argo Rollout 설정 및 배포 전략
├── eks/            # EKS 클러스터 설정 및 관리
├── istio/          # Istio 설정 및 서비스 메시 구성
├── jenkins/        # Jenkins 파이프라인 및 설정
└── prometheus/     # 모니터링 시스템 설정
```

## 주요 기능 및 구현 내용

### 1. 인프라 구성
- AWS EKS 클러스터 구축
- IAM 기반 사용자 관리 및 RBAC 설정
- Route53을 통한 도메인 관리
- ALB와 Istio Ingress Gateway 연동

### 2. 보안
- cert-manager를 통한 자동 인증서 발급 및 관리
- HTTPS 종단간 암호화 구현

### 3. 모니터링
- Prometheus, Grafana, Alertmanager 구축
- Istio 메트릭 수집 및 시각화
- 커스텀 알림 규칙 설정
- Slack 연동

### 4. CI/CD
- Jenkins Organization Job을 통한 멀티 브랜치 파이프라인
- Argo Rollout을 통한 카나리 배포
- Gradle 빌드 최적화
- Docker 이미지 빌드 및 배포 자동화

## 트러블슈팅

### 1. ALB와 Istio 연동 문제
- **문제**: ALB Ingress Controller가 Istio Ingress Gateway로 트래픽을 전달하지 못함. HTTPS 연결 시 TLS 설정 충돌 발생  
- **해결**:  
  - Istio Gateway를 NodePort로 설정하여 ALB 대상 그룹과 연결  
  - cert-manager를 통해 TLS 인증서 자동 발급  
  - Gateway 리소스에 TLS 시크릿과 관련 설정 추가

### 2. Gradle 빌드 시간 최적화
- **문제**: Docker 컨테이너 내 Gradle 빌드 시간이 길어짐
- **해결**:
  - Jenkins on Kubernetes에서 PVC를 통한 Gradle 캐시 공유
  - 멀티 스테이지 빌드 도입

### 3. Jenkins CI 구성 오류
- **문제**: GitHub Organization Job에서 브랜치 인식 및 Webhook 동작 실패  
- **해결**:  
  - GitHub Personal Access Token 등록  
  - Credentials 재설정 및 webhook 수동 구성  
  - 브랜치 스캔 주기 조절

### 4. 모니터링 시스템 구축
- **문제**: Prometheus 메트릭 수집 누락  
- **해결**:  
  - kube-prometheus-stack 버전 호환성 점검  
  - ServiceMonitor, PodMonitor 명시적 등록  
  - Grafana 대시보드 자동 import 설정

### 5. 인증서 자동 갱신 문제
- **문제**: 와일드카드 인증서 발급 실패 또는 만료 후 자동 갱신 누락  
- **해결**:  
  - Route53과 cert-manager 연동 (DNS01 challenge)  
  - ClusterIssuer 설정에 `solvers` 추가  
  - cert-manager 컨트롤러 기반 실시간 갱신 확인


## 참고 문서
- [AWS ALB + Istio 연동 가이드](eks/AWS%20ALB%20+%20Istio.md)
- [cert-manager 사용 가이드](eks/cert-manager%20사용.md)
- [Gradle 빌드 최적화 가이드](eks/gradle%20빌드%20시간%20단축하기.md)
- [Jenkins Organization Job 설정](eks/Jenkins%20organization%20job.md)
- [kube-prometheus-stack 설치 가이드](eks/kube-prometheus-stack%20설치하기.md)
- [Route53 도메인 등록 가이드](eks/Route53으로%20도메인%20등록하기.md)
- [EKS 사용자 관리 가이드](eks/유저%20추가하기.md)