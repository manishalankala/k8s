


1.Virtual services (The hosts field, Routing rules, Destination, Routing rule precedence ,)

2.Destination rules

3.Gateways

4.Service entries

5.Sidecars







Pilot :


Routing. For example 90% of the traffic goes to the version 1 of a microservice and the remaining 10% goes to the version 2. Or some specific requests go to the version 1 and all the others to the version 2, according to some condition. And also: a) rewrite b) redirect
Support for microservices development, deployment and testing: a) timeouts b) retries c) circuit breakers d) load balancing e) fault injection for testing


Mixer:

Reporting: Logging, Distributed Tracing, Telemetry
Policy enforcement

Citadel (previously CA, previously Auth):

Secure communication between micro services and strong identity.




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
