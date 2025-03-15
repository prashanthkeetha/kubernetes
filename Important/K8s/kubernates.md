Kubernates is a container orchestration software tool .
the diffrence b/w Docker and Kubernates is k8s have self healing if thr containers dies (or) shutdowns controllers checks thed desired state is there if not seheduler creats the desired state.
K8s is a opensource project for CNCF(cloud Networking computing foundation)
kubenates express desired state by yaml files is call manifest.
kubernates have master and nodes.
we can interact k8s by two ways 
  1. REST API using json
  2. kubctl  
* Master have :
--------------
API Server - the resposiblity of API server to have communication to full cluster (Ex: It is the head of the cluster)
etcd- It is the memory storage of the cluster.
scheduler - which schedule the work to the and looks out for the new work
controller- this ensures that the desire state is maintained
* Nodes:
--------
container runtime: this is a technology which runs containers in the node
kube- proxy: ths is responsible for networking.
kubelet: Agent that runs on nodes with is waiting for master instructions.
___________________________________________________________________________
# KubeConfig Context
                    KubeConfig context is a configuration in Kubeconfig file that determines which kubernetes cluster, user credentinals, and namespace kubectl 
                    should use for executing commands.
                    -> It acts as a profile that allows users to switch between multiple Kubernetes environment easily. 

-> The kubeconfig should be configured in the home path of ~/.Kube/config path
 
_______________________________________________________________________________
* Selector: it is used to identify the objects
* Labels: Labels are used to specify the particular information about the objects(pods),the lables are specified for the pods.
          by using labels we get the attributes of the each pod by using lables we search the pod information.



_____________________________________________________________________________________
Using nodes on each node you need to install docker.
* By default k8s master willnot run workloads if we want the containers can run on master by using kublet
* Now k8s networking the reference specification is CNI.many vendors using these.we use flannel it is a network provider for k8s

* workloads:
 1. pods: This is smallest unit of creation in k8s cluster every pod have a uniqe ip address.
          pods have the containers
    pod have 1.namespaces 2.cgroups
    for pod we get one ip address in pods we have conatiners the ip will distribute for containers
    in k8s we use scaling we increase pods not containers
    In pods we have diffrent containers
    a ) containers: which run applications
    b ) Init containers: thecontainers which run before the application runs.
    c ) Ehepemral Containers: these are debugging containers which troubleshoot the containers. 
    * Every pod has a restart policy to restart the containers inside the pod.
 2. Replica controller:it replicates the pods in the desired state we want.
    *In replica controller the histroy will not be stored if the data changes
 3. Replica Set: 
    *Replica Set is replicates the pods as desired state we want.
    * It store the persistant data in thr deployments if the data changes it stores the previous data.


__________________________________________________________________________

CNI(container networking interface):
drivers: implement networking startegy 
sandbox: uses for namespaces
endpoint:
 CNI works more containers technology
 In case of k8s CNI networking interface we use.
 by using CNI the networking will avilable for the cluster and nodes and pods.
 

 ____________________________________________________________________________

K8s Networking model:
to communicate with other pods in the cluster
All thepods in k8s will have cidr ranges
when the two nodes need to communicate in the cluster the packet communicate with virtual ethernet interface to bridge
Scaling in k8s:
scaling doesn't mean increasing containers it is increasing pods.
------
to see inside th pod command we use is (kubectl describe pod <name of the pod>).
------
while creating pods we apply lables we can quary.

-----
Replica Controller:
-------------------
 it replicates the pods in the desired state we want.
* Replication controller is equality based workload it allows filtering by using labels
* In replica controller the histroy will not be stored if the data changes
Replica set:
------------
*Replica Set is replicates the pods as desired state we want.
* repication set is set based workload
* It store the persistant data in thr deployments if the data changes it stores the previous data.
K8s metrics:
-----------
Top command is used to see the running applications inside the pods and nodes.
It is used for top commands it should be installed it won't avilable in the cluster.
K8s UI:
------
______________________________________________________

# getting inside to the container in K8s is

1. kubectl exec -it <podname> -- /bin/bash or /bin/sh
2. 
______________________________________________________

---
** ADDONS
1. METRICS SERVER
2. DASHBOARD
3. K8S UI
---
# Service:
  --------
  * we use service to load balance the traffic for pods
  Lables which we give in the Selector have given to to pods which lables have call the all pods with same lable inside the cluster 
  whenever we create a service K8s give cluster ip it is (internal ip)
  To expose service to outside world we use
  1. node port: Nodeport used to expose pods external inside the nodes
  2. Load Balancer: it will work in cluster (AKS,EKS,GKS)
  3. External: will give a dns record


  ------
if my master do not have 


* PROBE:
--------
probe is nothing to check the pod state is it healthy are not.
There are three types of probes
1. liveness probe : if this fails pod will restart the container
2. readiness probe: if this fails the pod will not be served by kubernets service.
3. startup probe:  kubeproxy or kublet does this task intiall to check
