### Why we need Kubernetes (K8s):
##### Traditional Deployment:
* In traditional deployments we were deploying multiple apps on the server and we can't manage resources WRT app.
* Similarly we can't mange do scale up and scale down dynamically WRT to our app requirements/need.
![Traditional Deployments](/Traditional-Deployment.jpg)
##### Virtualized Deployment:
* To overcome this traditional deployment issue `Virtualized Deployment` came.
* In this we can better use of our server resources but alot of layers added and alot of resource will be used by virtualized OS.
* Similarly we can't mange do scale up and scale down dynamically WRT to our app requirements/need.
![Virtualized Deployment](/Virtualized-Deployment.jpg)
##### Container Deployment:
* To Over come above deployment issues container deployment came. In this we can utilize much better resources.
* We can handle container deployment on both server & cloud environment.
* But it's very difficult to handle multiple containers and their life cycles so we need a tool to handle our containers so kubernetes come for the rescue.
![Container Deployment](/Container-Deployment.jpg)
### Kubernetes (K8s):
* Kubernetes is a containers orchestration tool and developed by google.
* Main responsibilities towards applications are:
    - High Availability (Means application don't have downtime)
    - Scalibility or high performance
    - Disaster Recovery (Backup and restore)
* K8s have at least one Master Node and multiple worker nodes
#### Kubernetes Architecture:
![Architecture](/Kubernetes-Architecture.jpg)
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
##### Pod:
* Smallest unit of K8s is pod.
* Our Appliations are deployed as a container and Pod is a abstraction of container.
* 1 Pod will contain 1 application.
* Each pod will have connected with it's internal IP address.

#### K8s Components: [Helping tutorial](https://www.youtube.com/watch?v=ivDzqrCfC2Y)

##### Service (svc):
* It's a abstract layer of Pod.
* We need to create service component for each pod because whenever pods are create or after destory again created it will have new intenal IP address.
* For exposing Pod we need Service component so that pod will be accible even after destory cycle. Because other application will access pod with Service endpoint not WRT IP.
* Both Service and pod have it's own lifecycle even if pod destory service remain there. 
* It's having two funtionality:
    -   Permanent IP    (if pods restarted/created again other api will not face any issue)
    -   By default all service components are Load balanced (If one pods goes down request automatically will be redirected to an other pod)
* In K8s we have two type of services 
    -   External Service (Accible outside k8s cluster like Rest APP pod)
    -   Internal Service (Accible only within k8s cluster like DB pod)
* Types:
    -   ClusterIP : `It's default type and It's accessible only within cluster level. No External trafic can communicate with this service.
    -   NodePort  : `It's a extension of ClusterIP and it's allow external trafic to communicate with pod but with fixed port, it's not secure becuase we are opening service communication for externals. NodePort we need to mention in the service.yml and it's range is 30000 to 32767.
    -   LoadBalancer: `It's a extension of NodePort and it's give better option to allow external trafic to access servic. When we create LoadBalancer service type then ClusterIP & NodePort Services will be created automatically. It's for native cloud platforms.
##### Ingres (ing):
* It's a abstract layer for service component, to resolve domain name into IP
* For public IP's or domain name, first request will go to ingress component.
* Then it will go to service and then after to Pod (application).
##### ConfigMap (cm):
* External Configurations/runtime arguments of application will be store in configmap.
* So that we can avoid redeployments of application if any configuration changed
##### Secret:
* It's an other map to store private credentials in base64 encoded format.
##### Volume (vol):
* If Database pods get restart or destored and created again in this case data will be lost.
* For that we need to use Volumes to persist data for long time.
* It basically attached local/physical/cloud storage of hardrive for DB pod.
##### Deployment:
* As Pod is a abstraction of container same way deployment is a abstraction for entire application.
* Whenever pod goes down another pod or replica of pod will be up this is handled by deployment.
* In short to achieve application high avaiability & scallibility we need deployment component.
* We can have all the configurations (svc, ing, vol, pod, ...) in deployment component.

##### StatefulSet:
* For Statful applications or specailly DBs also need replica will be up if DB pod goes down and this is handled by statefulSet.
* This is required for Stateful app becauses stateful apps need to be persist data through VOLUME.
* Specially DB data should be sync with other relicas.

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
####### CRUD Commands
```
kubectl get nodes
```
```
kubectl cluster-info
```
```
kubectl get all
```
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

####### Status Commands for K8s components:
```
kubectl get pod
```
```
kubectl get services
```
```
kubectl get deployment
```

####### Debuging Commands:
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
####### Deployments through yaml files:
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
####### Configuration Commands:
```
kubectl apply -f nginx-deployment.yaml
```
```
kubectl delete -f nginx-deployment.yaml
```
####### It will deploy all the configuration files
```
kubectl apply -f ./
```
####### Check cluster pod is listning on provided port, after below command it will display listning port
```
kubectl get pod pod-name --template='{{(index (index .spec.containers 0).ports 0).containerPort}}'
```
####### Port forwarding to access pod from outside of cluster, `above command will share listning port`
```
kubectl port-forward pod-name 8080:8080
```
- - - -
##### Configuration file details:
* All the components have 5 parts
    - ApiVersion    | version of the app
    - Kind          | it can be deployment/service
    - MetaData      | name of the application and labels
    - Spec          | it depends upon KIND.
    - Status (manae by K8s through etcd process in master node)
