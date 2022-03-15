-  ```    ```

- Cluster -   ``` A logical grouping of all components within a cluster.```

- Namespace -  ``` A named logical grouping of Kubernetes components within a cluster.Used to Isolate different workloads on the same cluster.   ```


- Node
```
A virtual machine or underlying server. There are two types of nodes: Control Plane and Worker nodes.
Worker nodes is where your application or workloads run. Control Plane nodes manage worker nodes.
```

- Pod -  ``` The smallest unit in K8s. It is an abstraction over a container.Generally defines an application workload.```

- Service
```
A static IP address and DNS name for a set of pods (persists an address even if a pod dies) and a load balancer
A "service" can also mean a container that continuously runs.
```

- Ingress -  ``` Translates HTTP/S rules to point to services ```





- API Server -  ``` The API Server allows users to interact with K8s components using the KubeCTL or by sending HTTP requests.   ```

- Kubelet -  ``` Kubeletis an agent installed on all nodes. Kublelet allows users to interact with node via the API Server and KubeCTL.   ```

- KubeCTL -  ``` A command line interface (CLI) that allows users to interact with the cluster and components via the API Server  ```

- Cloud Controller Manager -  ``` Allows you to link a Cloud Service Provider (CSP) eg. AWS, Azure GCP to leverage cloud services.   ```

- Controller Manager -  ``` A control loop that watches the state of the cluster and will change the current state back to desired state.   ```

- Scheduler -  ``` Determines where to place pods on nodes. Places them in a scheduling a queue.   ```

- Kube Proxy -  ``` An application on worker nodes that provides routing and filtering rules for ingress (incoming) traffic to pods.  ```


- ConfigMap
```
allows you to decouple environment-specific configuration from your container images, so that
your applications are easily portable. Used to store non-confidential data in key-value pair
```

- Secret -  ```  small amount of sensitive data such as a password, a token, or a key  ```


- Volumes -  ``` mounting storage eg. locally on the node, or remote to cloud storage   ```


- StatefulSet
```
provides guarantees about the ordering and uniqueness of these Pods
Think of databases where you have to determine read and write order or limit the amount of containers
StatefulSets are hard, when you can host your db externally from K8s cluster
```

- ReplicaSets -  ``` Maintain a stable set of replica pods running at a given time. Can provide a guarantee of availability.   ```


- Deployment -  ``` Is a blueprint for a pod (think Launch Template)  ```



- Manifest
```
A Manifest file is a document that is commonly used for customs to list the contents of
cargo, or passengers. Its an itemized list of things.

Manifest file is a generalized name for any Kubernetes Configuration File that define the
configuration of various K8s components.

all Manifest files with specific purposes:Deployment File,PodSpec File,Network Policy File
Files can be written in either: yaml or json

A manifest can contain multiple K8s component definitions/configurations.

Resource Configuration file is sometimes used to describe multiple
resources in a manifest.

```


- Control Plane 

```
API Server- the backbone of communication
Scheduler – determines where to start a pod on worker node
Controller Manager - detect state changes (if pod crashes,
etcd - A Key/Value Store that stores the state of the cluster
Kubelet – Allows user to interact with the node via KubeCTL
```

- Worker Nodes

```
Kubelet
Kube Proxy
Container Runtime
Pods and Containers
```

Pods
```
Pods are the smallest unit in Kubernetes.
Pods abstract away the container layer so you can directly interact with the Kubernetes Layer.

A Pod is intended to run one application in multiple containers Database Pod, Job Pod, Frontend App Pod, Backend App Pod
You can run multiple apps in a pod but those containers will tightly dependent.
Each Pod gets it own private IP address Containers will run on different ports Containers can talk to each other via localhost

Each Pod can have a shared storage volume attached. All containers will share the same volume
When the last remaining container dies (maybe crashes) in a pod so does the pod When a replacement pod is created, the pod will have an IP address will be assigned.
IP addresses are Ephemeral, (temporary) for pods, they don't by default persist.
```

Kubernetes api server
```
The core of Kubernetes control plane is the API server

The API server exposes an HTTP API that lets end users, different parts of your cluster, and external components communicate with one another.

The Kubernetes API lets you query and manipulate the state of API objects in Kubernetes (for example: Pods, Namespaces, ConfigMaps, and Events).

The API server is a component of the Kubernetes control plane that exposes the Kubernetes API. 

The API server is the front end for the Kubernetes control plane.

The main implementation of a Kubernetes API server is kube-apiserver.

kube-apiserver is designed to scale horizontally-that is, it scales by deploying more instances.You can run several instances of kube-apiserver and balance traffic between those instances.Everything has to go through the API Server.

You can interact with the API Server in three ways: UI, API, CLI KubeCTL

```


Deployment
```
A Deployment provides declarative updates for Pods and ReplicaSets.

A Deployment Controller changes the actual state to the desired state at a controlled rate.

The default Deployment Controller can be swapped out for other deployments tools eg: Argo CD, Flux, Jenkin X...

A Deployment define the desired state of ReplicaSets and Pods.

A deployment will create and manage a ReplicaSet.

A ReplicaSet will manage replicas of pod.

```

ReplicaSet
```
ReplicaSet is a way to maintain a desired amount of redundant pods (replicas) to provide a guarantee of availability.

The pod field metadata.ownerReferences determines The link from a pod to a ReplicaSet.

```

