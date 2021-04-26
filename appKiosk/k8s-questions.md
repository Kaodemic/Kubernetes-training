## which is two major player in k8s?

- [ ] A. Master / Node
- [ ] B. Node / Pod
- [ ] C. Pod / Container

> Ans: (A)
- `Masters`: The Master is the heart of Kubernetes, which controls and schedules
all the activities in the cluster
- `Nodes`: Nodes are the workers that run our containers

# Kubelet
Kubelet is a major process in the nodes, which reports node activities back to kubeapiserver periodically, such as pod health, node health, and liveness probe. As the preceding graph shows, it runs containers via container runtimes, such as Docker or rkt

# Proxy (kube-proxy)
Proxy handles the routing between pod load balancer (a.k.a. service) and pods, it
also provides the routing from outside to service. There are two proxy modes,
userspace and iptables. Userspace mode creates large overhead by switching kernel
space and user space. Iptables mode, on the other hand, is the latest default proxy
mode. It changes iptables NAT in Linux to achieve routing TCP and UDP packets
across all containers

# Interaction between Kubernetes master and nodes
![./Interaction-k8s-node.png](kube)

# K8s command 
- kubectl help
- kubectl options
- kubectl explain <resource> to get the detailed description for the
resource by command line. 

# when we manually create a pod with same labels with RC is ready all?
```
NAME          READY   STATUS        RESTARTS   AGE
example       2/2     Running       0          75m
nginx-bsl6m   1/1     Running       0          4m5s
nginx-plsdt   1/1     Running       0          4m5s
our-nginx     0/1     Terminating   0          4s
```
