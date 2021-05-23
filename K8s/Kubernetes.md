# Kubernetes
[TOC]

https://kubernetes.io/docs/home/



## Setup

Before this meeting please make sure you have on your machine:
* Git setup so that you can clone repositories from GitHub
* Helm installed - https://helm.sh/docs/intro/install/
* Azure Command Line installed, and logged in - https://docs.microsoft.com/en-us/cli/azure/install-azure-cli
* Install kubectl (https://docs.microsoft.com/en-us/cli/azure/aks?view=azure-cli-latest#az_aks_install_cli)





# Pod

Example pod yaml:

```yaml
apiVersion: v1
kind: Pod
metadata:
	name: example-pod
	labels:
		app: web
	spec:
		containers:
		- name: app
		  image: nginx
```



Retrieve the details of a pod with

```bash
kubectl get pod app -o yaml
```



Create a service object:

```bash
kubectl expose pod podnamegoeshere
```

Services are similar to load balancers and are used to distribute traffic to pods. You can show the service details with:

```bash
kubectl get service app -o yaml
```



kubectl explain pod.spec.containers



kubectl get ingress app

kubectl describe ingress

kubectl get pods --watch

Don't recommend setting CPU limits, they are hard to get right



## Deployment Strategies

There are two independent probes that monitor pods: 

* **Readiness probe** - returns a 200. Only used when bringing it into the service. In charge of deciding if the container can recieve traffic. If it starts throwing a 500, it will be removed from the service/load balancer. So a container that isn't ready because it isn't starting, isn't regarded as ready, so the service won't send traffic to it. Once it is started, the readiness probe will succeed, and the service will send traffic to it.

* **Liveness probe** - process started, process healthy. If this probe fails, the container will be restarted inside the pod.

With this in  mind, you set an initial delay before checking starts (so that any setup can complete) then you set a schedule to check ongoing - `timeoutSeconds`, `successThreshold`, `failureThrehold`.

### Selectors
Kubernetes resources can have any number of tags, such as `component: Backend` or `name: AppName` or `version: 1.0.0`. Selectors allow you to select a subset of resources with multiple tags.

### Blue-green deployment
This is where you create a copy of the current infrastructure (with the new version) then swap over to it. You use a selector to switch between the old and new copies of the infrastructure.

### ReplicaSet
Replicas are a subset of deployment, and create the replicas. A ReplicaSet can only hold a single type of pod. So the deployment handles a rolling update between two versions, but the ReplicaSets look after the numbers of pods of a single version. The deployment increases and decreses the numbers of pods inside the two ReplicaSets to switch over the pods to a new version. Up to 10 old ReplicaSets stay around, even when empty, so that you can rollback.

```bash
# Example of rolling back
kubectl rollout undo deployment app --to-revision=2
```

Initially, a deployment and a replicaset look the same. When you update a deployment, it creates a new replicaset definition, and begins swapping pods to the new one. You can see the replica sets with:

```bash
kubectl get replicasets
```






```bash
Your Kubernetes control-plane has initialized successfully!

To start using your cluster, you need to run the following as a regular user:

  mkdir -p $HOME/.kube
  sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
  sudo chown $(id -u):$(id -g) $HOME/.kube/config

Alternatively, if you are the root user, you can run:

  export KUBECONFIG=/etc/kubernetes/admin.conf

You should now deploy a pod network to the cluster.
Run "kubectl apply -f [podnetwork].yaml" with one of the options listed at:
  https://kubernetes.io/docs/concepts/cluster-administration/addons/

Then you can join any number of worker nodes by running the following on each as root:

sudo kubeadm join 10.0.0.10:6443 --token lffshc.77kk8yv4rww3h043 \
    --discovery-token-ca-cert-hash sha256:d88e2b0d3e32e6d229f431f32d198eb2bc65abf64723d00885aa4216fc62de09
```



kubectl port-forward service/hello 8080:80

This is useful to debug pod -> service issues, skipping the ingress.



Managed services set some of the flags for kubernetes, such as the time before moving pods (5 minutes). This is where it occasionally makes sense to build your own cluster, since that enables you to set some of these flags.
