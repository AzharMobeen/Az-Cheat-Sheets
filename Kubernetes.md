### Kubernetes:
* It's a tool to manage multiple containers.
* Main responsibilities towards applications are:
    - High Availability (Means application don't have downtime)
    - Scalibility or high performance
    - Disaster Recovery (Backup and restore)
* K8s have at least one Master Node and multiple worker nodes
#### Kubernetes Architecture:
* It have two types of nodes.
##### Master Node:
* It have 4 Processes:
    -   API Server (all the frontend UI, API, CLI will communicate through this)
    -   Controller Manager ()
    -   Scheduler ()
    -   eted (it's a key value storae to have kubernetes backup)
##### Worker Node:
* Each Node have multiple running pods and each pod can have multiple containers.
* It have 3 processes:
    -   Container Runtime
        -   As we need to run multiple containers (inside pod) so We need to install container runtime (Docker) on each node.)
    -   Kubelet:
        -   It's a process which actually interact container runtime and node.
        -   It's start the pod with the container inside and assign resources from the nodes to the container.
    -   Kube Proxy:
        -   For the communication b/w pods for example backend api to DB call handled through kube proxy.
        -   It will reduce the complexity for multiple replicas it's will forward the request with in same node if available.

#### K8s Components:

##### Config Map:
* External Configurations/runtime arguments of application.
##### Secret:
* It's an other map to store password in base64 encoded format.
* So it's contain credentials 

##### Ingres:
* For public IP's or url first request will go to ingress
* Then it will go to service.

##### Service:
* It's having two funtionality:
    -   Permanent IP    (if pods restarted/created again other api will not face any issue)
    -   Load balancer   (If one pods goes down request automatically will be redirected to an other pod)
* In K8s we have two type of services 
    -   External Service
    -   Internal Service

##### Deployment:
* Whenever pod goes down another pod or replica of pod will be up this is handled by deployment.

##### StatefulSet:
* For Statful applications or specailly DBs also need replica will be up if DB pod goes down and this is handled by statefulSet.
* This is required for Stateful app becauses stateful apps need to be persist data through VOLUME. 

##### Volume:
* If Database pods get restart then data will be gone.
* For that we need to use Volumes to persist data for long time.
* It basically attached local/physical/cloud storage of hardrive to DB pod.
--------------------------------------------------------------------------------
#### Production Cluster Setup
##### For test/Local Cluster setup:
* K8s needs RAM and processors to setup it's hard to setup locally for that we should use `minikube`
###### Minikube:
* It's a one node k8s cluster which have master and worker nodes processes on one machine.
* It also have docker container runtime pre installed.
* [For Minikube installation](https://minikube.sigs.k8s.io/docs/start/)
###### Kubectl:
* As above mentioned any type of interaction through UI, API or CLI (API SERVER from Master node) will handle.
* It's a most powerful command line tool which helps us to interact with k8s cluster.
* [For Kubectl installation](https://kubernetes.io/docs/tasks/tools/install-kubectl-linux/)
###### Kubectl commands:

create components (-h will provide help):
```
kubectl create -h
```
####### Create deployment/Pod:
* we can't create pod directly, we can create only with the help of DEPLOYMENT component
######## CRUD Commands
```
kubectl create deployment deployment-name --image=dockerimage 
```
```
kubectl create deployment nginx-depl --image=nginx
```
```
kubectl edit deployment nginx-depl
```
* let's edit image version to 1.16 and save file.

```
kubectl delete deployment mongo-depl
```
* After above command both deployment and pod will be deleted as well.

######## Status Commands for K8s components:
```
kubectl get pod
```
```
kubectl get services
```
```
kubectl get deployment
```

######## Debuging Commands:
* We can check logs of any deployment by
```
kubectl logs pod-name
```
```
 kubectl exec -it  mongo-depl-5fd6b7d4b4-6w42v -- bin/bash
```
```
kubectl describe pod mongo-depl
```
- - - -
###### Deployments through yaml files:
* It's really hard to do deployment through command line parameter wise.
* It's very easy to handle via yaml files
```
kubectl apply -f deployment-config.yaml
```
* Example:
    -   First we need to create *.yaml file then edit it and the apply command.
create file
```
touch nginx-deployment.yaml
```
edit file
```
vim nginx-deployment.yaml
```
######## Configuration Commands:
```
kubectl apply -f nginx-deployment.yaml
```
```
kubectl delete -f nginx-deployment.yaml
```
- - - -
##### Configuration file details:
* All the components have 5 parts
    - ApiVersion    | version of the app
    - Kind          | it can be deployment/service
    - MetaData      | name of the application and labels
    - Spec          | it depends upon KIND.
    - Status (manae by K8s through etcd process in master node)
