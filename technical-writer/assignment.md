# Debug Operations in Kubernetes

[Kubernetes](https://kubernetes.io/docs/concepts/overview/) contains several commands, sometimes we can use these commands to do things. 
In Azure, kubernetess is available, just like other cloud providers. 
Speaking of commands, [kubectl](https://kubernetes.io/docs/tasks/tools/#kubectl) is the CLI that is used to interact with k8s. The kubectl cli commmunicates with the kubernettes API server.
It has many commands.
See [a complete list of Kubernetes commands](https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#-strong-getting-started-strong-)

## `get pods`

A good command to know is kubectl get pods which is used to get a list of all pods that are available and what their status is. Just rememember that when you use this command tat you may have to specify the `namespace`.

```shell
kubectl get pods --namespace 
```

## `logs`

Another command that is helpful is the kubectl logs command. This command is used to retrive the logs of a specific pod - do use this when you have to review logs or need to debug a container.

## `exec`

Another we will dicuss is the `kubectl exec` command. A command that we can use to debug a container from the inside or to explore the the enviroment of the container itself. 

I recommend when debugging you start with kubectl get pods, then `kubectl logs` and lastly we can use `kubectl exec` to explore the inside of the container and review other log files or configurations. 

## `debug`

**Note:** The command `kubectl debug` is another option to considering when debugging a container. This command can be used to create a clone of a pod that does not terminate if an error is experienced inside the container. 
