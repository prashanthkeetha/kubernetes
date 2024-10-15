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

# Kubeadm:
----------
* Cluster installation: Kubeadm is used to initialize a kubernetes master node with the command "kubeadm init".
* This sets up the control plane components such as the API server, controller manager, and scheduler.
* Node Joining: Kubeadm provides a token and certificate that worker nodes can use to join the cluster with the command kubeadm join.

_______________________________________________________________________________
* Selector: it is used to identify the objects
* Labels: Labels are used to specify the particular information about the objects(pods),the lables are specified for the pods.
          by using labels we get the attributes of the each pod by using lables we search the pod information


_____________________________________________________________________________________
Using nodes on each node you need to install docker.
* By default k8s master willnot run workloads if we want the containers can run on master by using kublet
* Now k8s networking the reference specification is CNI.many vendors using these.we use flannel it is a network provider for k8s

* workloads:
 1. pods: This is smallest unit of creation in k8s cluster every pod have a uniqe ip address.
          pods have the containers inside the container we run application.
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
 4. INGRESS:
       --> In kubernetes ingress is used to provide external access to service within cluster.
       --> It uses the layer 7 load balancer which is Application load balancer which have access the outside world
       --> It uses to send the traffic through the load balancer 
       Structure:
                            User send the request to the url or the IP
                                            | 
                                   the request hit the load balancer
                                            |
                    the load balancer send the request to the INGRESS CONTROLLER
                                            |
                            INGRESS resource checks the resource
                                            |
                                INGRESS CONTROLLER send the request to service  
  5. CONFIGMAP:
      It is used to store the configration data of the pod which is not more risk to expose the data outside.
  6. SECRET:
      IT is used to store the data in the secret by encrypt the data using BASE64 and SHA256
      There are multiple types of secrets are there.
      1. Opaque
      2. Docker Registry
      3. TLS
      4. Service Account  
  7. node and PVC:
        "PVs are used to provide storage in Kubernetes clusters. When a PersistentVolumeClaim (PVC) is created, it binds to an available PersistentVolume (PV), effectively associating the storage resource specified by the PV with the PVC. This allows the PVC to access and utilize the storage defined by the PV for storing data.
         PVCs provide a way for applications to request storage in Kubernetes clusters. They can specify storage requirements such as storage class, access mode, and storage capacity. The underlying storage can be provided by various storage solutions, including hostPath for local storage or cloud-based storage solutions.
         # kubectl apply -f <yamlfilename>
         # kubectl get pv
         # kubectl get pvc
         # kubectl describe pv <pvname or id>
         # kubectl describe pvc <pvc name or id>
         # kubectl delete pv <pvname or id>
         # kubectl delete pvc <pvcname or id>
  8. Statefulset:
        Each pod in statefulset assigend a stable naming convention. This allows stateful application consistent network idtentity even they scale up and down
        In statefulset the pod are deployed in sequential order. It is used for storage purpose
        In statefulset there is persistant volume and persistent volume claim which stores the data externally without depending on pod.
        Headless service : this means which have uniqe IP to each pod in the statefulset
        Vertical scaling: In this which we increase the storgae capacity.
        Horizental scaling(HPA): IN this which we increase the number of the replicas or the pods the the application.
  9. Deamonset:
          It is used to deploy pods in each nodes that present in the cluster most of the cases it is used to deploy [Monitoring and Database] like Prometheous and Percona-db.
  10. HPA: IN this which we increase the number of the replicas or the pods using the application metrics. 
  11. Deployment:
              By using deployment we deploy our application without downtime it has a staragy which is (recreate and rolling update,Blue&green and A/B )
              RollOut: Updating the changes in the application if it not work again going back to the old version is called rollout
              -> When you want to go to the previous version of the application by using rollout
      # kubectl rollout history deployment/<nameofthedeployment> --revision=<numberoftheperviousversionwhichyouwant> (eg: --revision=2) 
      # kubectl rollout undo deployment/<nameofthedeployment> by using this command you can go back to the previous version.
  12. Node Affinity And Node Anti-Affinity:
        # requiredDuringSchedulingIgnoredduringExection it is a softrule
                   It is the first preffred rule
        # preferredDuringSchedulingIgnoredDuringExecution it is a hardrule

         The Node-Affinity have two types one is <requiredDuringSchedulingIgnoredduringExection> and <preferredDuringSchedulingIgnoredDuringExecution>
          1. Node-Affinity while scheduling the pod to the particular node the pod should have the same labels that which we are giving in the NodeAffinity it is called
            <requiredDuringSchedulingIgnoredduringExection> if the label doesnot matches the pod will go into pending state until the given label matches in the node.
            -> After matches the label if we remove the label from the node the pod don't get any distrubance the pod will run it is called <IgnoredduringExection>
          2. While scheduling the pod to the node without having matched label it will schedule to any of the nodes<preferredDuringSchedulingIgnoredDuringExecution>     after matching the assigned label to will place back to the assigned label node.
   13. NodeName:
            NodeName is nothing but assigning the pod to the particular node using the nodename
   14. NodeSelector:
           Kubernetes only schedules the Pod onto nodes that have each of the labels you specify.
   15. Pod-Affinity:
            The Pod-Affinity is used to schedule the pod co-locate in the same node using <labelselectors> and <matchexpressions>to communicate each other and by mention <topologyKey> which is the hostname
      Pod-Antiaffintiy:
             It is used to schedule the pods in the different nodes using <labelselectors> and <matchexpressions>to communicate each other mention <topologyKey> which is the hostname
   16. Taint and Tolarate:
         Taint: Added to the pod, only pods which can tolarate this taint can be schedule.
            There are 3 types of taints are there.
            1. NoSchedule: Hard(Donot schedule the pod if they can't tolarate)
            2. PrefereNoSchedule: soft (can schedule the pods if the no other nodes are avilable)
            3. NoExecute: strict(Delete running pods also if they can't tolarate newly added taint)
        Tolarations:
              Tolaration is used to schedule the pod even the node is tainted
# Imagepullpolicy:
------------------
1. ALways:
             Kubernetes will always pull the image from the registry whenever the pod is started. This ensures that you always get the latest version of the image.
             -> It take longer startup times if the image is larger or if the registry is slow.
2. IfNotPresent:
            Kubernetes pull the image if the image is not present in the node
               
3. Never:
________________________________________________________________________


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
* It store the persistant data in the deployments if the data changes it stores the previous data.
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
# NAMESPACES:
to go inside the namespace kubectl get pods -n <namespacename>
Defination: Namespaceis a isolated area which divide the clustur into multiple parts of cluster.
In Namespaces there is RBAC(Role Back access control) where have access permissions depend on the roles.
There are four namespaces that get create while creating cluster
1. Default: it is where we work
2. kube-node-lease
3. kube-public
4. kube-system

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

  1. node port: 
   used to expose pods external inside the nodes by using node ip
  2. Load Balancer: it will work in cluster (AKS,EKS,GKS)
  3. External: will give a dns record
  4. 


  ------
if my master do not have 


* PROBE:
--------
probe is nothing to check the pod state is it healthy are not.
There are three types of probes
1. liveness probe : if this fails pod will restart the container
2. readiness probe: if this fails the pod will not be served by kubernets service.
3. startup probe:  kubeproxy or kublet does this task intiall to check weather all the necessary 

Without executing the yaml file we can check wheather the syntax is correct or not and the keyvalue pairs is correct is not.
# kubectl apply -f <yamlfilename> --dry-run  
------------------------------------------------------------------------------------
------------------------------------------------------------------------------------

{
View Persistent Volume Claim (PVC) Space: If you want to identify the space left in a specific Persistent Volume Claim (PVC), you can execute the following command:
# kubectl get pvc <pvc-name> --namespace <namespacename>
Replace <pvc-name> with the actual name of your PVC and <namespace> with the appropriate namespace.

Check Disk Space Inside a Running Pod: If there’s a running pod with a mounted PV from the PVC, you can use the following command to list all file systems, including the mounted volumes, and their free disk space:
# kubectl exec -n <namespace> <pod-name> -- df -h

Replace <namespace> with the desired namespace and <pod-name> with the name of the pod associated with the PVC

to see the storage usage of the node is
# kubectl top node
# kubectl top node <nodename>

Mark Node as Unschedulable: If you want to prevent new pods from being assigned to a specific node (e.g., for maintenance), use:
# kubectl cordon <node-name>

Mark Node as Schedulable: To make a previously unschedulable node available for pod assignment again:
# kubectl uncordon <node-name>

Safely Evict Pods from a Node: If you need to perform maintenance on a node, you can safely evict all pods from it using:
# kubectl drain <node-name>

Annotations are key-value pairs that provide additional data associated with an object. They can be larger than labels and include arbitrary string values
Update Node Annotation: To update an annotation for a specific node (replace <node-name> with the actual node name)

# kubectl annotate node <node-name> KEY=VALUE

For example, to set an annotation named flannel.alpha.coreos.com/public-ip with the value new-value, you can run:
# kubectl annotate node <node-name> flannel.alpha.coreos.com/public-ip=new-value

Overwriting Existing Annotation: If you want to overwrite an existing annotation, add the --overwrite flag:
# kubectl annotate node <node-name> KEY=VALUE --overwrite

Remove an Annotation: To remove an annotation (e.g., if it exists), simply omit the value:
# kubectl annotate node <node-name> KEY-

To Rename the node in kubernetes use

--------------------------------------------------
Generate a plain-text list of all namespaces:

# kubectl get namespaces
to go inside the name space kubectl get pods -n <namespacename>
Show a plain-text list of all pods:

# kubectl get pods

Generate a detailed plain-text list of all pods, containing information such as node name:

# kubectl get pods -o wide

Display a list of all pods running on a particular node server:

# kubectl get pods --field-selector=spec.nodeName=[server-name]
List a specific replication controller in plain-text:

# kubectl get replicationcontroller [replication-controller-name]

Generate a plain-text list of all replication controllers and services:

# kubectl get replicationcontroller,services

Show a plain-text list of all daemon sets:

# kubectl get daemonset

Copy Files to/from a Pod:
# kubectl cp <file_path> <pod_name>:<container_path>

-------------------------------------------------------------------------
Creating a Resource Namespace
----------------------------
Create a resource such as a service, deployment, job, or namespace using the kubectl create command.

For example, to create a new namespace, type:
# kubectl create namespace [namespace-name]

Apply this configuration using:
# kubectl create -f namespace-dev.yaml

To switch the namespace the command is or set default namespace
# kubectl config set-context --current --namespace=<namespacename>

Unset Default Namespace:
# kubectl config set-context --current --namespace=""

To see the list of namespaces
# kubectl get namespaces

To describe namespaces
# kubectl describe namespace <namespace_name> or kubectl describe namespace -n <namespace_name>

To delete namespace
# kubectl delete namespace <namespace_name>

to apply resource in specific namespace we use
# kubectl apply -f <yamlfilename> -n <namespacename>  

To see the workloads like pods,replicas,deployment,statefulset and services
# kubectl get pod -n <namespacename>
# kubectl get replicaset -n <namespacename>
# kubectl get deployment -n <namespacename>

View Resources Across All Namespaces:
# kubectl get pods --all-namespaces
# kubectl get deployments --all-namespaces
# kubectl get services --all-namespaces
# and so on...

Apply Configuration in a Namespace:
# kubectl apply -f <config_file.yaml> --namespace=<namespace_name>

Execute Command in a Namespace:
# kubectl exec -it <pod_name> --namespace=<namespace_name> -- /bin/bash






---------------------------------------
Applying and Updating a Resource
--------------------------------------
To apply or update a resource use the kubectl apply command. The source in this operation can be either a file or the standard input (stdin).

Create a new service with the definition contained in a [service-name].yaml file:

# kubectl apply -f [service-name].yaml

Create a new replication controller with the definition contained in a [controller-name].yaml file:

# kubectl apply -f [controller-name].yaml

Create the objects defined in any .yaml, .yml, or .json file in a directory:

# kubectl apply -f [directory-name]

You can update a resource by configuring it in a text editor, using the kubectl edit command. This command is a combination of kubectl get and kubectl apply.

For example, to edit a service, type:

# kubectl edit svc/[service-name]

This command opens the file in your default editor. To use a different editor, specify it in front of the command:

# KUBE_EDITOR=”[editor-name]” kubectl edit svc/[service-name]
-----------------------------------------------------------------
Displaying the State of Resources
-----------------------------------------------------------------
To display the state of any number of resources in detail, use the kubectl describe command. By default, the output also lists uninitialized resources.

View details about a particular node:

# kubectl describe nodes [node-name]

View details about a particular pod:
# kubectl describe pods [pod-name]

Display details about a pod whose name and type are listed in pod.json:

# kubectl describe -f pod.json

See details about all pods managed by a specific replication controller:

# kubectl describe pods [replication-controller-name]

Show details about all pods:

# kubectl describe pods
---------------------------------------
Deleting Resources
---------------------------------------
To remove resources from a file or stdin, use the kubectl delete command.

Remove a pod using the name and type listed in pod.yaml:

# kubectl delete -f pod.yaml

Remove all pods and services with a specific label:

# kubectl delete pods,services -l [label-key]=[label-value]

Remove all pods (including uninitialized pods):

# kubectl delete pods --all
-----------------------------------
Executing a Command
----------------------------------
Use kubectl exec to issue commands in a container or to open a shell in a container.

Receive output from a command run on the first container in a pod:

# kubectl exec [pod-name] -- [command]

Get output from a command run on a specific container in a pod:

# kubectl exec [pod-name] -c [container-name] -- [command]

Run /bin/bash from a specific pod. The received output comes from the first container:

# kubectl exec -ti [pod-name] -- /bin/bash
------------------------------------------
Modifying kubeconfig Files
------------------------------------------
kubectl config lets you view and modify kubeconfig files. This command is usually followed by another sub-command.

Display the current context:

kubectl config current-context

Set a cluster entry in kubeconfig:

# kubectl config set-cluster [cluster-name] --server=[server-name]

Unset an entry in kubeconfig:

# kubectl config unset [property-name]
--------------------------------------
Printing Container Logs
------------------------------------
To print logs from containers in a pod, use the kubectl logs command.

Print logs:

# kubectl logs [pod-name]

To stream logs from a pod, use:

# kubectl logs -f [pod-name]
------------------------------
Short Names for Resource Types
-------------------------------
Some of the kubectl commands listed above may seem inconvenient due to their length. For this reason names of common kubectl resource types also have shorter versions.

Consider the command mentioned above:

# kubectl create namespace [namespace-name]

You can also run this command as:

# kubectl create ns [namespace-name]
-------------------------------------
Certainly! In Kubernetes, labels are key-value pairs attached to objects like Pods, Services, and Nodes. They allow you to organize and select subsets of objects. Here’s how you can list labels for different types of resources:
--------------------------------------
Pods: To list all labels for pods, use the following command:
# kubectl get pods --show-labels

Nodes: To see labels for all nodes, run:
# kubectl get nodes --show-labels
To see the label of particular pod 
# kubectl get pod <podname> --show-labels
To see the perticular node lables
# kubectl get node <nodename> --show-labels
To see the lable assigned to the number of nodes
# kubectl get node -l <nameofthelabel>
To add the labels to the node 
}
