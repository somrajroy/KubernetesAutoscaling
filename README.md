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

   1. [Horizontal Pod Autoscaler (HPA)](https://kubernetes.io/docs/tasks/run-application/horizontal-pod-autoscale/) : <br/>
      * Horizontal scaling refers to the deployment of extra pods in the Kubernetes Cluster in response to a growing load. HPA can be useful both for stateless applications and stateful workloads. Allows customers to dynamically (i.e., automatically) increase or decrease the number of running pods based on application’s usage changes.<br/>
      * HPA also decreases the number of pods when application usage comes down. The HPA takes the mean of a per-pod metric value for calculations. It calculates whether removing or adding replicas would bring the current value closer to the target value. <br/>
      * It is to be noted that Horizontal Pod autoscaling is not compatible with unscalable workloads,  such as DaemonSets. Daemonsets work on a “one pod per node” basis, and therefore, they cannot have replicas, which is why HPA is not compatible with Daemonsets. <br/>
      * Before setting up Kubernetes Autoscaling, Metric Server needs to be installed the Cluster. The Kubernetes Metrics Server is a resource consumption data aggregator which isn’t installed by default. The Kubernetes Metrics Server collects resource metrics from each worker node’s kubelet and presents them to the Kubernetes API server via the Kubernetes Metrics API. <br/>
      * The Kubernetes Metrics Server collects resource metrics from each worker node’s kubelet and presents them to the Kubernetes API server via the Kubernetes Metrics API.<br/>
      * HPA works best when combined with Cluster Autoscaler. When the existing nodes on the cluster are exhausted, HPA cannot scale resources up, so it needs Cluster Autoscaler to help add nodes in the Kubernetes cluster.<br/>
      * [Demo and details are available here](https://github.com/somrajroy/Kubernetes-HPA-minikube)<br/>
      * Limitations of HPA <br/>
        * HPA is not compatible with unscalable workloads,  such as DaemonSets (who does not have replicas) <br/>
        * Application may needs to be architected with scale out in mind so that distributing workloads across multiple servers is possible. <br/>
        * HPA makes scaling decisions based on resource request values at the container level. Therefore, it is essential to have configured the resource request values for all of your containers. Efficiently set CPU and memory limits on pods, else pods may terminate frequently or, on the other end of the spectrum, there will be resource wastage. <br/>
        * Due to its inherent limitations, HPA works best when combined with Cluster Autoscaler.  <br/>
   3. [Cluster Autoscaler (CA)](https://github.com/kubernetes/autoscaler/tree/master/cluster-autoscaler#cluster-autoscaler) <br/>
   4. [Vertical Pod Autoscaler (VPA)](https://github.com/kubernetes/autoscaler/tree/master/vertical-pod-autoscaler)<br/>