Stateful Sets
```
Stateful Sets are used when you need traffic to be sent to a specific pods.

Stateful Set will always have:
- a unique and predictable name and address
- ordinal index number assigned each pod
- a persistent volume attached, with a persistent link from pod to the storage
If a pod is rescheduled the original Persistent Volume (PV) will be
mounted to ensure data integrity and consistency.
- Stateful Set pods will always start in the same order, and terminate in
reverse order

StatefulSets currently require a "headless" service to manage the identities

There is a headless service used to maintain the network identity pods, and another service that provides read access to the pods


DNS Hostname for Writes:
Writes will be directed to the Main pod by its DNS Hostname which is identified by a Headless Service

ClusterIP for Reads:
For read traffic it can be distributed to all reading pods using a ClusterIP Service

Headless Service:
The headless service is a Service with ClusterIP set to none.
It does not provide load balancing
It does not provide a static IP address
A Headless Service is used to identify specific pods by assigning them DNS record.
A Headless Service is required in order for a StatefulSet to work.

PVC and PV:
Each pod has its own volume. A Persistent Volume Claim can dynamically reference a Persistent Volume.

```


Namespace
```
A Namespace is a way to logically isolate resources within a Kubernetes Cluster.
Namespaces can be organized based on project, department or any user-defined grouping.

Kubernetes starts 4 initial namespaces

1. default:   The default namespace where all our pods and services run unless a namespace is specified.
2. kube-public:  For resources that are publicly visible and readable.
3. kube-system: The system namespace that stores objects created by the Kubernetes System.Engineers deploying applications are not supposed to touch this namespace.
4. kube-node-lease: Holds lease objects associated with each node. Used to detect node failures by sending heartbeats

kubectl get namespace

You can create your own namespaces : kubectl create namespace dev

Names of resources need to be unique within a namespace, but not across namespaces.

Namespace-based scoping is applicable only for namespaced objects eg. Deployments, Services
But not for cluster-wide objects eg. StorageClass, Nodes, PersistentVolumes


Single-Namespace Objects : ConfigMaps and Secrets can't be shared across namespaces

Multi-Namespace Objects: A Service and Pods can belong to multiple namespaces

Cluster-Wide Objects: Volumes and Nodes cannot be bound to namespace since they are global

You can apply system quota restrictions on a namespace to avoid overuse eg Mem, Compute

If you don't provide a namespace for a component it will end up in the default namespace
```


Endpoints
```
Endpoints track the IP Addresses of the pods assigned to a Kubernetes Service.
When a service selector matches a pod label, The pod IP Address is added to the pool of endpoints
Pods expose themselves to services via endpoints.

kubectl get endpoints

Endpoint Slices break up Endpoints into smaller manageable segments. Each Endpoint Slice has a limit of 100 pods.
```


```
Selectors are a way of selecting one or more Kubernetes

there are 3 types of selectors:
Label Selector : Select K8s objects based on the applied label example env: prod
Field Selector : Select K8s objects based on object data eg. Metadata, Status
Node Selector  : Select nodes for very specific pod placement

kubectl get pods --show-labels

kubectl label pods apache-web owner=devops
```

Recommended labels
```
app.kubernetes.io/name
The name of the application
app.kubernetes.io/instance
A unique name identifying the instance of an application
app.kubernetes.io/version
The current version of the application (e.g., a semantic version,
revision hash, etc.)
app.kubernetes.io/component
The component within the architecture
app.kubernetes.io/part-of
The name of a higher level application this one is part of
app.kubernetes.io/managed-by
The tool being used to manage the operation of an application
app.kubernetes.io/created-by
The controller/user who created this resource
```

Label selector 
```
K8 objects like Services and ReplicaSets will target pods based on label selectors

You can use selector in the KubeCTL command line with --selector or it alias -I

kubectl get pods --selector env=development
kubectl get pods -l env=development
```

Annotations
```
Kubernetes annotations allows you to attach arbitrary non-identifying metadata to
Clients (eg. tools and libraries) can and my require this annotation in order to work.
```

Podspec
```
PodSpec is a configuration file that describes a pod.

The spec section will define
• multiple containers
• The name of the container
• The image
• The command to run on startup The port the container will operate on
• The restart policy

```

grpc
```
RPC is a framework of communication in distributed systems. It allows one program on machine to communication with
another program on remote machine without knowing its remote. The concept of RPC has been around since 1970s.

GRPC is a modern open source high performance Remote Procedure Call (RPC)
framework that can run in any environment. GRPC was initially created by Google.

GRPC can connect services in and across data centers with pluggable support for:
• load balancing
• tracing
• health checking
• authentication


GRPC works by:
• Installing a gRPC library for your program.
• Defining a Protobuf file that describe the data structure.
• Profobuf files have .proto exentsion
Writing code that works with gRPC

```


Kubelet
```
is responsible for pod internal API communication via the API Server.
Kubelet is a node agent that runs on every single node (control plane and workers).

The Kubelet perform the following tasks:
• watches for pod changes
• Configures the Container Runtime to
• Pull images
• Create namespaces
• Run containers

Kubelet uses PodSpec files to determine
what images to pull and containers to run.

Kubelet
will continuously monitor
Pods of any kinds of changes.

Kubelet
will
send back HTTPS
requests to API server containers
logs and execution requests

Kubelet
can interact with a Container
Runtime Interface (CRI) also using gPRC

Kubelet
can interact with
Storage
through the Container Storage
Interface (CSI) using GRPC

```


