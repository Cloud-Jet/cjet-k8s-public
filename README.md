CloudJet Kubernetes - GitOps 배포 레포지토리
Helm과 ArgoCD 기반으로 CloudJet 마이크로서비스를 Kubernetes에 배포하고 관리하는 레포지토리입니다.
개요
CloudJet 항공편 예약 시스템의 Kubernetes 배포를 관리합니다. 코드 변경부터 배포까지의 과정을 ArgoCD가 Git 상태와 동기화하는 방식으로 처리합니다.
구성 요소는 다음과 같습니다.

코드 변경 시 ArgoCD가 Git 변경을 감지해 자동 배포
Istio 서비스 메시를 통한 트래픽 관리 및 mTLS
Prometheus, Grafana, Kiali, Jaeger, Loki로 구성된 관측성 스택
External Secrets, RBAC, Network Policy 기반 보안 설정
KEDA를 이용한 이벤트 기반 오토스케일링
개발/스테이징/운영 환경별 values 분리

아키텍처
GitOps 워크플로우
#mermaid-r1mr-r1{font-family:"Anthropic Sans",system-ui,"Segoe UI",Roboto,Helvetica,Arial,sans-serif;font-size:16px;fill:#E5E5E5;}@keyframes edge-animation-frame{from{stroke-dashoffset:0;}}@keyframes dash{to{stroke-dashoffset:0;}}#mermaid-r1mr-r1 .edge-animation-slow{stroke-dasharray:9,5!important;stroke-dashoffset:900;animation:dash 50s linear infinite;stroke-linecap:round;}#mermaid-r1mr-r1 .edge-animation-fast{stroke-dasharray:9,5!important;stroke-dashoffset:900;animation:dash 20s linear infinite;stroke-linecap:round;}#mermaid-r1mr-r1 .error-icon{fill:#CC785C;}#mermaid-r1mr-r1 .error-text{fill:#3387a3;stroke:#3387a3;}#mermaid-r1mr-r1 .edge-thickness-normal{stroke-width:1px;}#mermaid-r1mr-r1 .edge-thickness-thick{stroke-width:3.5px;}#mermaid-r1mr-r1 .edge-pattern-solid{stroke-dasharray:0;}#mermaid-r1mr-r1 .edge-thickness-invisible{stroke-width:0;fill:none;}#mermaid-r1mr-r1 .edge-pattern-dashed{stroke-dasharray:3;}#mermaid-r1mr-r1 .edge-pattern-dotted{stroke-dasharray:2;}#mermaid-r1mr-r1 .marker{fill:#A1A1A1;stroke:#A1A1A1;}#mermaid-r1mr-r1 .marker.cross{stroke:#A1A1A1;}#mermaid-r1mr-r1 svg{font-family:"Anthropic Sans",system-ui,"Segoe UI",Roboto,Helvetica,Arial,sans-serif;font-size:16px;}#mermaid-r1mr-r1 p{margin:0;}#mermaid-r1mr-r1 .label{font-family:"Anthropic Sans",system-ui,"Segoe UI",Roboto,Helvetica,Arial,sans-serif;color:#E5E5E5;}#mermaid-r1mr-r1 .cluster-label text{fill:#3387a3;}#mermaid-r1mr-r1 .cluster-label span{color:#3387a3;}#mermaid-r1mr-r1 .cluster-label span p{background-color:transparent;}#mermaid-r1mr-r1 .label text,#mermaid-r1mr-r1 span{fill:#E5E5E5;color:#E5E5E5;}#mermaid-r1mr-r1 .node rect,#mermaid-r1mr-r1 .node circle,#mermaid-r1mr-r1 .node ellipse,#mermaid-r1mr-r1 .node polygon,#mermaid-r1mr-r1 .node path{fill:transparent;stroke:#A1A1A1;stroke-width:1px;}#mermaid-r1mr-r1 .rough-node .label text,#mermaid-r1mr-r1 .node .label text,#mermaid-r1mr-r1 .image-shape .label,#mermaid-r1mr-r1 .icon-shape .label{text-anchor:middle;}#mermaid-r1mr-r1 .node .katex path{fill:#000;stroke:#000;stroke-width:1px;}#mermaid-r1mr-r1 .rough-node .label,#mermaid-r1mr-r1 .node .label,#mermaid-r1mr-r1 .image-shape .label,#mermaid-r1mr-r1 .icon-shape .label{text-align:center;}#mermaid-r1mr-r1 .node.clickable{cursor:pointer;}#mermaid-r1mr-r1 .root .anchor path{fill:#A1A1A1!important;stroke-width:0;stroke:#A1A1A1;}#mermaid-r1mr-r1 .arrowheadPath{fill:#0b0b0b;}#mermaid-r1mr-r1 .edgePath .path{stroke:#A1A1A1;stroke-width:1px;}#mermaid-r1mr-r1 .flowchart-link{stroke:#A1A1A1;fill:none;}#mermaid-r1mr-r1 .edgeLabel{background-color:transparent;text-align:center;}#mermaid-r1mr-r1 .edgeLabel p{background-color:transparent;}#mermaid-r1mr-r1 .edgeLabel rect{opacity:0.5;background-color:transparent;fill:transparent;}#mermaid-r1mr-r1 .labelBkg{background-color:rgba(0, 0, 0, 0.5);}#mermaid-r1mr-r1 .cluster rect{fill:#CC785C;stroke:hsl(15, 12.3364485981%, 48.0392156863%);stroke-width:1px;}#mermaid-r1mr-r1 .cluster text{fill:#3387a3;}#mermaid-r1mr-r1 .cluster span{color:#3387a3;}#mermaid-r1mr-r1 div.mermaidTooltip{position:absolute;text-align:center;max-width:200px;padding:2px;font-family:"Anthropic Sans",system-ui,"Segoe UI",Roboto,Helvetica,Arial,sans-serif;font-size:12px;background:#CC785C;border:1px solid hsl(15, 12.3364485981%, 48.0392156863%);border-radius:2px;pointer-events:none;z-index:100;}#mermaid-r1mr-r1 .flowchartTitleText{text-anchor:middle;font-size:18px;fill:#E5E5E5;}#mermaid-r1mr-r1 rect.text{fill:none;stroke-width:0;}#mermaid-r1mr-r1 .icon-shape,#mermaid-r1mr-r1 .image-shape{background-color:transparent;text-align:center;}#mermaid-r1mr-r1 .icon-shape p,#mermaid-r1mr-r1 .image-shape p{background-color:transparent;padding:2px;}#mermaid-r1mr-r1 .icon-shape .label rect,#mermaid-r1mr-r1 .image-shape .label rect{opacity:0.5;background-color:transparent;fill:transparent;}#mermaid-r1mr-r1 .label-icon{display:inline-block;height:1em;overflow:visible;vertical-align:-0.125em;}#mermaid-r1mr-r1 .node .label-icon path{fill:currentColor;stroke:revert;stroke-width:revert;}#mermaid-r1mr-r1 .node .neo-node{stroke:#A1A1A1;}#mermaid-r1mr-r1 [data-look="neo"].node rect,#mermaid-r1mr-r1 [data-look="neo"].cluster rect,#mermaid-r1mr-r1 [data-look="neo"].node polygon{stroke:url(#mermaid-r1mr-r1-gradient);filter:drop-shadow( 1px 2px 2px rgba(185,185,185,1));}#mermaid-r1mr-r1 [data-look="neo"].node path{stroke:url(#mermaid-r1mr-r1-gradient);stroke-width:1px;}#mermaid-r1mr-r1 [data-look="neo"].node .outer-path{filter:drop-shadow( 1px 2px 2px rgba(185,185,185,1));}#mermaid-r1mr-r1 [data-look="neo"].node .neo-line path{stroke:#A1A1A1;filter:none;}#mermaid-r1mr-r1 [data-look="neo"].node circle{stroke:url(#mermaid-r1mr-r1-gradient);filter:drop-shadow( 1px 2px 2px rgba(185,185,185,1));}#mermaid-r1mr-r1 [data-look="neo"].node circle .state-start{fill:#000000;}#mermaid-r1mr-r1 [data-look="neo"].icon-shape .icon{fill:url(#mermaid-r1mr-r1-gradient);filter:drop-shadow( 1px 2px 2px rgba(185,185,185,1));}#mermaid-r1mr-r1 [data-look="neo"].icon-shape .icon-neo path{stroke:url(#mermaid-r1mr-r1-gradient);filter:drop-shadow( 1px 2px 2px rgba(185,185,185,1));}#mermaid-r1mr-r1 :root{--mermaid-font-family:"Anthropic Sans",system-ui,"Segoe UI",Roboto,Helvetica,Arial,sans-serif;}cjet-backend 코드 변경GitHub Actions CIDocker 이미지 빌드ECR Registry PushRepository Dispatchcjet-k8s values.yaml업데이트Git CommitArgoCD 변경 감지Helm Chart 배포Kubernetes Rolling Update
서비스 구조
┌─────────────────────────────────────────────────────────────┐
│                     Istio Gateway                           │
│                  (External LoadBalancer)                    │
└─────────────────────┬───────────────────────────────────────┘
                      │
