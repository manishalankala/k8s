Argo CD is a GitOps continuous delivery (CD) tool on top of Kubernetes.

Events Argo CD can process: AMQP, AWS SNS, AWS SQS, Cron Schedules, GCP PubSub, GitHub, GitLab, HDFS, File Based Events, Kafka, Minio, NATS, MQTT, K8s Resources, Slack, NetApp StorageGrid, Webhooks, Stripe, NSQ, Emitter, Redis, and Azure Events Hub.

Actions Argo CD can perform: Argo Workflows, Standard K8s Objects, HTTP Requests, AWS Lambda, NATS Messages, Kafka Messages, Slack Notifications, Argo Rollouts CR, Custom/Build Your Own Triggers, and Apache OpenWhisk.



defines an argoproj.io/v1alpha1/Application resource

project — The Argo CD project where this application should reside

source.repoURL — The GitHub repository URL from which ArgoCD should sync the manifests

source.targetRevision — The revision of the Git repository from which ArgoCD should sync the manifests

source.path — The path of the directory in the Git repository where the manifests to be applied are located

destination.server — The Kubernetes API server URL where Argo CD should deploy the resources

destination.namespace — The namespace where Argo CD should deploy the resources. In this case, it is the default namespace

syncPolicy.automated.selfHeal — It signifies that the sync is automated. The selfHeal flag specifies that partial app sync is needed if the resources change in the target (i.e. the Kubernetes cluster) but not the source.