Kubectl
```
KubeCTL is a command line tool lets you control Kubernetes clusters

kubectl looks for a file named config in the $HOME/.kube directory

annotation - key value data that can be applied to resources
apply - Executes manifest files to create, modify K8 resources
auth - Inspect if you are authorized to perform an action
autoscale - Create an autoscaler that automatically chooses and sets the number of pods that run
cp - Copy files and directories to and from containers
create – Create specific K8 cluster-level resources: ConfigMap, Deployment, Job, Namespace, Role,
delete – Delete resources filenames, stdin, resources and names, or by resources and label select
describe – Show details of a specific resource or group of resources
diff – diffs the online configuration with local config
edit - Edit a resource from the default editor. Edit a deployed manifest file and apply the changes.
exec – Execute a command within a container
expose – Expose as resource as Kubernetes service
get – generally used to get the status of an existing Kubernetes resource
kustomize – Print a set of API resources generated from instructions in a kustomization.yaml file
label – update labels on a resource
logs – print the logs for a container in a pod or specific resource
patch – Update fields of a resource using strategic merge patch, a JSON merge patch, or a JSON patch
port-forward – Forward one or more local ports to a pod.
proxy – Creates a proxy server between localhost and the Kubernetes API Server
run – Create and run a container image in a pod
scale – Set a new size for a Deployment, ReplicaSet, Replication Controller, or StatefulSet

Flags
generally start with two hyphens. eg. -server
• Sometimes flags have abbreviations with single hypen eg. -s
• Available flags will vary based on command
• Sometimes flags can be assigned values or do not expect a value.


```



Resource types
```
Bindings
Componentstatuses (cs)
Configmaps (cm)
Endpoints (ep)
Events (ev)
Limitranges (limits)
Namespaces (ns)
Nodes (no)
Persistentvolumeclaims (pvc)
Persistentvolumes (pv)
Pods (po)
Podtemplates
Replicationcontrollers (rc)
Resourcequotas (quota)
Secrets
Serviceaccounts (sa)
Services (svc)
Mutatingwebhookconfigurations
Validatingwebhookconfigurations
Customresourcedefinitions
Apiservices
Controllerrevisions
Daemonsets (ds)
Deployments (deploy)
Replicasets (rs)
Statefulsets (sts)
Tokenreviews
Localsubjectaccessreviews
Selfsubjectaccessreviews
Selfsubjectrulesreviews
Subjectaccessreviews
Horizontalpodautoscalers
Cronjobs (cj)
Jobs
Certificatesigningrequests (csr)
Leases
Endpointslices
Events (ev)
Ingresses (ing)
Flowschemas
Prioritylevelconfigurations
Ingressclasses
```


minikube
```

Minikube
sets up a local single-node Kubernetes cluster
macOS, Linux, and Windows for learning purposes.

Supports the latest Kubernetes release
Cross-platform
Deploy as a VM, a container, or on bare-metal
• Multiple container runtimes
Docker API endpoint for blazing fast image pushes
Advanced features such as LoadBalancer, filesystem
mounts, and FeatureGates
Addons for easily installed Kubernetes applications
Supports common Cl environments

```


K3s

```
is a lightweight tool designed to run production-level Kubernetes workloads for
low-resourced and remotely located loT and Edge devices and Bare metal.
Originally Created by Rancher, a Sandbox CNCF Project
RANCHER


K3s
does not use kubelet, but it runs kubelet on the host machine and uses the host's
scheduling mechanism to run containers
K3s uses kube-proxy to proxy the network connections of the nodes.
K8s uses kube-proxy to proxy the network connections of an individual container.

k3s can have tighter security deployment than k8s because of their small attack surface area.


K3d
is a
platform-agnostic, lightweight wrapper that runs K3s in a docker container.
It helps run and scale single or multi-node K3S clusters quickly without further
setup while maintaining a high availability mode.
```

Management layers
```
A management layer for running Kubernetes on other platforms or allows you to extend your control plane to multiple platform

Weave Kubernetes Platform (WKP) - All of Weaves open-sources tools packaged as a platform so you can build out a GitOps enabled cluster.

Rafay - Similar to OpenShift with a larger focus on governance and GitOps-based management for any K8s clusters running on anything (including OpenShift)

VMWare Tanzu - Wherever vSphere runs, you can mange you can deploy and monitor Kubernetes clusters.

Azure Arc - multi-cluster-management governing compute such as K8s from other CSPS or on-premise or the edge.

Google Anthos - multi-cluster-management Is GKE being extended to mange clusters deployed to VMs on other cloud's or premise. Its focused on managing K8.

Platform9 -  Similar to RayFay but relies more on third-party tooling instead of trying to leverage native functionality from public cloud service providers.


Red Hat OpenShift Platform as a Service for K8s
• Openshift is Kubernetes with a commercial platform by RedHat built on-top.
• Kubectl is extended with additional functionality with the oc cli
• quickly deploy local code to a remote OpenShift cluster via odo
• A quality assurance pipeline built into the platform
• Fixing critical bugs earlier instead of waiting for next K8s release
• Using Redhat CoreOS (an operating system optimized for running containers)
• OperatorsHub, an automated installation tool (one click marketplace)
• Graphical Ul developer console
• CodeReady workspaces, Cloud Developer Environment for Kubernetes

Rancher Kubernetes Engine (RKE)
• Runs entirely within Docker containers.
• It works on bare-metal and virtualized servers.
• RKE solves the problem of installation complexity, a common issue in the Kubernetes community.
• Installation and operation of Kubernetes is both simplified and easily automated,
• It's entirely independent of the operating system and platform you're running.
• As long as you can run a supported version of Docker, you can deploy and run Kubernetes with RKE.

```




