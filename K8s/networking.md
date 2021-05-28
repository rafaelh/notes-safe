# K8s Networking

[TOC]

## Cluster Networking
Rules of kubernetes networking:
* Any pod can talk to any pod
* Agents on a node can communicate with all pods on that node (eg Kubelet)

If you are designing a larger cluster, that needs to cross several physical networks, there are plugins such as **flannel** or **weavenet**, which are ***network plugins***. Network plugins are Software Defined Networking (SDN), and can provide functions like encryption.

**Flannel** is deployed as a daemonset, that provisions up a pod inside each node. Flannel encapsulates each packet and addresses communications accordingly.

`podIP` is the IP address for the pod. It is stored in etcd.

Other components:
* (**CRI**) Container Runtime Interface - usually docker or containerd
* (**CNI**) Container Network Interface - Attaches the container to a network - Flannel, Cilium, etc
* (**CSI**) Container Storage Interface

## Internal Traffic
The endpoint object in etcd is a dynamic collection of pod IP addresses and ports (so, endpoints). You can check this with:

```bash
kubectl get endpoints

# Also interesting (shows kube-proxy):
kubectl get daemonset --all-namespaces
```

Services don't actually exist. They are a construct inside the database used to configure traffic. There are four types of these contstruct, which encapsulate each other (headless -> LoadBalancer at the top level):

* **Headless** - (Uses **CoreDNS**) No IP address, client-side (manual) load balancing. CoreDNS comes with most clusters (runs as a kube-system pod), and manages this as something like `service.namespace.svc.cluster.local`. On requesting the IP from the service, coredns gives you the IP addresses of the pods in it. This can be a problem if you cache DNS, since CoreDNS might change its entries. Good for things like scaling databases and other persistent connections.
* **ClusterIP** - (Uses **kube-proxy**) Includes headless, but has a fake IP address (not routable) which is used as a traffic label. Kube-proxy then intercepts the traffic and rewrites it on the fly to go to the correct pod. Kube-proxy does this by writing iptables rules on each node. If a new node is created, kube-proxy is notified, and updates the iptables rules on the new node. Round-robin style distribution of traffic.
* **NodePort** - (Uses **kube-proxy**) You get a fake IP address, and port in the range of 30000, which is exposed on every node. If the target pod isn't spun up on a node, the iptables rules that kube-proxy set will forward traffic from the empty node to a node which does have the pod. Used for exposing to the outside world without an ingress (or *for* and ingress with multiple pods).
* **LoadBalancer** - A combination of all the previous services, which also automatically provisions a cloud load balancer (only works on managed services). Creates one load balancer per service, which is super-expensive.

### Ingress
An ingress is just a pod and service working to rewrite internal traffic.

