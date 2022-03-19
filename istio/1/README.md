

REF : https://istio.io/latest/docs/setup/getting-started/#download


curl -L https://istio.io/downloadIstio | sh -

cd istio-1.13.2

export PATH=$PWD/bin:$PATH

export PATH="$PATH:/root/istio-1.13.2/bin"

istioctl install --set profile=demo -y

kubectl label namespace default istio-injection=enabled

kubectl get namespace -L istio-injection

kubectl -n istio-system get svc

kubectl apply -f info.yaml

kubectl apply -f gateway.yaml

kubectl -n istio-system get service istio-ingressgateway

export INGRESS_HOST=$(kubectl -n istio-system get service istio-ingressgateway -o jsonpath='{.status.loadBalancer.ingress[0].ip}')

export INGRESS_PORT=$(kubectl -n istio-system get service istio-ingressgateway -o jsonpath='{.spec.ports[?(@.name=="http2")].port}')

export SECURE_INGRESS_PORT=$(kubectl -n istio-system get service istio-ingressgateway -o jsonpath='{.spec.ports[?(@.name=="https")].port}')

echo http://$INGRESS_HOST:$INGRESS_PORT/productpage

## Accessing The Kiali Service

kubectl -n istio-system port-forward  svc/kiali 20001:20001 ## Using port forwarding

kubectl patch service kiali --patch '{"spec":{"type":"LoadBalancer"}}' -n istio-system ## Convert the service to LoadBalancer

kubectl -n istio-system get service kiali -o jsonpath='{.status.loadBalancer.ingress[0].ip} ## Then get the IP address and port

kubectl -n istio-system get service kiali -o jsonpath='{.spec.ports[?(@.name=="http-kiali")].port}'

Username: admin

Password: admin

## Traffic Monitoring
curl -o /dev/null -s -w %{http_code} http://publicip/productpage
