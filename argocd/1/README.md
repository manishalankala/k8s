### install ArgoCD in k8s
kubectl create namespace argocd
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml

### Access ArgoCD UI
kubectl get svc -n argocd
kubectl port-forward svc/argocd-server 8080:443 -n argocd

### To set the Service type to NodePort

kubectl patch svc argocd-server -p '{"spec": {"type": "NodePort"}}' -n argocdkubectl patch svc argocd-server -p '{"spec": {"type": "NodePort"}}' -n argocd

### Login with admin user and below token (as in documentation):
kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 --decode && echo


### To login 
kubectl get pod -n argocd |grep argocd-server

Pod ID is the password

http://{HOST_IP}:NODEPORT


### To login via cli 
curl -sSL -o /usr/local/bin/argocd https://github.com/argoproj/argo-cd/releases/download/v1.8.3/argocd-linux-amd64
chmod +x /usr/local/bin/argocd

argocd login {HOST_IP}:NODEPORT --username admin --password podid

### Create sample app via ui

argocd app create guestbook --repo https://github.com/argoproj/argocd-example-apps.git --path guestbook --dest-server https://kubernetes.default.svc --dest-namespace default

kubectl get all -n default

argocd app delete guestbookargocd app delete guestbook
