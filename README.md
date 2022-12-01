### Kubernetes Autoscaling
Scalability is one of the core benefits & value propositions of Kubernetes (K8s). In order to get the most out of this benefit (and use K8s effectively), a solid understanding of how Kubernetes autoscaling works is required.  <br/>
* Kubernetes is the de-facto standard for container orchestration. However Kubernetes is highly complex. It’s a difficult environment requiring a steep learning curve to seize its real potential. Kubernetes defines a complex infrastructure so that applications can be simple. Everything that an application typically has to take into consideration (security, logging, redundancy, scaling etc.) are all built into the Kubernetes fabric. <br/>
* **Shifting workloads to Containers is one of the most important cloud cost optimization techniques & reduces hosting costs.** Containers optimize costs by combining different tasks in fewer servers. Other benefits include a lower digital footprint, faster operability, built-in monitoring, etc.Hence it is essential to understand the cost saving and efficiency features of Kubernetes<br/>
* Kubernetes Autoscaling : This automation capability means client don't have to manually provision & scale down resources as demand changes. It prevents needless spending. However despite (or maybe in spite) of the sophisticated autoscaling technology, a Kubernetes cluster often accumulates financial waste and creates performance bottlenecks over time. This article will help developers/administrators to overcome those to a large extent. <br/>
* [Cost optimization](https://www.gartner.com/en/information-technology/glossary/cost-optimization) is a business-focused, continuous discipline to drive spending and cost reduction, while maximizing business value. It includes:<br/>
  * Obtaining the best pricing and terms for all business purchases <br/>
  * Standardizing, simplifying and rationalizing platforms, applications, processes and services <br/>
  * Automating and digitalizing IT and business operations <br/>
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
        * The cluster autoscaler watches for pods that can't be scheduled on nodes because of resource constraints. The cluster then automatically increases the number of nodes.<br/>
        * The cluster autoscaler decreases the number of nodes when there has been unused capacity for a period of time. Pods on a node to be removed by the cluster autoscaler are safely scheduled elsewhere in the cluster.<br/>
        * [The cluster autoscaler may be unable to scale down if pods can't move, in these situations](https://github.com/kubernetes/autoscaler/blob/master/cluster-autoscaler/FAQ.md#what-types-of-pods-can-prevent-ca-from-removing-a-node)<br/>
        * Cluster autoscaler does not support local PersistentVolumes. <br/>
        * It is best practice to use CA and HPA together. <br/>
        * [Demo and details are available here](https://github.com/somrajroy/AWS-EKS-Cluster-Autoscaling)<br/>
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
     * [Key principles - three fundamental drivers of cost with AWS](https://docs.aws.amazon.com/whitepapers/latest/how-aws-pricing-works/key-principles.html)<br/>
     * [Optimise your Azure costs](https://azure.microsoft.com/en-in/solutions/cost-optimization/)<br/>
     * [How to optimize your cloud investment with Cost Management](https://learn.microsoft.com/en-us/azure/cost-management-billing/costs/cost-mgt-best-practices)<br/>
     * [AWS Cost Optimization](https://wa.aws.amazon.com/wellarchitected/2020-07-02T19-33-23/wat.pillar.costOptimization.en.html)<br/>
     * [Azure Cost Optimization Techniques](https://www.linkedin.com/pulse/azure-cost-optimization-techniques-dr-rabi-prasad-padhy?trk=pulse-article_more-articles_related-content-card)<br/>
     * [Azure cost optimization: 8 ways to save when using Azure](https://www.nigelfrank.com/insights/8-ways-to-save-when-using-azure)<br/>
     * [6 Azure cost optimization best practices](https://www.techtarget.com/searchcloudcomputing/tip/Implement-these-Azure-cost-optimization-best-practices)<br/>
     * [Azure Cost Optimization: 12 Ways to Save on Azure](https://bluexp.netapp.com/blog/azure-cost-optimization-12-ways-to-save-on-azure-cvo-blg)<br/>
     * [Azure Data Transfer Costs: Everything You Need To Know](https://cloudmonitor.ai/2021/08/azure-data-transfer-costs-everything-you-need-to-know/)<br/>
     * [Overview of Data Transfer Costs for Common Architectures](https://aws.amazon.com/blogs/architecture/overview-of-data-transfer-costs-for-common-architectures/)<br/>
