
# kubectl


## Validation and generators

kubectl uses something called generators which is an abstraction that takes care of serialization.

resources that have --restart-policy=Always are considered Deployments

Resources that have --restart-policy=Never are considered Pods. 



# kube-apiserver

 the server first starts, it looks at all the CLI flags the user provided and assembles a list of suitable authenticators.
 
 the x509 handler will verify that the HTTP request is encoded with a TLS key signed by the CA root cert
 
the bearer token handler will verify that the provided token (specified in the HTTP Authorization header) exists in the file on disk specified by --token-auth-file

the basicauth handler will similarly ensure that the HTTP request's basic auth credentials match its own local state.

## Authorization

webhook, which interacts with an off-cluster HTTP(S) service;

ABAC, which enforces policies defined in a static file;

RBAC, which enforces RBAC roles which are added by the admin as k8s resources

Node, which ensures that node clients, i.e. the kubelet, can only access resources hosted on itself.


## Admission control

what should and should not be permitted

Each controller is stored as a plugin in the plugin/pkg/admission directory, and is made to satisfy a small interface. 

Each one is then compiled into main kubernetes binary itself


InitialResources - sets default resource limits to the resources for a container based on past usage

LimitRanger - sets defaults for container requests and limits, or enforce upper bounds on certain resources (no more than 2GB of memory, default to 512MB)

ResourceQuota - calculates and denies a number of objects (pods, rc, service load balancers) or total consumed resources (cpu, memory, disk) in a namespace.
