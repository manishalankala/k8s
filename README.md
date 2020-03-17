# k8s


Contents
| ------- |
Application Lifecycle Management 8% |
Installation, Configuration & Validation 12% |
Kubernetes Core Concepts 19% |
Kubernetes Networking 11% |
Kubernetes Scheduling 5% |
Kubernetes Security 12% |
Kubernetes Cluster Maintenance 11% |
Kubernetes Logging / Monitoring 5% |
Kubernetes Storage 7% |
Kubernetes Troubleshooting 10% |



Table | Links
------- | ----
UNDERSTAND PODS | https://kubernetes.io/docs/concepts/workloads/pods/pod/
UNDERSTAND PODS | https://kubernetes.io/docs/concepts/workloads/pods/init-containers/
UNDERSTAND AND MANAGING CONTAINER STATE USING DEPLOYMENT | 
Replica Sets  | https://kubernetes.io/docs/concepts/workloads/controllers/replicaset/
ReplicationController | https://kubernetes.io/docs/concepts/workloads/controllers/replicationcontroller/
Daemon Sets | https://kubernetes.io/docs/concepts/workloads/controllers/daemonset/
Rolling Updates and Rollback | https://kubernetes.io/docs/concepts/workloads/controllers/statefulset/
StatefulSets | https://kubernetes.io/docs/concepts/workloads/controllers/statefulset/
Jobs and Cronjobs | https://kubernetes.io/docs/concepts/workloads/controllers/jobs-run-to-completion/
Jobs and Cronjobs | https://kubernetes.io/docs/concepts/workloads/controllers/cron-jobs/
DEEP UNDERSTANDING ABOUT SERVICES | 
Services |  https://kubernetes.io/docs/concepts/services-networking/service/
Services |  https://kubernetes.io/docs/concepts/services-networking/connect-applications-service/
DNS for Services and Pods

https://kubernetes.io/docs/concepts/services-networking/dns-pod-service/

Ingress
https://kubernetes.io/docs/concepts/services-networking/ingress/


DETAILED UNDERSTANDING ABOUT VOLUMES
Volumes and Persistent Volumes

https://kubernetes.io/docs/concepts/storage/volumes/
https://kubernetes.io/docs/concepts/storage/persistent-volumes/


Config Maps
POD SCHEDULING
Assigning Pods to Nodes

https://kubernetes.io/docs/concepts/configuration/assign-pod-node/


Taints and Tolerations

https://kubernetes.io/docs/concepts/configuration/taint-and-toleration/


Secrets

https://kubernetes.io/docs/concepts/configuration/secret/


Organising Cluster Access using kubeconfig files
https://kubernetes.io/docs/concepts/configuration/organize-cluster-access-kubeconfig/


Schedular
https://kubernetes.io/docs/concepts/configuration/scheduler-perf-tuning/


LOGGING ARCHITECTURE
https://kubernetes.io/docs/concepts/cluster-administration/logging/


MOST IMPORTANT – LOTS OF PRACTICE AND….
kubectl Cheat Sheet
https://kubernetes.io/docs/reference/kubectl/cheatsheet/























Service = Provides network acces to a dynamically chnaging group of pods

Conrainerd = container run time



etcd = provides distributed synchronized data storage about the cluster stage

kube-apiserver = servers the k8s api ,primary interface for the cluster

kube-controller-manager = Bundles several components into one package.

Kube-scheduler = Bundles serveral components into one package

kubelet = Agent that handles/executes running containers on a Node

kube-proxy = handles network communication between nodes by adding firewall routing rules.

Container run time = Software used to run containers on a system

Pod = A group of one or more containers in Kubernetes that shares storage and an IP address in a Kubernetes cluster

Microservices = Small, independent services that work together to make up an application

Monolith = One large executable that contains all parts of the application

Kube Master = Server that controls the kubernetes cluster


Deployments = way to automate the management of your pods. A deployment allows you to specify a desired state a set of pods.(Scaling, Rolling updates,Self healing)


services = allows you to dynamically access a group of replica pods. So a service creates an abstration layer on top of a set of replica pods. we can access the service rather than accessing the pods directly .




# kube-master 



curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -

``
sudo add-apt-repository \
   "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
   $(lsb_release -cs) \
   stable"
