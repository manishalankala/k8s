# k8s


Service = Provides network acces to a dynamically chnaging group of pods

Conrainerd = container run time

kubelet = Agent that handles running containetes on a Node

etcd = provides distributed data storage about the cluster stage

Container run time = Software used to run containers on a system

Pod = A group of one or more containers in Kubernetes that shares storage and an IP address in a Kubernetes cluster

Microservices = Small, independent services that work together to make up an application

Monolith = One large executable that contains all parts of the application

Kube Master = Server that controls the kubernetes cluster





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
   