Container Runtime Interface (CRI)

```
Containerd

Images you build with Docker aren't really "Docker images".
• Docker now uses the containerd runtime
your images are built in the standardized Open Container
Initiative (OCI) format.

CRI-O

It
allows Kubernetes to use any OCl-compliant runtime as the container runtime for running pods.
Today it supports runc and Kata Containers as the container runtimes but any OCI-conformant
runtime can be plugged in principle.
```


```
What is a process?
A process is an instance of a running program on Linux
What is a CGroup?
Control groups (cgroups) allows you to group processes to apply different kinds of limitations:
Resource limiting
- groups can be set to not exceed a configured memory limit, which also includes the file system cache
Prioritization – some groups may get a larger share of CPU utilization or disk I/0 throughput
Accounting
- measures a group's resource usage, which may be used, for example, for billing purposes
Control
freezing groups of processes, their checkpointing and restarting

Think of CGroups as a way to limit programs
From overusing CPU, Memory or Storage.

The primary design goal for cgroups was to provide a unified interface to manage processes
or whole operating-system-level virtualization, including Linux Containers (LXC)

```



Backing Store and etcd
```
Components of a Kubernetes cluster include: pods, nodes, control plane, and
volumes, and each of these needs a level of protection in case of disaster
Kubernetes resources are stored in an etcd (but could be backed by MariaDB).
Application data is stored in persistent volumes for applications running on the cluster.

etcd is a strongly consistent, distributed key-value store that provides a reliable way to store data that needs to be accessed
by a distributed system or cluster of machines.

things used by : k8s cluster, coredns,Rook
```



Volumes
```
Kubernetes supports many types of volumes.
A Pod can use any number of volume types simultaneously

Persistent Volumes :  Attaching an external storage to a pod. The data will persist even if the pod is terminated.

Ephemeral Volumes : A volume that's only exists as long as the pod exists.Intended for temporary data storage.

Projected Volumes : maps several existing volume sources into the same directory.

Volume Snapshot : Archiving a volume configuration and its data for rollbacks or backup

Types of Volumes Supported:
awsElasticBlockStore
azureDisk
azureFile
cephfs
cinder
configMap
downwardAPI
emptyDir
fc (fibre channel)
gcePersistentDisk
glusterfs
hostPath
iscsi
local
nfs
persistentVolumeClaim
portworxVolume
projected
rbd
secret
vsphereVolume

```

Persistent Volume

```
A piece of storage in the cluster that has been provisioned by an administrator or dynamically provisioned using Storage Classes

PV is similar to node in that is a cluster resource. PV does not reside within a node or pod.

PVs are volume plugins like Volumes, but have a lifecycle independent of any individual Pod that uses the
This API object captures the details of the implementation of the
storage, be that NFS, ISCSI, or a cloud-provider-specific storage system

mounting pv directly to a pod is not allowed and is against the Kubernetes design principles.
It would cause tight coupling below the pod volume and the underlying storage.
Persistent Volume Claims ensures decoupling

```

```
A storage class is way of defining a class of storage that is backed by a provisioner.

Storage Class can be used by many Persistent Volumes.

Storage class contains:
Provisioner – who is storage? Eg. Amazon EBS
Parameters - what type of storage of Amazon EBS to use
reclaimPolicy
- If the pod is gone does the volume remain?
```


Persistent Volome Claim

```
A Persistent Volume Claim (PVC) is used to decouple Persistent
Volumes from pods. A PVC asked for a type of storage, and if a PV that
meets the criteria is matched, the a PV is Claimed and Bound.
PVCS similar to a Pod requesting resources from a Node
Pods consume node resources
PVCS consume PV resources
Pods can request specific levels of resources (CPU and Memory)
PVCS can request specific size and access modes (e.g., they can
be mounted ReadWriteOnce, ReadOnlyMany or
ReadWriteMany, see AccessModes).

```


ConfigMaps

```
A ConfigMap is an API object used to store non-confidential data in key-value pairs for pods

Pods can consume ConfigMaps as:
environment variables
command-line arguments
configuration files in a volume

A ConfigMap allows you to decouple environment-specific configuration from your container images, so that your applications are easily portable.
```


Kubernetes Services
```

Kubernetes Service allows you to attach a static IP address and DNS name for a set of pods
A K8s Service allows you to persist an address for a pod even if it dies. K8s Service also acts a load balancer

A Pod without a service will have a dynamic IP address. So when the pod dies so does the IP address.


following service types:
ClusterIP - default, randomly forward traffic to any pod set with target port
Headless - send traffic to very specific pod, when you have stateful pods eg. Database
NodePort – external service, allows you to use worker node IP address
LoadBalancer – similar to nodeport except leverages Cloud Service Provider's (CSPS) load balancer
ExternalName – a special service that does not have selectors and uses DNS names instead

```

