
# 1. Sidecar Pattern

# Neflix Sidecar

# Service Mesh
Microservice 환경에서 다양한 서비스들이

- Netflix oss
- Istio

# Service Mesh platform
## Istio

#### 주요기능
다음 기능을 통해 서비스를 플랫폼과 관리 정책등에서 분리하여
서비스 개발 및 운영을 유연하게 돕는다  
- Traffic Management  
  App 서비스들 간의 네트워크 흐름과 API 호출을 제어 함
- Service Identity and Security  
  App 서비스들을 고유하게 관리하고 서비스들 간의 통신을 보호
- Policy enforcement  
  서비스 정책을 범용 적용 가능  
- Telemetry  
  서비스들간의 의존도 및 상태를 모니터링 하기 위한 Metric

  #### 구성
  - Data plane(proxy)
    Sidecar pattern으로 서비스에 포함되며(Envoy) App 서비스들간의 통신 및 Control plane관의 통신을 제어
  - Control plane
    Proxy를 관리하고 Mixer롤 통해 서비스들의 telemetry를 관리  

  <img height="300" src="images/envoy-architecture.PNG">  

  - Envoy
    서비스들의 모든 트레픽에 관여하여 service mesh 구성을 지원
    service discovery, load balancing, TLS termination, HTTP/2 & gRPC proxying, circuit breakers, health checks, staged rollouts with %-based traffic split, fault injection, and rich metrics 가능
    applicatin instance pool을 갖고 있어서 loadbalancing 지원 및 health check를 통한 instance 상태 최신화  
      - circuit breaker
        hystrix와는 다르게 envoy가 HTTP에러를 발생하고, 이를 호출한 app 서비스에서 fallback처리 해줘야 함

  - Mixer
    Access control, usage policies를 proxy를 통해 적용하고 telemetry를 수집 함

  - Pilot   
    Service discovery for envoys, dynamic routing resiliency(timeout, retires, circuit breakers) 기능 지원   
    <img height="300" src="images/pilot-architecture.PNG">  
    <img height="300" src="images/envoy-discovery-lb.PNG">  

  - Citadel  
    자체 credential을 이용한 인증 기능

    #### Rule configuration
    Virtual service에 Istio service mesh에서 서비스간에 어떻게 요청을 처리할지 정의 함
    [참고]https://istio.io/docs/concepts/traffic-management/rules-configuration/


## Linkerd


일정

금주 조사  ~ 7/27
2주후 까지 구현  7/30 ~ 8/17
1주후 DEP 구성을 위한 DEP 현황 공유 8/8일쯤

3 or 4주 후 부터 DEP POC 8/20 ~ 9/7

정리 및 보고 9/10 ~ 9/12


버퍼 17
