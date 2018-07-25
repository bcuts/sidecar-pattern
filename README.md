# Sidecar pattern

# 1. 개요
사이드카는 본체 서비스의 지원 기능을 제공 합니다.
- 모니터링,
- 로깅,
- 네트워킹
- 설정
- 폴리글랏 서비스 브릿징

#### 특징
App 서비스에 지원 기능이 포함 된 경우 지원기능으로 인한 이슈 발생 시, 서비스에도 영향을 주는것을 방지
서로 다른 언어로 개발된 서비스 도 MSA 환경에 포함하여 디스커버리, 통합 모니터링, 로깅, 헬스체크 등이 가능
다른 벤더 서비스도 MSA 환경에 포함하여 헬스체크, 모니터링이 가능  
App 서비스와 사이드카 모듈을 동일 컨테이너에 배포하여 리소스 모니터링 등의 작업 가능

하지만, 이런 사이드카 서비스에 대한 관리로 인해 복잡성이 증가 될 수 있음

사이트카 패턴을 이용하여,
다양한 서비스에 대하여 같은 유형의 인터페이스를 제공하여 MSA 환경에서 통합 관리가 되도록 하면서,
관리의 복잡성을 최소화 함. 서비스 메쉬 구성에 필요

#### 대상
- infra
- network
- logging
- configuration
- 이기종 서비스??

#### Netflix sidecar plugin
지원기능
- Eureka, Ribbon
-  

# Service mesh 지원 platform
Microservice들의 Service mesh가 커지고 복잡해 질 수록 서비스 상태 파악 및 관리가 어려워진다.

## Istio
Lyft's envoy를 proxy로 사용
다이나믹 라우팅, 서비스 디스커버리, 로드밸런싱, TLS 터미네이션, gRPC 프로싱 등 기능 제공

Kubernetes와 Eureka에 들옥 된 서비스도 지원가능

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

#### 기타 mesh platform
- Linkerd
Netty, Finagle위에 올라 감
다이나믹 라우팅(A/B 테스트팅 활용 가능), 서비스 디스커버리, 로드밸런싱, TLS 터미네이션, gRPC 프로싱 등 기능 제공

## Envoy  
