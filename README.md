### Kubernetes Autoscaling
Guide to Kubernetes autoscaling for cloud cost optimization. Autoscaling in Kubernetes demystified. <br/>
* Kubernetes is the de-facto standard for container orchestration. However Kubernetes is highly complex. It’s a difficult environment requiring a steep learning curve to seize its real potential. Kubernetes defines a complex infrastructure so that applications can be simple. Everything that an application typically has to take into consideration (security, logging, redundancy, scaling etc.) are all built into the Kubernetes fabric. <br/>
* Kubernetes Autoscaling : This automation capability means client don't have to manually provision & scale down resources as demand changes. It prevents needless spending. However despite (or maybe in spite) of the sophisticated autoscaling technology, a Kubernetes cluster often accumulates financial waste and creates performance bottlenecks over time. This article will help developers/administrators to overcome those to a large extent. <br/>
* As the prefix “auto” suggests, autoscaling in Kubernetes is one of the container orchestration platform’s major automation capabilities. It simplifies resource management that would otherwise require intensive human effort to achieve. It ensures optimal resource utilization and cloud spending. The tighter Kubernetes scaling mechanisms are configured, the lower the waste and costs of running application(s).  <br/>
* Different cloud platforms have different native autoscaling features. Kubernetes enables autoscaling at the cluster/node level as well as at the pod level, two different but fundamentally connected layers of Kubernetes architecture. <br/>
* The Kubernetes autoscaling mechanism uses two layers : 
  * Pod-based scaling—supported by the Horizontal Pod Autoscaler (HPA) and the newer Vertical Pod Autoscaler (VPA).<br/>
  * Node-based scaling—supported by the Cluster Autoscaler (CA). <br/>
* So there are three different methods supported by Kubernetes Autoscaling. HPA, VPA & CA. <br/>

  #### 1. [Horizontal Pod Autoscaler (HPA)](https://kubernetes.io/docs/tasks/run-application/horizontal-pod-autoscale/) <br/>
          HPA can be useful both for stateless applications and stateful workloads. <br/>
  #### 2. [Cluster Autoscaler (CA)](https://github.com/kubernetes/autoscaler/tree/master/cluster-autoscaler#cluster-autoscaler) <br/>
  #### 3. [Vertical Pod Autoscaler (VPA)](https://github.com/kubernetes/autoscaler/tree/master/vertical-pod-autoscaler)<br/>