``

sudo apt-get install lsb-core lsb

sudo apt-get install curl wget software-properties-common nmap netstat

sudo apt-get update

sudo apt-get install -y docker-ce=18.06.1~ce~3-0~ubuntu



sudo apt-mark hold docker-ce

sudo docker version

curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key add -

cat << EOF | sudo tee /etc/apt/sources.list.d/kubernetes.list
deb https://apt.kubernetes.io/ kubernetes-xenial main
EOF

sudo apt-get update

sudo apt-get install -y kubelet=1.12.7-00 kubeadm=1.12.7-00 kubectl=1.12.7-00

sudo apt-mark hold kubelet kubeadm kubectl


sudo kubeadm init --pod-network-cidr=10.244.0.0/16

mkdir -p $HOME/.kube

sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config

sudo chown $(id -u):$(id -g) $HOME/.kube/config

kubectl version (if success full configured clientversion and serverion version are available)


The kubeadm init command should output a kubeadm join command containing a token and hash. Copy that command and run it with sudo on both worker nodes

sudo kubeadm join $some_ip:6443 --token $some_token --discovery-token-ca-cert-hash $some_hash

kubectl get nodes


On all three nodes, run the following :

echo "net.bridge.bridge-nf-call-iptables=1" | sudo tee -a /etc/sysctl.conf

sudo sysctl -p

Install Flannel in the cluster by running this only on the Master node:

kubectl apply -f https://raw.githubusercontent.com/coreos/flannel/bc79dd1505b0c8681ece4de4c0d86c5cd2643275/Documentation/kube-flannel.yml

kubectl get nodes (you can see all 3 nodes are on Status -ready state )

kubectl get pods -n kube-system (All the backend pods run on the kube-system namespace,  three pods with flannel in the name, and all three should have a status of Running.)


```
cat << EOF | kubectl create -f -
apiVersion: v1
kind: Pod
metadata:
  name: nginx
spec:
  containers:
  - name: nginx
    image: nginx
EOF

```

kubectl get pods

kubectl describe pod nginx

kubectl delete pod nginx


kubectl get nodes

kubectl describe nodes



Checking pod to pod communication


Create a deployment with two nginx pods:

```
cat << EOF | kubectl create -f -
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx
  labels:
    app: nginx
spec:
  replicas: 2
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:1.15.4
        ports:
  
```


Create a busybox pod

```
cat << EOF | kubectl create -f -
apiVersion: v1
kind: Pod
metadata:
  name: busybox
spec:
  containers:
  - name: busybox
    image: radial/busyboxplus:curl
    args:
    - sleep
    - "1000"
EOF

```


Get the IP addresses of your pods

kubectl get pods -o wide


Get the IP address of one of the nginx pods, then contact that nginx pod from the busybox pod using the nginx pod's IP address:

kubectl exec busybox -- curl $nginx_pod_ip





Get a list of system pods running in the cluster:


kubectl get pods -n kube-system


Check the status of the kubelet service:

sudo systemctl status kubelet





Deployment example :

```
cat <<EOF | kubectl create -f -
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
  labels:
    app: nginx
spec:
  replicas: 2
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:1.15.4
        ports:
        - containerPort: 80
EOF

```


Get a list of deployments:   kubectl get deployments

Get more information about a deployment: kubectl describe deployment nginx-deployment

Get a list of pods: kubectl get pods










Kubernetes Services :

Create a NodePort service on top of your nginx pods

