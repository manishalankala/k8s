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
