
## Service Mesh

The term __service mesh__ is used to describe the network of microservices and the interactions between them.

Features include
- __Security__ rate limiting, and end-to-end authentication
- __Connect__ service discovery,load balancing, A/B testing, canary rollouts, failure recovery(retry, circuit breaker)
- __Observe__  metrics, and monitoring
- __Control__  access control

### Why service mesh vs network policies

Service mesh oprates at Level 7 of OSI model give more features compare to Level 3 of network level with network policies

### Istio

Istio is a completely open source service mesh that layers transparently onto existing distributed applications. It’s also a platform, including APIs, that let it integrate into any logging platform, or telemetry or policy system.

Istio works by having a small network proxy sit alongside each microservice called “sidecar”. It’s role is to intercept all of the service’s traffic, and handles it more intelligently than a simple layer 3 network can.

An Istio service mesh is logically split into a data plane and a control plane.
- The __data plane__ is composed of a set of intelligent proxies (Envoy) deployed as sidecars. These proxies mediate and control all network communication between microservices. They also collect and report telemetry on all mesh traffic.
- The __control plane__ manages and configures the proxies to route traffic.

#### Components

__Envoy Proxy__ Processes the inbound/outbound traffic from inter-service and service-to-external-service transparently.
__Pilot__ provides service discovery for the Envoy sidecars, traffic management capabilities for intelligent routing (e.g., A/B tests, canary deployments, etc.), and resiliency (timeouts, retries, circuit breakers, etc.)
__Citadel__ enables strong service-to-service and end-user authentication with built-in identity and credential management.
__Galley__ is Istio’s configuration validation, ingestion, processing and distribution component. It is responsible for insulating the rest of the Istio components from the details of obtaining user configuration from the underlying platform (e.g. Kubernetes).

#### Download and run sample application with Istio

https://istio.io/docs/setup/getting-started/
