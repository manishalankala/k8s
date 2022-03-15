

Cluster -   ``` A logical grouping of all components within a cluster.```

```



Namespace
```
A named logical grouping of Kubernetes components within a cluster.
Used to Isolate different workloads on the same cluster.
```

Node
```
A virtual machine or underlying server. There are two types of nodes: Control Plane and Worker nodes.
Worker nodes is where your application or workloads run. Control Plane nodes manage worker nodes.
```

Pod
```
The smallest unit in K8s. It is an abstraction over a container.Generally defines an application workload
```

Service
```
A static IP address and DNS name for a set of pods (persists an address even if a pod dies) and a load balancer
A "service" can also mean a container that continuously runs.
```

Ingress
```
Translates HTTP/S rules to point to services
```


API Server
```
The API Server allows users to interact with K8s components using the KubeCTL or by sending HTTP requests.
```
Kubelet
```
Kubeletis an agent installed on all nodes. Kublelet allows users to interact with node via the API Server and KubeCTL.
```
KubeCTL
```
A command line interface (CLI) that allows users to interact with the cluster and components via the API Server
```
Cloud Controller Manager
```
Allows you to link a Cloud Service Provider (CSP) eg. AWS, Azure GCP to leverage cloud services.
```
Controller Manager
```
A control loop that watches the state of the cluster and will change the current state back to desired state.
```
Scheduler
```
Determines where to place pods on nodes. Places them in a scheduling a queue.
```
Kube Proxy
```
An application on worker nodes that provides routing and filtering rules for ingress (incoming) traffic to pods.
```


ConfigMap
```
allows you to decouple environment-specific configuration from your container images, so that
your applications are easily portable. Used to store non-confidential data in key-value pair
```

Secret
```
small amount of sensitive data such as a password, a token, or a key
```

Volumes
```
mounting storage eg. locally on the node, or remote to cloud storage
```

StatefulSet
```
provides guarantees about the ordering and uniqueness of these Pods
Think of databases where you have to determine read and write order or limit the amount of containers
StatefulSets are hard, when you can host your db externally from K8s cluster
```

ReplicaSets
```
Maintain a stable set of replica pods running at a given time. Can provide a guarantee of availability.
```

Deployment
```
Is a blueprint for a pod (think Launch Template)
```


Manifest
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