┌─────────────────────┴───────────────────────────────────────┐
│                 Virtual Service                             │
│              (Traffic Routing)                              │
└─┬─────────────┬─────────────┬─────────────┬─────────────────┘
  │             │             │             │
┌─▼─────┐ ┌─────▼─┐ ┌─────────▼┐ ┌─────────▼┐ ┌──────────────▼┐
│ Auth  │ │Flight │ │ Booking │ │ Admin   │ │    Payment    │
│Service│ │Service│ │ Service │ │Service  │ │   Service     │
│(5001) │ │(5002) │ │ (5003)  │ │(5004)   │ │   (5005)      │
└───────┘ └───────┘ └─────────┘ └─────────┘ └───────────────┘
네임스페이스 구조

user: 사용자 대상 서비스 (auth, flight, booking)
admin: 관리자 서비스 (admin-service, payment-service)
monitoring: 관측성 스택 (Prometheus, Grafana, Kiali, Jaeger, Loki)
istio-system: Istio 컨트롤 플레인
keda: KEDA 오토스케일링 오퍼레이터

Helm 차트 구조
helm/
├── Chart.yaml                    # Helm 차트 메타데이터
├── values.yaml                   # 기본 설정 값
├── values-dev.yaml               # 개발 환경 설정
├── values-prod.yaml              # 운영 환경 설정
├── templates/
│   ├── deployments.yaml          # 마이크로서비스 배포
│   ├── services.yaml             # 서비스 정의
│   ├── configmap.yaml            # 설정 맵
│   ├── external-secrets.yaml     # AWS Secrets Manager 연동
│   ├── serviceaccounts.yaml      # 서비스 계정
│   ├── namespace.yaml            # 네임스페이스 생성
│   ├── gateway.yaml              # Istio Gateway
│   ├── virtualservice.yaml       # Istio Virtual Service
│   ├── destinationrule.yaml      # Istio Destination Rules
│   ├── admin-namespace-mtls.yaml # mTLS 정책
│   ├── keda-scaledobject.yaml    # KEDA 오토스케일링
│   ├── istio-gateway-service.yaml # 게이트웨이 서비스
│   └── servicemonitor.yaml       # Prometheus 모니터링
└── .helmignore
values.yaml 구성
기본 설정
yamlglobal:
  namespace: user
  applicationNamespaces: ["user", "admin"]
  imagePullPolicy: Always

