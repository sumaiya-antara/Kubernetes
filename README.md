# Kubernetes

### There are two planes in Kubernetes architecture: 
1. Control Plane 
2. Data Plane. 

![alt text](https://github.com/sumaiya-antara/Kubernetes/blob/master/Kubernetes.png "kubernetes architecture")


## Pods:
In kubernetes architecture, the containers remain in **pod**. In each instance/server/node, one or multiple pods can reside. Each pod can contain a collection of containers or a single container.

![alt text](https://github.com/sumaiya-antara/Kubernetes/blob/master/Pods.png "pods")

## Master Nodes and Worker Nodes:
The Pods are controlled by the **Master Node** in the control plane. The pods remain in the **Worker Nodes** in the data plane.

## API Server:
**API Server** is the heart of Master Node and it is the bridge. Every action and communication must be done through the API Server. The API Server in Master Node communicates with the nodes/servers for managing the containers.

## kubectl:
There must be a terminal or GUI through which we can access the API Server and give necessary commands or directions. A client called **kubectl** is the command line tool for providing commands in the API Server.

That means, if we want to know an update status of the residing containers(in pods of a server), we have to type commands in kubectl and the API Server will then communicate with the nodes in data plane.

## Kubelet:
**Kubelet** is a component of kubernetes which runs on every nodes. It is basically a tool. The API Server communicates actually with the kubelet in worker nodes. Kubelet is a client and it gets the container information/status/health from **Docker dd**. 

Docker dd provides the container health status to kubelet and then kubelet send this update to API Server in Master Node. The status is then shown in kubectl.

## ETCD and how it works:
This above whole process takes almost 1s for each communication with a single container (sending the command and getting result). If there are 50 containers, it would take 50*1s = 50s to get response for a single command.

For solving this issue and reducing the runtime, **ETCD** is placed in Master Node. ETCD is a key-value store which contains the cache and health updates of the containers in worker nodes. 

The kubelet continuously sends the container health status to the ETCD in every 5s and ETCD stores them in key-value pair form. Now, the process time becomes less. We just have to type commands in kubectl, then the API Server will process this command and communicate with ETCD and get the container information and show it in kubectl. Here, the kubelet ahs to to send data/update to ETCD via API Server.

## Use of Scheduler:
There is also a **Scheduler** in the Master Node. Scheduler assigns the pods with nodes. When new pods are created, scheduler finds the best nodes for them. Scheduler gets this data from ETCD and also from worker nodes via API Server. Scheduler then decides the best nodes and send this decision to API Server and API Server work according to that decision. So, scheduler is the decision maker or the brain of master node. 

### Note:
Every communication or work must be done via API Server. In every communcation path, API Server must exist in every traffic. 