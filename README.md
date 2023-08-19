### Kubernetes Autoscaling
Scalability is one of the core benefits & value propositions of Kubernetes (K8s). In order to get the most out of this benefit (and use K8s effectively), a solid understanding of how Kubernetes autoscaling works is required.  <br/>
* Kubernetes is the de-facto standard for container orchestration. However Kubernetes is highly complex. It’s a difficult environment requiring a steep learning curve to seize its real potential. Kubernetes defines a complex infrastructure so that applications can be simple. Everything that an application typically has to take into consideration (security, logging, redundancy, scaling etc.) are all built into the Kubernetes fabric. <br/>
* **Shifting workloads to Containers is one of the most important cloud cost optimization techniques & reduces hosting costs.** Containers optimize costs by combining different tasks in fewer servers. Other benefits include a lower digital footprint, faster operability, built-in monitoring, etc.Hence it is essential to understand the cost saving and efficiency features of Kubernetes<br/>
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
      * Limitations & best practices of HPA <br/>
        * HPA is not compatible with unscalable workloads,  such as DaemonSets (who does not have replicas) <br/>
        * Cannot (should not) be used with VPA : An exception to this is when clients use a HPA that relies on custom and external metrics to scale the Pods.<br/>
        * Application may needs to be architected with scale out in mind so that distributing workloads across multiple servers is possible. <br/>
        * HPA makes scaling decisions based on resource request values at the container level. Therefore, it is essential to have configured the resource request values for all of your containers. Efficiently set CPU and memory limits on pods, else pods may terminate frequently or, on the other end of the spectrum, there will be resource wastage. <br/>
        * Due to its inherent limitations, HPA works best when combined with Cluster Autoscaler - this allows to coordinate scalability of pods with the behavior of nodes in the cluster. <br/>
        * Prefer custom metrics over external metrics when possible—the external metrics API represents a security risk because it can provide access to a large number of metrics. A custom metrics API presents less risk if compromised, because it only holds specific metrics.<br/>
        * HPA Architecture <br/>
        ![image](https://user-images.githubusercontent.com/92582005/204138589-9f9ceefd-90ae-41db-8eb3-a1c9d847ff02.png) <br/>
   3. [Cluster Autoscaler (CA)](https://github.com/kubernetes/autoscaler/tree/master/cluster-autoscaler#cluster-autoscaler) <br/>
        * Clusters are how Kubernetes groups machines. They are comprised of Nodes which run Pods. Pods have containers that 
          request resources. The CA adds or removes Nodes in a Cluster based on resource requests from Pods.<br/>
        * Node starvation :  When a node doesn't meet the needed requirements for the pod's resource, the pod remains on 
          status Pending, and it's not scheduled to any nodes until there are enough resources available. <br/>
        * Cluster scalability : Scaling the cluster is remedy to the above problem. The CA increases the size of the cluster 
          when there are pods that are not able to be scheduled due to resource shortages. This process can be a little 
          overwhelming, especially for a cluster with variable demand. When the number of pods fluctuates a lot, it requires 
          the operator to constantly monitor for unscheduled pods and make changes in real time. Cluster Autoscaler(CA) is a 
          Kubernetes feature that automatically adjusts the number of worker nodes in a cluster based on workload demands. 
          <br/>
        * The cluster autoscaler(CA) watches for pods that can't be scheduled on nodes because of resource constraints. The 
          cluster then automatically increases the number of nodes. CA automates the process of scaling the cluster 
          manually. It's installed within the cluster and watches for unscheduled pods with resource constraints, then it 
          automatically increases the number of nodes in a cluster to meet these requirements. The CA decreases the number 
          of nodes in a cluster, if there's unused cluster capacity for a specified amount of time. <br/>
        * The cluster autoscaler decreases the number of nodes when there has been unused capacity for a period of time. Pods on a node to be removed by the cluster autoscaler are safely scheduled elsewhere in the cluster.<br/>
        * [The cluster autoscaler may be unable to scale down if pods can't move, in these situations](https://github.com/kubernetes/autoscaler/blob/master/cluster-autoscaler/FAQ.md#what-types-of-pods-can-prevent-ca-from-removing-a-node)<br/>
        * Cluster autoscaler does not support local PersistentVolumes. <br/>
        * It is best practice to use CA and HPA together. <br/>
        * [Demo and details are available here](https://github.com/somrajroy/AWS-EKS-Cluster-Autoscaling)<br/>
        * Limitations of CA : Below are some limitations of CA which architects needs to keep in mind. <br/>
           * Request-based scaling : CA does not make scaling decisions using CPU or memory usage.  CA scales a cluster 
             based on the resource requests of the pods running on it, rather than their actual usage. This limitation means 
             that the unused computing resources requested by users will not be detected by CA, resulting in a cluster with 
             waste and low utilization efficiency. Hence right sizing pods is important & HPA is recoommeded along with CA. 
             Scaling kubernetes clusters requires fine-tuning the setting of the autoscalers so that they work in concert. 
             This combination of CA & HPA makes it very simple to automate scaling for container workloads and the 
             Kubernetes environment. CA & HPA should be used in concert together for maximum benefit.<br/>
           * Scaling can take time : Whenever there is a request to scale up the cluster, CA issues a scale-up request to a 
             cloud provider within 30–60 seconds. The actual time the cloud provider takes to create a node can be several 
             minutes or more. This delay means that the application performance may be degraded while waiting for the 
             extended cluster capacity.  <br/>
           *  Lack of on-premise support: Cluster Autoscaler is designed to work with cloud-based Kubernetes clusters, and 
              there is limited support for on-premise deployments. This may limit its use in certain scenarios.<br/>
   5. [Vertical Pod Autoscaler (VPA)](https://github.com/kubernetes/autoscaler/tree/master/vertical-pod-autoscaler)<br/>
        * This is a new technology. Kubernetes Vertical Pod Autoscaler (VPA) is an autoscaler that enables automatic CPU and memory request and limit adjustments based on historical resource usage measurements. <br/>
        * Updating running pods is still experimental in VPA, and performance in large clusters remains untested. VPA reacts to most out-of-memory events, but not all, and the behavior of multiple VPA resources that match the same pod remains undefined. <br/>
        * VPA recreates pods when updating pod resources, possibly on a different node. As a result, all running containers restart. Kubernetes doesn’t allow dynamic changes in the resource limits of a running pod. The VPA can’t update the existing pods with new limits. VPA terminates pods using outdated limits. When the pod’s controller requests a replacement, the VPA controller injects the updated resource request and limits values to the new pod’s specification.<br/>
        * Avoid using HPA and VPA in tandem — HPA and VPA are incompatible. It is not advisable to use both HPA & VPA together for the same set of pods, unless HPA is configured to use either custom or external metrics.<br/>
        * It is advisable to use VPA with CA.<br/>
        * A VPA deployment has three main components: VPA Recommender, VPA Updater, and VPA Admission Controller. <br/>
        * VPA requires at least two healthy pod replicas to work. This kind of defeats its purpose in the first place and is the reason why it isn’t used extensively. As a VPA destroys a pod and recreates it to vertically autoscale it, it requires at least two healthy pod replicas to ensure there’s no service outage. This creates unnecessary design complications on single instance stateful applications. For stateless applications, it can be better off using a Horizontal Pod Autoscalar instead of VPA.<br/>
        * [VPA is still in preview with Azure as of December-2022](https://learn.microsoft.com/en-us/azure/aks/vertical-pod-autoscaler)<br/>
        ![image](https://user-images.githubusercontent.com/92582005/204138533-83b7007c-30e1-4dd2-b1d3-e35458a49689.png) <br/><br/>
        
   #### Further references <br/>
     * [What does AWS mean by Cost Optimization?](https://www.linkedin.com/comm/pulse/what-does-aws-mean-cost-optimization-neal-davis)<br/>
     * [How to save money on Amazon Web Services](https://www.linkedin.com/pulse/how-save-money-amazon-web-services-neal-davis)<br/>
     * [8 best practices to reduce AWS bill for Kubernetes](https://cast.ai/blog/8-best-practices-to-reduce-your-aws-bill-for-kubernetes/)<br/>
     * [Cluster autoscaling with AKS](https://learn.microsoft.com/en-us/training/modules/aks-cluster-autoscaling/)<br/>