```

cat << EOF | kubectl create -f -
kind: Service
apiVersion: v1
metadata:
  name: nginx-service
spec:
  selector:
    app: nginx
  ports:
  - protocol: TCP
    port: 80
    targetPort: 80
    nodePort: 30080
  type: NodePort
EOF

```
![image](https://user-images.githubusercontent.com/33985509/74418339-8689a180-4e48-11ea-9f1a-ab8145019014.png)

Get a list of services in the cluster: kubectl get svc or kubectl get services

Since this is a NodePort service, you should be able to access it using port 30080 on any of your cluster's servers. You can test this with the command:  curl localhost:30080



























# kube-node1



curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -

``
sudo add-apt-repository \
   "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
   $(lsb_release -cs) \
   stable"
``



sudo apt-get install lsb-core lsb

sudo apt-get install curl wget software-properties-common nmap netstat

sudo apt-get update

sudo apt-get install -y docker-ce=18.06.1~ce~3-0~ubuntu

sudo apt-mark hold docker-ce

sudo docker version

curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key add -

cat << EOF | sudo tee /etc/apt/sources.list.d/kubernetes.list
deb https://apt.kubernetes.io/ kubernetes-xenial main
EOF

sudo apt-get update

sudo apt-get install -y kubelet=1.12.7-00 kubeadm=1.12.7-00 kubectl=1.12.7-00

sudo apt-mark hold kubelet kubeadm kubectl








# kube-node2




curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -

``
sudo add-apt-repository \
   "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
   $(lsb_release -cs) \
   stable"
``


sudo apt-get install lsb-core lsb

sudo apt-get install curl wget software-properties-common nmap netstat


sudo apt-get update

sudo apt-get install -y docker-ce=18.06.1~ce~3-0~ubuntu

sudo apt-mark hold docker-ce

sudo docker version

curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key add -

cat << EOF | sudo tee /etc/apt/sources.list.d/kubernetes.list
deb https://apt.kubernetes.io/ kubernetes-xenial main
EOF

sudo apt-get update

sudo apt-get install -y kubelet=1.12.7-00 kubeadm=1.12.7-00 kubectl=1.12.7-00

sudo apt-mark hold kubelet kubeadm kubectl




























--------------------------------------------------------------------------------------------------------------------------------------------




https://www.katacoda.com/courses/ubuntu/playground


    1  apt update
    
    2  clear
    
    3  sudo apt-get update && apt-get install -y apt-transport-https
    
    4  clear
    
    5  sudo curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | apt-key add
    
    6  deb http://apt.kubernetes.io/ kubernetes-xenial main
    
    7  apt intsall deb
    
    8  apt install deb
    
    9  apt install deb3
    
   10  deb http://apt.kubernetes.io/ kubernetes-xenial main
   
   11  apt install wget
   
   12  apt install curl
   
   13  clear
   
   14  ls
   
   15  apt update
   
   16  clear
   
   17  ls
   
   18  apt install vim
   
   19  apt install nano -y
   
   20  clear
   
   21  vi /etc/apt/sources.list.d/kubernetes.list
   
   22  sudo apt-get update
   
   23  apt install apt-utils
   
   24  apt install net-tools
   
   25  apt install nmap -y
   
   26  apt-get install -y kubelet kubeadm kubectl kubernetes-cni
   
   27  sudo swapoff -a
   
   28  clear
   
   29  cat /etc/fstab
   
   30  sudo kubeadm init
   
   31  mkdir -p $HOME/.kube
   
   32  sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
   
   33  sudo chown $(id -u):$(id -g) $HOME/.kube/config
   
   34  apt install sudo
   
   35  sudo kubectl apply -f https://raw.githubusercontent.com/coreos/flannel/master/Documentation/kube-flannel.yml
   
   36  sudo kubectl apply -f https://raw.githubusercontent.com/coreos/flannel/master/Documentation/k8s-manifests/kube-flannel-rbac.yml
   
   37  sudo kubectl get pods --all-namespaces
   
   38  hostname -I
   
   39  apt install netstat
   
   40  apt update
   
   41  netstat -ntulp | grep LISTEN
   
   42  cat /etc/hosts
   
   43  vi /etc/hosts
   
   44  clear
   
   45  ls
   
   46  apt update
   
   47  history
   
   48 sudo kubeadm join --token TOKEN MASTER_IP:6443
   
   49 Kubectl get nodes
   
   
   
   
   
   
   
   Sample Microservice example
   
   cd ~/
git clone https://github.com/linuxacademy/robot-shop.git


Create a namespace and deploy the application objects to the namespace using the deployment descriptors from the Git repository:

kubectl create namespace robot-shop

kubectl -n robot-shop create -f ~/robot-shop/K8s/descriptors/

Get a list of the application's pods and wait for all of them to finish starting up:

kubectl get pods -n robot-shop -w   


Once all the pods are up, you can access the application in a browser using the public IP of one of your Kubernetes servers and port 30080:

http://$kube_server_public_ip:30080