Traffic Policies

```
K8s Service allows you set a Traffic policies to determine how ingress traffic is routed.
There are 2 types of Traffic policies

1. External Traffic Policy
how traffic from external sources is routed and has two valid values:
Cluster – route external traffic to all ready endpoints
Local - only route to ready node-local endpoints

2. Internal Traffic Policy
how traffic from internal sources is routed (has the same two values as External)

If the traffic policy is Local and there are are no node-local endpoints,
then kube-proxy does not forward any traffic for the relevant Service

```


ClusterlP

```
ClusterIP is the default service type for a K8 service.
It is used for internal traffic. External traffic will not reach the service. Traffic will be randomly distributed to any targeted pods.

Traffic originating from within the cluster will pass through the Node's Kubernetes Proxy and then onto to Kubernetes Service

A service can span multiple worker nodes for cross-node pods.

When to use ClusterIP:
Debugging
Testing
Internal traffic
Internal Dashboards
```

NodePort

```
NodePort allows you to expose a port for Virtual Machines running pods that the Service is managing.

There is no external load balancer so NodePort is intended for a single Kubernetes Service and for non production workloads.

```



Ports types
```
Port exposes the Kubernetes service on the specified port within the cluster.
Other pods within the cluster can communicate with this server on the specified port.

Target Port is the port on which the service will send requests to, that your pod will be
listening on. Your application in the container will need to be listening on this port also.

NodePort
exposes a service externally to the cluster by means of the target nodes IP
address and the NodePort. NodePort is the default setting if the port field is not specified.

```


Load Balancer 
```
A Load Balancer service type allows you to use an External Load Balancer.
The External Load Balancer handles the routings and traffic distribution logic

An external load balancer would be from a managed third-party cloud service.
Eg. AWS Network Load Balancer (NLB)

Load Balancer type is well suited for production workloads.
Generally its recommended to use Kubernetes Ingress

```

Headless 

```
A headless service is a service with no ClusterIP address

A headless service does not provide load balancing or proxying
Headless are useful when you are dealing with a Stateful application (reads and writes) and you need writes to go to a specific pod.

Headless Service is needed to manage network identity of the stateful pods by assigning
DNS Record to each pod so you can route traffic to a DNS Hostname.

```


External Name

```
ExternalName Services as the same as ClusterlP Service with the exception of instead returning a StaticIP it returns a CNAME record.

What is a CNAME record? - Canonical Name (CName) record is a DNS record that maps one doman name (an alias) to another name (canonical name)

```


Kubernetes Ingress

```
Ingress exposes HTTP and HTTPS routes from outside the cluster to services within the cluster.
Traffic routing is controlled by rules defined on the Ingress resource.

The reason we use a K8s Ingress is so we can translate a custom domain on SSL to a service running within our K8s cluster.

```

DNS

```

What is a Domain Name System (DNS)?
It is a service that is responsible for translating (or resolving) a service name to its IP address.

CoreDNS is the default DNS server for Kuberentes and ensures pods and services haves Fully Qualified
Domain Name (FQDN). Without CoreDNS the cluster communication would cease to work.

FQDN?
a domain name that specifies its exact location in the tree hierarchy, also known as an absolute domain.

In-Tree Plugins (Internal Plugins)
acl - enforces access control policies on source ip and prevents unauthorized access to
DNS servers
any - any gives a minimal response to ANY
azure - enables serving zone data from Microsoft Azure DNS service
cache - enables a frontend cache
health - enables a health check endpoint
log - enables query logging to standard output
and many more...
Out-of-Tree Plugins (External Plugins)
git – pull git repositories
alias – replaces zones apex CNAMES
redisc – enables a networked cache using Redis
Kubernetai – serve multiple Kubernetes within a Server


CoreDNS pods are abstracted by a service object called kube-dns.

Each pod (any pod, not just CoreDNS pods) has a resolv.conf file to help with DNS resolving.

nslookup my-service-name
# Server: 10.100.0.10
# Address: 10.100.0.10#53
# Name: kubernetes. default.svc.cluster.local
# Address: 10.100.0.1

```


Probes
```
Liveness Probe :
The kubelet uses liveness probes to know when to restart a container.
For example, liveness probes could catch a deadlock, where an application is running, but unable
to make progress. Restarting a container in such a state can help to make the application more
available despite bugs.

Readiness Probe :
The kubelet uses readiness probes to know when a container is ready to start accepting traffic.
A Pod is considered ready when all of its containers are ready.
One use of this signal is to control which Pods are used as backends for Services.
When a Pod is not ready, it is removed from Service load balancers.


Startup Probe:
The kubelet uses startup probes to know when a container application has started.
If such a probe is configured, it disables liveness and readiness checks until it succeeds, making
sure those probes don't interfere with the application startup.This can be used to adopt liveness checks on slow starting containers, avoiding them getting
killed by the kubelet before they are up and running.

```


Network netfilter

```
The netfilter project enables:
packet filtering
network address and port translation (NAPT)
Translation packet logging
Userspace packet queueing
other packet mangling


The netfilter
hooks are a framework inside the Linux kernel that allows kernel modules
to register callback functions at different locations of the Linux network stack.

The registered callback function is then called back for every packet that traverses the
respective hook within the Linux network stack.

Projects build ontop of Netfilter:
Iptables - generic firewalling software that allows you to define rulesets
Nftables – successor to iptables, more flexible, scalable and performance packet classification
IPVS – specifically designed for load balancing, uses hash mapping generic firewalling software that allows you to define rulesets

```


