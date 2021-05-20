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