services:
  auth:
    name: auth-service
    image:
      repository: public.ecr.aws/v3g6g4v7/cj-auth-svc
      tag: v5.0.0
    port: 5001
    replicas: 2
    resources:
      requests:
        memory: "256Mi"
        cpu: "250m"
      limits:
        memory: "512Mi"
        cpu: "500m"

  admin:
    name: admin-service
    namespace: admin  # Admin 전용 네임스페이스
    replicas: 1
보안 설정
yaml# ServiceAccount with IAM Role 연동
serviceAccounts:
  accounts:
    - name: cloudjet-secrets-sa
      roleArn: arn:aws:iam::556683426101:role/CloudJetSecretsRole
      namespaces: ["user", "admin"]

# External Secrets 설정
secrets:
  externalSecrets:
    enabled: true
    storeName: aws-secrets-store
    serviceAccountName: cloudjet-secrets-sa
Istio 설정
yamlistio:
  gateway:
    enabled: true
    hosts:
      - cloudjet.example.com
  virtualService:
    enabled: true
    routes:
      - match:
          - uri:
              prefix: /api/auth
        route:
          - destination:
              host: auth-service
배포 가이드
사전 요구사항

Kubernetes 1.24+
Helm 3.0+
Istio 1.27+
ArgoCD 2.8+
kubectl 접근 권한