IP Tables

```
What is a Userspace?
Modern computer operating system segregates virtual memory into kernel space and user space
Kernel space is reserved for running a privileged operating system kernel, kernel extensions, and device drivers.
User space is the memory area where application software and some drivers execute.

What is
IP Tables? iptables is a user-space utility program that allows a system administrator to configure the IP packet filter rules of the Linux kernel firewall
iptables applies to IPV4
ip6tables to IPV6


Iptables are simply virtual firewalls on Linux

It is common to and modify iptables to restrict access based on ports and protocols
iptables -I INPUT 1 -p tcp --dport 80 -j ACCEPT

```

IPVS
```
IP Virtual Server (IPVS) uses the NetFilter framework.IPVS also incorporates LVS (Virtual Linux Server)

Iptables struggles to scale to tens of thousands of services as Iptables is bottlenecked at 5000 nodes per cluster

IPVS is specifically designed for load balancing and uses more efficient data structures (hash tables) allowing for almost unlimited scale under the hood

In the future the Kube Proxy will default to using IPVS.

```

Various Proxies
```
What is a proxy? a server application that acts as an intermediary between a client requesting a resource and the server providing that resource

There are many kinds of proxies you will encounter in Kubernetes:
Kubectl proxy – proxies from a localhost address to the Kubernetes apiserver
Apiserver proxy- a bastion built into the apiserver, connects a user outside of the cluster to cluster IPs which otherwise might not be reachable
Kube proxy – runs on each node and used to reach services
Proxy/Load balancer in front of API servers – acts as load balancer if there are several apiserver
Cloud Load Balancers - for external cluster traffic to reach pods
Forward Proxy: A bunch of servers egressing traffic have to pass through the proxy first
Reverse Proxy: Ingress traffic trying to reach a collection of servers

```

Kubeproxy

```
kube-proxy is a network proxy that runs on each node in your cluster. It is designed to load balance traffic to pods.
kube-proxy maintains network rules on nodes. These network rules allow network communication to your Pods from network sessions inside or outside of your cluster.

kube-proxy uses the operating system packet filtering layer (if there is one and it's available). Otherwise, kube-proxy forwards the traffic itself.

Kube-proxy can run in three modes:
1. ipt(default). - Suited for simple us
2. Ipvs - Suites for 1000+ services.
3. Userspace (legacy) - Not recommended for use

```



Service Mesh

```
A Service Mesh manages service-to-service communication for microservice architectures.
A service meaning "an application" not a K8s Service component.
A service mesh is an infrastructure layer that can provide the following: Reliability (Traffic Management, Retries, Load Balancing),Observability (Metrics, Traces),Security (TLS Certifications, Identity)

The installation of the proxies into pods as well as the proxies capabilities is managed by the Service Mesh Control Plane

Service
Meshes use a sidecar pattern.
A proxy container is installed on each pod. Application containers must pass through the proxy before leaving the egressing the pod.

Available Service Meshes for Kubernetes

Istio is the currently most popular service mesh for Kubernetes due to its highly configurable nature.
Istio uses Envoy as its proxy. Istio is a not a CNCF project.

Envoy is an open-source edge and service proxy. Multiple Service Meshes uses Envoy as its proxy.
Envoy is a graduated CNCF project.

Kuma is a CNCF sandbox project that uses Envoy as its proxy.

Linkerd is a CNCF graduated project is known for having strong security and "it just works".
Linkerd does not use Envoy, instead it uses a simple and ultralight "micro-proxy" called Linkerd2-proxy

Consul is an open-source service mesh by Hashicorp. Its not a CNCF project.
Consul is offered by Hashicorp as managed cloud Service Mesh.




Envoy

Envoy is a self contained process that is designed to run alongside every application server.
Envoy can be installed on a Virtual Machine or as a Container.

Envoy
supports a wide range of functionality
L3/L4 filter architecture
HTTP L7 filter architecture:
First class HTTP/2 support
HTTP/3 support
HTTP L7 routing
8RPC support
Service discovery and dynamic configuration
Health checking
Advanced load balancing
Front/edge proxy support
Best in class observability


In practice you likely will not install and manually configure Envoy, You would allow a Service Mesh control plane to install into your pods
A Service Mesh will may come with a Ul or configuration files to configure your envoy

```




NAT
```
What is Network Address Translation (NAT)?
a method of mapping an IP address space into another by modifying network address information in
the IP header of packets while they are in transit across a traffic routing device

```


EthO and Network Namespace
```
What is an Ethernet Device?
An Ethernet Device is a software and/or hardware technology that allows a server
to communicate on a computer network. A Network Interface Card (NIC) are
commonly used to establish a wired connection to a network.

Cloud Service Providers (CSPS), have Virtual NICS for your Virtual Machines (VMs) to connect to the Virtual Network

What is a Network Namespace?
etho represents the first Ethernet interface attached to your
Virtual Machine.

A network interface is an abstraction on top of the ethernet
interface to provide a logical networking stack with its own
routes, firewall rules, and network devices.

Linux by default has one Network Namespace called the Root
Network Space and this is what programs will use by default.


ifconfig
To create modify or view Network Namespaces the ip netns command can be used.
sudo ip netns add nsl
ip netns ns1

```


