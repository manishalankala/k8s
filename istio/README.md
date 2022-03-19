


1.Virtual services (The hosts field, Routing rules, Destination, Routing rule precedence ,)

2.Destination rules

3.Gateways

4.Service entries

5.Sidecars






-------------------

Pilot :


Routing. For example 90% of the traffic goes to the version 1 of a microservice and the remaining 10% goes to the version 2. Or some specific requests go to the version 1 and all the others to the version 2, according to some condition. And also: a) rewrite b) redirect
Support for microservices development, deployment and testing: a) timeouts b) retries c) circuit breakers d) load balancing e) fault injection for testing

provides services discovery for the Envoy sidecars, traffic management capabilities for intelligent routing (A/B testing, canary deployments ..etc) and resiliency (timeouts, retries, circuit breakers ..etc).

-------------------


Mixer:

Reporting: Logging, Distributed Tracing, Telemetry
Policy enforcement

platform-independent component which accesses control and usage policies across the service-mesh and collects telemetry data from the envoy proxy and other services. The proxy extracts request level attributes and send them to Mixer to evaluate

-------------------

Citadel (previously CA, previously Auth):

Secure communication between micro services and strong identity.

It provides strong service-to-service and end-user authentication with built-in identity and credential management. This can be used to upgrade unencrypted traffic in the service mesh. Using Citadel, operators can enforce policies based on service identity rather than on network controls.

-------------------


Galley :

It validates user-authored Istio API configuration on behalf of the other Istio control plane components. Over time, Galley will take over responsibility as the top-level configuration ingestion, processing and distribution component on Istio. It will be responsible for insulting the rest of the Istio components form the details of the Istio components from the details of obtaining user configuration from the underlying platform (Kubernetes).



Note : 

Envoy, the sidecar proxy, gets its routing and configuration tables from Pilot to implement . 

Envoy reports to Mixer about each request, to implement . 

Envoy asks Mixer to allow or forbid requests, to implement .

Envoy gets certificates from Citadel to implement.



## Ingress

Controlling ingress traffic for an Istio service mesh.

1. Ingress Gateways : 

A Gateway provides more extensive customization and flexibility than Ingress, and allows Istio features such as monitoring and route rules to be applied to traffic entering the cluster.

Ref : https://istio.io/latest/docs/tasks/traffic-management/ingress/ingress-control/


2. Kubernetes Ingress : 

exposes HTTP and HTTPS routes from outside the cluster to services within the cluster.

Ref : https://istio.io/latest/docs/tasks/traffic-management/ingress/kubernetes-ingress/


3. Secure Gateways : 

to configure an ingress gateway to expose an HTTP service to external traffic.to expose a secure HTTPS service using either simple or mutual TLS

Ref : https://istio.io/latest/docs/tasks/traffic-management/ingress/secure-ingress/

4. Kubernetes Gateway API :

to expose a service outside the service mesh cluster using the Kubernetes Gateway API.

Ref : https://istio.io/latest/docs/tasks/traffic-management/ingress/gateway-api/


5.Ingress Gateway without TLS Termination

to configure HTTPS ingress access to an HTTP service.

Ref : https://istio.io/latest/docs/tasks/traffic-management/ingress/ingress-sni-passthrough/


--------------------------------------------

Custom configuration :

https://istio.io/latest/docs/setup/additional-setup/customize-installation/