1. 네임스페이스 생성
bashkubectl create namespace cloudjet
kubectl create namespace monitoring
2. Istio 라벨링
bashkubectl label namespace cloudjet istio-injection=enabled
3. Helm 배포
bash# 레포지토리 클론
git clone https://github.com/Cloud-Jet/cjet-k8s-public.git
cd cjet-k8s-public

# Helm 배포
helm install cloudjet ./helm -n cloudjet

# 또는 특정 값으로 배포
helm install cloudjet ./helm -n cloudjet \
  --set services.auth.image.tag=v1.0.0 \
  --set services.auth.replicas=3
4. ArgoCD Application 생성
bashargocd app create cloudjet-app \
  --repo https://github.com/Cloud-Jet/cjet-k8s-public.git \
  --path helm \
  --dest-server https://kubernetes.default.svc \
  --dest-namespace cloudjet \
  --sync-policy automated \
  --auto-prune \
  --self-heal
설정 관리
환경별 설정
개발 환경 (values-dev.yaml)
yamlglobal:
  environment: development

services:
  auth:
    replicas: 1
    resources:
      requests:
        memory: "128Mi"
        cpu: "100m"
운영 환경 (values-prod.yaml)
yamlglobal:
  environment: production

services:
  auth:
    replicas: 3
    resources:
      requests:
        memory: "256Mi"
        cpu: "250m"
      limits:
        memory: "512Mi"
        cpu: "500m"
환경별 배포
bash# 개발 환경
helm upgrade cloudjet ./helm -n cloudjet-dev -f values-dev.yaml

# 운영 환경
helm upgrade cloudjet ./helm -n cloudjet-prod -f values-prod.yaml
보안
External Secrets Operator
yaml# AWS Secrets Manager 연동
apiVersion: external-secrets.io/v1beta1
kind: ExternalSecret
metadata:
  name: cloudjet-unified-secret
spec:
  secretStoreRef:
    name: aws-secrets-store
    kind: SecretStore
  target:
    name: cloudjet-secrets
    creationPolicy: Owner
  data:
    - secretKey: SECRET_KEY
      remoteRef:
        key: cloudjet/secrets
        property: SECRET_KEY
Service Account & RBAC
yamlapiVersion: v1
kind: ServiceAccount
metadata:
  name: cloudjet-secrets-sa
  annotations:
    eks.amazonaws.com/role-arn: arn:aws:iam::556683426101:role/cloudjet-secrets-role
Network Policies
yamlapiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: cloudjet-network-policy
spec:
  podSelector:
    matchLabels:
      app: cloudjet
  policyTypes:
  - Ingress
  - Egress
  ingress:
  - from:
    - namespaceSelector:
        matchLabels:
          name: istio-system
자동 스케일링 (KEDA)
ScaledObject 설정
yamlapiVersion: keda.sh/v1alpha1
kind: ScaledObject
metadata:
  name: auth-service-scaler
spec:
  scaleTargetRef:
    name: auth-service
  minReplicaCount: 2
  maxReplicaCount: 10
  triggers:
  - type: prometheus
    metadata:
      serverAddress: http://prometheus:9090
      metricName: http_requests_per_second
      threshold: '30'
      query: sum(rate(http_requests_total{job="auth-service"}[1m]))
지원 스케일링 트리거

Prometheus 메트릭 (CPU, 메모리, 사용자 정의 메트릭)
HTTP 요청량 기반
Redis Queue 길이 기반
AWS CloudWatch 메트릭 연동

스케일링 정책
yamlbehavior:
  scaleDown:
    stabilizationWindowSeconds: 300
    policies:
    - type: Percent
      value: 10
      periodSeconds: 60
  scaleUp:
    stabilizationWindowSeconds: 0
    policies:
    - type: Percent
      value: 100
      periodSeconds: 15
모니터링
Prometheus 연동
yaml# ServiceMonitor for Prometheus
apiVersion: monitoring.coreos.com/v1
kind: ServiceMonitor
metadata:
  name: cloudjet-monitor
spec:
  selector:
    matchLabels:
      app: cloudjet
  endpoints:
  - port: metrics
    interval: 30s
    path: /metrics
관측성 스택
Prometheus (메트릭 수집)

스토리지: 20GB gp2 볼륨, 15일 보존
수집 대상: Istio, Kubernetes, 애플리케이션 메트릭
리소스: 1-2GB 메모리, 0.5-1 CPU

Grafana (시각화)