Cluster Networking
```
Kubernetes has the following opinions about cluster networking:
• all Pods can communicate with all other Pods without using NAT
• all Nodes can communicate with all Pods without using NAT.
the IP that a Pod sees itself as, is the same IP that others see it as

NATS are and can be used in Kubernetes, even though of the above contradiction


There are 4 broad types of network communication for clusters:

1. Container-to-Container : Containers all in the same pod have the same IP address and port space.
Containers can communicate with each other via localhost via different ports

2. Pod-to-Pod :

For Pod to Pod communication on the same on the node Veth is used to communication from the Pod
Network Namespace to the Root Network Namespace.

In the Root Network Namespace a bridge is used allow all Pod Network Namespaces to talk to other pods.

Pods can see all other pods, and communicate using their IP addresses.

Routing allows multiple networks to communicate independently and yet remain separate using a Router

Bridging connects two separate networks as if they were a single network using a Bridge

3. Pod-to-Service

When a pod dies its IP Address changes and this can make communication hard if you are relying on the IP address for
communication.

A Service creates a virtualized IP (static IP) and then uses iptables which is installed on the Node to Network Address
Translation (NAT) and Load Balancing to other pods.


4. External-to-Service

```


Virtval Ethernet Devices (Veths)

```
veth devices are Virtual Ethernet devices.

They can act as tunnels between network namespaces to create a bridge to a physical network device
in another namespace, but can also be used as standalone network devices.
Packets on one device in the pair are immediately received on the other device
veth devices are always created in interconnected pairs

You use the ip link command to create veth pairs

ip link add <pl-name> netns <pl-ns> type veth peer <p2-name> netns <p2-ns>

```

Ingress/Egress From Internet to Cluster

```
Egress
How pod traffic exits to the internet will be network specific.
So in the case of AWS pods use the Amazon VPC Container
Network Interface (CNI) plugin to be able to talk to your
Virtual Private Cloud (VPC) and then egress out to the
Internet Gateway (IGW) via route tables.

Ingress
For traffic to reach a pod, it travels to a service.
From there a Service could be using:

K8s Service with Type Load Balancer
This will work with Cloud Controller Manager to
implement a solution that works with a T4 (UDP/TCP)
Load Balancer

K8s Ingress
It will use a Ingress Controller to work with a Cloud
Service Provider load balancer eg. T4 or T7 (application
load balancer)

```

Cluster Layer security
```
1. Components of the cluster
Securing configurable cluster components
Controlling access to the Kubernetes API
Use Transport Layer Security (TLS) for all API traffic
API Authentication
API Authorization
Controlling access to the Kubelet
Controlling the capabilities of a workload or user at runtime
Limiting resource usage on a cluster
Controlling what privileges containers run with
Preventing containers from loading unwanted kernel modules
Restricting network access
Restricting cloud metadata API access
Controlling which nodes pods may access
Protecting cluster components from compromise
Restrict access to etcd
Enable audit logging
Restrict access to alpha or beta features
Rotate infrastructure credentials frequently
Review third party integrations before enabling them
Encrypt secrets at rest
Receiving alerts for security updates and reporting vulnerabilities

2. Components in the cluster
Securing
the applications running within the cluster
RBAC Authorization (Access to the Kubernetes API)
Authentication
Application secrets management (and encrypting them in etcd at rest)
Ensuring that pods meet defined Pod Security Standards
Quality of Service (and Cluster resource management)
Network Policies
TLS for Kubernetes Ingress

```


Security
```
Network access to API Server (Control plane)

All access to the Kubernetes control plane is not allowed publicly on the internet and is controlled by
network access control lists restricted to the set of IP addresses needed to administer the cluster.

Network access to Nodes (nodes)
Nodes should be configured to only accept connections from the control plane on the specified ports, and accept
connections for services in Kubernetes of type NodePort and LoadBalancer.

Kubernetes access to Cloud Provider API
Each cloud provider grant a different set of permissions to the Kubernetes control plane and nodes. It is best to provide
the cluster with cloud provider access that follows the principle of least privilege for the resources it needs to administer.

Access to etcd
Access to etcd (the datastore of Kubernetes) should be limited to the control plane only. Depending on your
configuration, you should attempt to use etcd over TLS. More information can be found in the etcd documentation.

etcd Encryption
Wherever possible it's a good practice to encrypt all storage at rest, and since etcd holds the state of the
entire cluster (including Secrets) its disk should especially be encrypted at rest

Best Practices
1. Enable Kubernetes Role-Based Access Control (RBAC)
2. Use Third-Party Authentication for API Server
3. Protect etcd with TLS, Firewall and Encryption
4. Isolate Kubernetes Nodes
5. Monitor Network Traffic to Limit Communications
6. Use Process Whitelisting
7. Turn on Audit Logging
8. Keep Kubernetes Version Up to Date
9. Lock Down Kubelet

```


