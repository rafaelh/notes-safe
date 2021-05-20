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

