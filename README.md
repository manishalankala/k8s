# k8s


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
