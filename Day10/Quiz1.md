#Kubernetes

### 1 . What is a Kubernetes Pod?
- ✅ A collection of containers that share network and storage resources
- ❌ A virtual machine in Kubernetes
- ❌ A database service in Kubernetes
- ❌ A security mechanism for running containers

### 2 . What is the function of etcd in Kubernetes?
- ❌ It schedules workload accross nodes
- ✅ It acts as the primary database for cluster state information
- ❌ It runs application containers
- ❌ It balances network traffic.

### 3 . What does kube proxy does in Kubernetes?
- ❌ It provides persistent storage for pods
- ❌ It stores all cluster information
- ✅ It manages networking rules and service discovery
- ❌ It schedules pods on nodes

### 4 . Which component ensures that the desited state of the cluster matches the actual state?
- ❌ API Server
- ❌ Kubelet
- ✅ Controller Manager
- ❌ Kube Proxy

### 5 . How do Worker Nodes communicate with the Kubernetes Control Plane?
- ❌ Through etcd directly
- ✅ Using REST API calls via the API server
- ❌ By writing logs to the disk 
- ❌ Through SSH connections

### 6 . What is the primary purpose of Kubernetes?
- ❌ Replacing virtual machines
- ❌ Providing a programming language for cloud applications
- ✅ Automating the deployment, scaling and management of containerized applications
- ❌ Managing databases

### 7 . Which of the following is not a core component of the Kubernetes Master node?
- ✅ Kubelet
- ❌ etcd
- ❌ Controller Manager
- ❌ API Server

### 8 . Which of the following best describes the role of the Kubelet?
- ❌ It stores cluster configuration data
- ✅ It ensures that containers are running on a node and reports status to the control plane
- ❌ If schedules workloads to different nodes
- ❌ It turns on the master node to manage API requests

### 9 . What component of Kubernetes is responsible for scheduling Pods on nodes?
- ❌ API Server
- ✅ Kubernetes Scheduler
- ❌ Controller Manager
- ❌ Kubelet

### 10 . What is the function of the kubernetes API Server?
- ❌ It stores the cluster state persistently
- ❌ It balances network traffic
- ❌ It runs application workloads
- ✅ It provides an interface for managing the cluster