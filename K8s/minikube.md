# minikube



```
minikube start --driver=hyperv 
```

Point minikube to the docker daemon

```bash
# Get the command
minikube docker-env --shell=powershell

# The actual command
& minikube -p minikube docker-env --shell=powershell | Invoke-Expression
```



minikube status



```bash
minikube docker-env

# To point your shell to minikube's docker-daemon, run:
eval $(minikube -p minikube docker-env)
```





```bash
minikube addons enable dashboard

```

