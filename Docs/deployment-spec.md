### apiVersion: 
We defined the api version for the deployment which is apps/v1.

### kind: 
It is set to Deployment.

### metadata: 
It contains information about the deployment. We set the name of the deployment to pixellib-demo.

### spec: 
It contains a complete list of the specifications for the entire deployment and the pods that will be created.

### spec.replicas: 
We define the number of pods and set it to 2. This means that there will be two instances of our application running.

### spec.selector.matchLabels: 
This selects the pod to be used in the Deployment, using the pod label which is app: myapp-pod.

### spec.template.metadata: 
We define metadata for the pod. spec.template.metadata.name assigns a name to the pod, which is pixellib-pod. 

### spec.template.metadata.labels 

specifies the labels for the pod which is app: myaoo-pod that makes it possible for Deployment to select the pod with that label. The label will also be used by the Service we shall define later to select the pod.

### spec.template.spec.containers : 
We define the specifications for containerized applications. 

### spec.template.containers.name :
specifies the name of the container that will be created from a docker image, 


### spec.template.containers.image 
specifies the image from which docker container will be created and the image used is a docker image I published on docker hub with the name mydocker/myapp . 

### spec.template.containers.imagePullPolicy 
is set to IfNotPresent, that specifies that the image should be pulled or downloaded if it is not available. 

### spec.template.containers.ports.containerPort 
specifies the container port that must match the exposed port from the docker image used and it is 5000.