```
Role-based Access Controls
(RBAC) is a way of defining permissions for identities based on a organizational role.
RBAC authorization uses the rbac.authorization.k8s.io API group to drive authorization
decisions, allowing you to dynamically configure policies through the Kubernetes API.
To enable RBAC, start the API server with the --authorization-mode flag


kube-apiserver --authorization-mode=RBAC

With Kubernetes RBAC there are only Allow Rules, Everything is Deny by default.


By default Secrets are unencrypted in etcd store.
Anyone with access to the etcd store has access to the secrets
Anyone who has access to a Pod within Namespace with have access to the Secrets used by that pod

How to keep Secrets safe:
Enable Encryption at Rest for Secrets
Enable or configure RBAC rules that restrict reading data in Secrets
Use mechanisms such as RBAC to limit which principals are allowed to create new Secrets or replace existing ones


Network Policies act as virtual firewalls for pod communication.
Pod communication can be restricted based on the follow scopes:
Pod-to-pod
Namespaces
Specific IPs

Selectors are used to determine to select resources with matching labels for the Network Policy to be applied to
The Network Plugin you are using must support Network Policies
Eg. Calico, Weave Net , Cilium, Kube-router, Romana.

```

Certificates and TLS

```
Kubernetes provides a certificates.k8s.io API, which lets you provision TLS certificates signed by a Certificate
Authority (CA) that you control. These CA and certificates can be used by your workloads to establish trust.

What is Public key infrastructure (PKI)?
PKI is a set of roles, policies, hardware, software and procedures needed to create, manage,
distribute, use, store and revoke digital certificates and manage public-key encryption.

What is a x.509 certificate?
A standard defined by the International Telecommunication Union (ITU) for public key certifications.
X.509 certificates are used in many Internet protocol:
SSL/TLS and HTTPS
Signed and encrypted email
Code Signing and Document Signing
A certificate contains
An identity
A public key – RSA, DSA, ECDA etc...
- hostname, organization or individual


Kubernetes requires PKI for the following operations:
Client certificates for the kubelet to authenticate to the API server
Server certificate for the API server endpoint
Client certificates for administrators of the cluster to authenticate to the API server
Client certificates for the API server to talk to the kubelets
Client certificate for the API server to talk to etcd
Client certificate/kubeconfig for the controller manager to talk to the API server
Client certificate/kubeconfig for the scheduler to talk to the API server.
Client and server certificates for the front-proxy

most certificates are stored in /etc/kubernetes/pki
Lightweight distributions store in different locations
```


What is Autoscaling?
In computing; autoscaling is when systems without manual intervention adjust capacity (eg. amount of CPU, Ram)
to meet the demand (traffic from users) by adding or removing resources commonly triggered by events.

Horizontal Pod Scaling (HorizontalPodAutoscaler) Add more pods to meet the demand

Vertical Pod Scaling (VerticalPodAutoscaler) Right-size pods for the optimal CPU and memory resources

Cluster Auto Scaling (Cluster Autoscaler or Karpenter) Add or remove Nodes (compute servers) based on demand

Cluster API
Declarative APls and tooling to simplify provisioning, upgrading, and operating multiple Kubernetes clusters.
Cluster API can be extended to support any infrastructure (AWS, Azure, vSphere, etc.), bootstrap or control
plane (kubeadm is built-in) provider.


Deployment Strategies
```
A deployment strategy is a way to change or upgrade an application.

Rollout is when you replace or update servers with new version of an application
Rollback is when you replace or revert recently updated servers back to the previous version

There are several different deployment strategies that can be utilized with Kubernetes:

Recreate - Terminate the current pods, create new pods all at once

RollingUpdate - Replaces one or multiple pods at a time

Canary - Add new pods and route a subset of your users to the new server, if no bugs or errors occur, roll out
changes to all pods



Blue / Green - Deploy an exact copy of your entire infrastructure, swap the traffic and then terminate the old
environment or rollback to old environment

Blue / Green Deployment Is when you completely create a new
environment of all components, and you send all traffic to the
new (Green) environment, if its all okay, you then terminate the
old (blue) environment. If anything goes wrong you rollback to
blue and teardown green.
Zero downtime
No reduce availability
Slow to deploy but faster than Canary
If something goes wrong larger impact to users immediately
Instantly rollback to previous infrastructure




A/B Testing, Red / Black - Similar to Canary and Blue/Green but continuously keep the new environments
running to test features on a subset of users

Uses a Canary or Blue Green method of deployment but serves the new app
(experimental features) to subset of users based on a set of load balancing rules.

Uses a Canary or Blue Green method of deployment but serves the new app
(experimental features) to subset of users based on a set of load balancing rules.

Dark Launches - Similar to A/B Testing, except you use a feature flag to rollout new features, and rollback by
turning off the feature in the software instead of reverting infrastructure changes

Similar to A/B Testing, except A to B happens at the application layer (within the app code)
• You want to test a feature on a subset of users,
• You code a feature flag into your app to turn the new feature on or off
If the users like the feature you leave it switched on, if they don't you turn it off.
Doesn't require you to rollback at the infrastructure level. (fast rollback)


Recreate - Terminate all running instances and recreate with Also known as an In-Place deploy

Rolling Updates - slowly replaces pods one by one. This is the default strategy of Kubernetes
*Reduced availability might happen while each set of pods is taken terminated as new are created
Rollbacks can be slow and hard
Deploys will be slow

Two important values:
maxSurge:
The amount of pods that can be added
maxUnavailable :
The amount of pods that can be unavailable

A maxSurge of 2 and a maxUnavailable of 2 would ensure no drop in availability.
The deployment would have to first create the new pods before tearing down the old.

```