데이터소스: Prometheus, Loki 연동
대시보드: 애플리케이션, 비즈니스, Istio 메트릭
스토리지: 10GB 지속적 볼륨
접근: grafana.cloudjet.click

Kiali (서비스 메시 관측성)

기능: 서비스 토폴로지, 트래픽 플로우 시각화
연동: Prometheus, Grafana, Jaeger 통합
접근: kiali.cloudjet.click

Jaeger (분산 추적)

설정: All-in-One 배포, 메모리 스토리지
용량: 최대 50,000 traces
접근: jaeger.cloudjet.click

Loki (로그 집계)

백엔드: AWS S3 (cloudjet-loki-storage)
보존: 7일 로그 보관
IAM: loki-irsa-role 서비스 계정

알람 설정
yaml# PrometheusRule
apiVersion: monitoring.coreos.com/v1
kind: PrometheusRule
metadata:
  name: cloudjet-alerts
spec:
  groups:
  - name: cloudjet.rules
    rules:
    - alert: HighErrorRate
      expr: rate(http_requests_total{status=~"5.."}[5m]) > 0.1
      for: 5m
      annotations:
        summary: "High error rate detected"
CI/CD 통합
배포 프로세스

cjet-backend 레포지토리 코드 변경 감지
GitHub Actions에서 Docker 이미지 빌드
CD 워크플로우가 values.yaml 태그 자동 업데이트
ArgoCD가 변경사항 감지 및 동기화
롤링 업데이트로 무중단 배포

배포 전략
yaml# Rolling Update 설정
strategy:
  type: RollingUpdate
  rollingUpdate:
    maxUnavailable: 25%
    maxSurge: 25%

# Health Check
livenessProbe:
  httpGet:
    path: /api/auth/health
    port: 5001
  initialDelaySeconds: 30
  periodSeconds: 10

readinessProbe:
  httpGet:
    path: /api/auth/health
    port: 5001
  initialDelaySeconds: 5
  periodSeconds: 5
테스트
Helm 차트 테스트
bash# 템플릿 렌더링 테스트
helm template cloudjet ./helm --debug

# 문법 검사
helm lint ./helm

# 드라이런 테스트
helm install cloudjet ./helm --dry-run --debug
배포 후 검증
bash# 모든 파드 상태 확인
kubectl get pods -n cloudjet

# 서비스 연결 테스트
kubectl exec -n cloudjet deployment/auth-service -- curl http://auth-service:5001/api/auth/health

# Istio 트래픽 확인
kubectl exec -n istio-system deployment/istiod -- istioctl proxy-status
트러블슈팅
파드가 시작되지 않는 경우
bash# 이벤트 확인
kubectl describe pod <pod-name> -n cloudjet

# 로그 확인
kubectl logs <pod-name> -n cloudjet -f

# 리소스 확인
kubectl top pods -n cloudjet
서비스 연결 문제
bash# 서비스 엔드포인트 확인
kubectl get endpoints -n cloudjet

# DNS 해결 테스트
kubectl exec -n cloudjet deployment/auth-service -- nslookup flight-service

# Istio 설정 확인
istioctl analyze -n cloudjet
외부 접근 문제
bash# 게이트웨이 상태 확인
kubectl get gateway -n cloudjet

# Virtual Service 확인
kubectl get vs -n cloudjet

# 로드밸런서 IP 확인
kubectl get svc -n istio-system istio-ingressgateway
업데이트 가이드
버전 업데이트
bash# 특정 서비스만 업데이트
helm upgrade cloudjet ./helm -n cloudjet \
  --set services.auth.image.tag=v2.0.0

# 전체 업데이트
git pull origin main
helm upgrade cloudjet ./helm -n cloudjet
롤백
bash# 롤백 이력 확인
helm history cloudjet -n cloudjet

# 이전 버전으로 롤백
helm rollback cloudjet 1 -n cloudjet

# Kubernetes 네이티브 롤백
kubectl rollout undo deployment/auth-service -n cloudjet
기여하기

레포지토리 Fork
Feature Branch 생성
변경사항 구현 및 테스트
Pull Request 생성
코드 리뷰 및 병합

개발 가이드라인

Helm 모범 사례 준수
YAML 문법 검사 필수
변경사항에 대한 문서 업데이트
템플릿 주석 및 설명 추가

지원

GitHub Issues: cjet-k8s Issues
Wiki: 프로젝트 위키
관련 프로젝트: cjet-backend
