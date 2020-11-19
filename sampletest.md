# kubernetes





![image](https://user-images.githubusercontent.com/33985509/74141432-2e0f9580-4bf7-11ea-8816-990ce03e7e77.png)



https://www.katacoda.com/courses/kubernetes



https://www.katacoda.com/courses/kubernetes/launch-single-node-cluster


![image](https://user-images.githubusercontent.com/33985509/74138916-b6d80280-4bf2-11ea-9846-80546257bbcd.png)


![image](https://user-images.githubusercontent.com/33985509/74139086-08808d00-4bf3-11ea-9d37-1d7e926173f9.png)

![image](https://user-images.githubusercontent.com/33985509/74139112-159d7c00-4bf3-11ea-8dfb-d67c5a0181e4.png)


![image](https://user-images.githubusercontent.com/33985509/74141954-2c929d00-4bf8-11ea-8c7f-7978562cdce1.png)



![image](https://user-images.githubusercontent.com/33985509/74142047-606dc280-4bf8-11ea-9865-51b224a68062.png)


![image](https://user-images.githubusercontent.com/33985509/74142353-f99cd900-4bf8-11ea-86fb-04c3f074a564.png)


![image](/uploads/0efb3be28788c4710ad0032f6ae18a8f/image.png)

![image](https://user-images.githubusercontent.com/33985509/74143463-2225d280-4bfb-11ea-8481-c1521c658c3c.png)


![image](https://user-images.githubusercontent.com/33985509/74145335-d2e1a100-4bfe-11ea-863a-499016a0d2f3.png)


![image](https://user-images.githubusercontent.com/33985509/74143535-471a4580-4bfb-11ea-9fcb-a8343b259c25.png)

![image](https://user-images.githubusercontent.com/33985509/74144159-8301da80-4bfc-11ea-9a58-70cf6a597657.png)


![image](https://user-images.githubusercontent.com/33985509/74144533-38cd2900-4bfd-11ea-8c78-be8e4c9efe69.png)



```
cat <<EOF | kubectl create -f - 
apiVersion: v1
kind: ServiceAccount
metadata:
  name: admin-user
  namespace: kube-system
---
apiVersion: rbac.authorization.k8s.io/v1beta1
kind: ClusterRoleBinding
metadata:
  name: admin-user
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: ClusterRole
  name: cluster-admin
subjects:
- kind: ServiceAccount
  name: admin-user
  namespace: kube-system
EOF

```



 https://2886795302-8443-elsy04.environments.katacoda.com/
 
 
 ![image](https://user-images.githubusercontent.com/33985509/74144895-dde80180-4bfd-11ea-82bb-6be1f840df1e.png)



-------------------------------------------------------------------------------------------------------------------------------------

Using Kubectl

![image](https://user-images.githubusercontent.com/33985509/74148865-bd707500-4c06-11ea-80d1-f769d64ae7c6.png)

![image](https://user-images.githubusercontent.com/33985509/74148873-bfd2cf00-4c06-11ea-9e52-2517d25e5de3.png)

use kubectl to create a service which exposes the Pods on a particular port.

command to expose the container port 80 on the host 8000 binding to the external-ip of the host

kubectl get svc

docker ps | grep httpexposed

![image](https://user-images.githubusercontent.com/33985509/74150712-c8c59f80-4c0a-11ea-8c70-1949f7d4001f.png)



Deploy containers using YAML :


deployment.yaml

```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: webapp1
spec:
  replicas: 1
  selector:
    matchLabels:
      app: webapp1
  template:
    metadata:
      labels:
        app: webapp1
    spec:
      containers:
      - name: webapp1
        image: katacoda/docker-http-server:latest
        ports:
        - containerPort: 80
        
```



kubectl create -f deployment.yaml

kubectl get deployment

kubectl describe deployment webapp1

![image](https://user-images.githubusercontent.com/33985509/74151397-68cff880-4c0c-11ea-90f4-b5b56a6c1e4a.png)


The Service selects all applications with the label webapp1. As multiple replicas, or instances, are deployed, 

they will be automatically load balanced based on this common label.

The Service makes the application available via a NodePort


service.yaml

```
apiVersion: v1
kind: Service
metadata:
  name: webapp1-svc
  labels:
    app: webapp1
spec:
  type: NodePort
  ports:
  - port: 80
    nodePort: 30080
  selector:
    app: webapp1

```

kubectl create -f service.yaml

kubectl get svc

kubectl describe svc webapp1-svc

curl host01:30080

![image](https://user-images.githubusercontent.com/33985509/74152543-ce24e900-4c0e-11ea-9ec3-00ec1b18fb75.png)


-----------------------------------------------------------------------------------------------------------------------------------


















sample on docker



docker run -itd --name k8s-master -h k8s-master --privileged ubuntu:18.04 (dropped)

docker run -itd --name systemd --security-opt seccomp=unconfined --tmpfs /run --tmpfs /run/lock -v /sys/fs/cgroup:/sys/fs/cgroup:ro -t solita/ubuntu-systemd:18.04

docker run -itd --name worker01 -h worker01 ubuntu:18.04

docker run -itd --name worker02 -h worker02 ubuntu:18.04


![image](https://user-images.githubusercontent.com/33985509/73249565-6b1a6780-41b5-11ea-9244-e7b44af68a3f.png)

sudo vim /etc/hosts

```
172.17.0.7      k8s-master
172.17.0.5      worker01
172.17.0.6      worker02
```

ping -c 3 k8s-master

ping -c 3 worker01

ping -c 3 worker02

apt update

apt install sudo vim apt-utils net-utils nmap

```
sudo apt -y install \
  gcc \
  make \
  cmake \
  git \
  btrfs-progs \
  golang-go \
  go-md2man \
  iptables \
  libassuan-dev \
  libc6-dev \
  libdevmapper-dev \
  libglib2.0-dev \
  libgpgme-dev \
  libgpg-error-dev \
  libostree-dev \
  libprotobuf-dev \
  libprotobuf-c-dev \
  libseccomp-dev \
  libselinux1-dev \
  libsystemd-dev \
  pkg-config \
  runc \
  uidmap \
  libapparmor-dev

```

sudo curl -sSL https://get.docker.com/ | sh

service docker start (it won't start properly)

sudo dockerd

service docker start

service docker stop

service docker restart

service docker status


or 

```
# Install Docker CE
## Set up the repository:
### Install packages to allow apt to use a repository over HTTPS
apt-get update && apt-get install \
apt-transport-https ca-certificates curl software-properties-common

### Add Docker’s official GPG key
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | apt-key add -

### Add Docker apt repository.
add-apt-repository \
  "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
  $(lsb_release -cs) \
  stable"

## Install Docker CE.
apt-get update && apt-get install \
  containerd.io=1.2.10-3 \
  docker-ce=5:19.03.4~3-0~ubuntu-$(lsb_release -cs) \
  docker-ce-cli=5:19.03.4~3-0~ubuntu-$(lsb_release -cs)

# Setup daemon.
 /etc/docker/daemon.json 
{
  "exec-opts": ["native.cgroupdriver=systemd"],
  "log-driver": "json-file",
  "log-opts": {
    "max-size": "100m"
  },
  "storage-driver": "overlay2"  (changed to aufs)
}


mkdir -p /etc/systemd/system/docker.service.d

# Restart docker.
systemctl daemon-reload
systemctl restart docker
```


sudo swapon -s

sudo swapoff -a

sudo apt install -y apt-transport-https

curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key add -

cd /etc/apt/

sudo vim sources.list.d/kubernetes.list

deb http://apt.kubernetes.io/ kubernetes-xenial main

sudo apt update

sudo apt install -y kubeadm kubelet kubectl


Error

![image](https://user-images.githubusercontent.com/33985509/73250497-458e5d80-41b7-11ea-9904-e1ac45bdb359.png)


verified docker was not running

restarted docker service

again some errors exist


apt-get install --reinstall linux-image-`uname -r`




sudo kubeadm init --pod-network-cidr=10.244.10.0/16 --apiserver-advertise-address=172.17.0.12 --kubernetes-version "1.11.0"


```
root@k8s-master:~# kubeadm init

W0128 09:51:02.749244    6733 validation.go:28] Cannot validate kubelet config - no validator is available

W0128 09:51:02.749283    6733 validation.go:28] Cannot validate kube-proxy config - no validator is available

[init] Using Kubernetes version: v1.17.2

[preflight] Running pre-flight checks

[WARNING Firewalld]: no supported init system detected, skipping checking for services
        
[WARNING Service-Docker]: no supported init system detected, skipping checking for services
        
 [WARNING IsDockerSystemdCheck]: detected "cgroupfs" as the Docker cgroup driver. The recommended driver is "systemd". Please follow the guide at https://kubernetes.io/docs/setup/cri/
 
[WARNING Service-Kubelet]: no supported init system detected, skipping checking for services

[preflight] Pulling images required for setting up a Kubernetes cluster

[preflight] This might take a minute or two, depending on the speed of your internet connection

[preflight] You can also perform this action in beforehand using 'kubeadm config images pull'

[kubelet-start] no supported init system detected, won't make sure the kubelet not running for a short period of time while setting up configuration for it.

W0128 09:51:33.160618    6733 flags.go:110] cannot determine if systemd-resolved is active: no supported init system detected, skipping checking for services

[kubelet-start] Writing kubelet environment file with flags to file "/var/lib/kubelet/kubeadm-flags.env"

[kubelet-start] Writing kubelet configuration to file "/var/lib/kubelet/config.yaml"

[kubelet-start] Starting the kubelet

[kubelet-start] no supported init system detected, won't make sure the kubelet is running properly.

[certs] Using certificateDir folder "/etc/kubernetes/pki"

[certs] Generating "ca" certificate and key

[certs] Generating "apiserver" certificate and key

[certs] apiserver serving cert is signed for DNS names [k8s-master kubernetes kubernetes.default kubernetes.default.svc 
kubernetes.default.svc.cluster.local] and IPs [10.96.0.1 172.17.0.7]

[certs] Generating "apiserver-kubelet-client" certificate and key

[certs] Generating "front-proxy-ca" certificate and key

[certs] Generating "front-proxy-client" certificate and key

[certs] Generating "etcd/ca" certificate and key

[certs] Generating "etcd/server" certificate and key

[certs] etcd/server serving cert is signed for DNS names [k8s-master localhost] and IPs [172.17.0.7 127.0.0.1 ::1]

[certs] Generating "etcd/peer" certificate and key

[certs] etcd/peer serving cert is signed for DNS names [k8s-master localhost] and IPs [172.17.0.7 127.0.0.1 ::1]

[certs] Generating "etcd/healthcheck-client" certificate and key

[certs] Generating "apiserver-etcd-client" certificate and key

[certs] Generating "sa" key and public key

[kubeconfig] Using kubeconfig folder "/etc/kubernetes"

[kubeconfig] Writing "admin.conf" kubeconfig file

[kubeconfig] Writing "kubelet.conf" kubeconfig file

[kubeconfig] Writing "controller-manager.conf" kubeconfig file

[kubeconfig] Writing "scheduler.conf" kubeconfig file

[control-plane] Using manifest folder "/etc/kubernetes/manifests"

[control-plane] Creating static Pod manifest for "kube-apiserver"

[control-plane] Creating static Pod manifest for "kube-controller-manager"

W0128 09:51:38.032376    6733 manifests.go:214] the default kube-apiserver authorization-mode is "Node,RBAC"; using "Node,RBAC"
[control-plane] Creating static Pod manifest for "kube-scheduler"

W0128 09:51:38.033544    6733 manifests.go:214] the default kube-apiserver authorization-mode is "Node,RBAC"; using "Node,RBAC"
[etcd] Creating static Pod manifest for local etcd in "/etc/kubernetes/manifests"

[wait-control-plane] Waiting for the kubelet to boot up the control plane as static Pods from directory "/etc/kubernetes/manifests". This can take up to 4m0s

[kubelet-check] Initial timeout of 40s passed.

[kubelet-check] It seems like the kubelet isn't running or healthy.

[kubelet-check] The HTTP call equal to 'curl -sSL http://localhost:10248/healthz' failed with error: Get http://localhost:10248/healthz: dial tcp 127.0.0.1:10248: connect: connection refused.

[kubelet-check] It seems like the kubelet isn't running or healthy.

[kubelet-check] The HTTP call equal to 'curl -sSL http://localhost:10248/healthz' failed with error: Get http://localhost:10248/healthz: dial tcp 127.0.0.1:10248: connect: connection refused.

[kubelet-check] It seems like the kubelet isn't running or healthy.

[kubelet-check] The HTTP call equal to 'curl -sSL http://localhost:10248/healthz' failed with error: Get http://localhost:10248/healthz: dial tcp 127.0.0.1:10248: connect: connection refused.

[kubelet-check] It seems like the kubelet isn't running or healthy.

[kubelet-check] The HTTP call equal to 'curl -sSL http://localhost:10248/healthz' failed with error: Get http://localhost:10248/healthz: dial tcp 127.0.0.1:10248: connect: connection refused.

[kubelet-check] It seems like the kubelet isn't running or healthy.

[kubelet-check] The HTTP call equal to 'curl -sSL http://localhost:10248/healthz' failed with error: Get http://localhost:10248/healthz: dial tcp 127.0.0.1:10248: connect: connection refused.

Unfortunately, an error has occurred:timed out waiting for the condition

This error is likely caused by:
 - The kubelet is not running
 - The kubelet is unhealthy due to a misconfiguration of the node in some way (required cgroups disabled)

If you are on a systemd-powered system, you can try to troubleshoot the error with the following commands:
 - 'systemctl status kubelet'
 - 'journalctl -xeu kubelet'

Additionally, a control plane component may have crashed or exited when started by the container runtime.

To troubleshoot, list all containers using your preferred container runtimes CLI, e.g. docker.

Here is one example how you may list all Kubernetes containers running in docker:

- 'docker ps -a | grep kube | grep -v pause'


Once you have found the failing container, you can inspect its logs with:

- 'docker logs CONTAINERID'

error execution phase wait-control-plane: couldn't initialize a Kubernetes cluster

To see the stack trace of this error execute with --v=5 or higher

```



The br_netfilter module is required for kubernetes installation. 

Enable this kernel module so that the packets traversing the bridge are processed by iptables for filtering and for port forwarding, 

and the kubernetes pods across the cluster can communicate with each other.

Run the command below to enable the br_netfilter kernel module.

modprobe br_netfilter

echo '1' > /proc/sys/net/bridge/bridge-nf-call-iptables  

or

nano /proc/sys/net/ipv4/ip_forward

you should see 0 delete 0 and write 1


![image](https://user-images.githubusercontent.com/33985509/73263598-34524a80-41d1-11ea-9bf1-42d222ab23d5.png)

```
[ERROR SystemVerification]: 
failed to parse kernel config: 
unable to load kernel module: "configs", output: "modprobe: 
ERROR: ../libkmod/libkmod.c:586 kmod_search_moddep() could not open moddep file '/lib/modules/5.0.0-37-generic/modules.dep.bin'\nmodprobe: FATAL: Module configs not found in directory /lib/modules/5.0.0-37-generic\n", 
err: exit status 1

```

apt-cache search linux-headers-4

apt-get install linux-headers-$(uname -r)

apt-get install linux-headers

sudo apt-get install libpam-cracklib

apt-get install systemd libpam-systemd systemd-ui


apt-get install 
conntrack
cri-tools
etables
ethtool
iproute2
kmod
kubectl 
kubelet
kubernetes-cni
libatml




kubeadm init


mkdir -p $HOME/.kube

sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config

sudo chown $(id -u):$(id -g) $HOME/.kube/config


kubectl version

systemd-resolve --status | cat

apt install lsof


Cannot validate kube-proxy config - no validator is available

Cannot validate kubelet config - no validator is available

kubeadm token generate

qc54me.ywkkclhfw2vy7q02


kubeadm join 172.17.0.12:6443 --token qc54me.ywkkclhfw2vy7q02 --discovery-token-unsafe-skip-ca-verification


Error : 
The connection to the server 172.17.0.12:6443 was refused - did you specify the right host or port?

