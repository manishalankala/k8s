### apiVersion: 
We defined the api version for the Service which is v1.

### kind: 
We define the kind as Service.

### metadata: 
We specify the name of the Service as servicename.

### spec: 
We define the specifications for the Service to be created.

### spec.type: 
We define the service type as LoadBalancer, that will enable us to access apps running within the pods from outside.

### spec.ports: 
We define the ports to be used in the Service. We mapped port 80 to port 5000 of the target port of the running container.

### spec.selector: 
We specify the pod that will use the Service with the name app:myapp-pod from the Deployment.
