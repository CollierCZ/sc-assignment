# Debug Operations in Kubernetes

When monitoring containers on [Kubernetes](https://kubernetes.io/docs/concepts/overview/),
you may need to debug those containers to troubleshoot an issue.
To interact with a container, use the [kubectl command-line tool](https://kubernetes.io/docs/tasks/tools/#kubectl).
This tool offers a way to communicate with Kubernettes clusters using the API.
To use the commands, first [install kubectl](https://kubernetes.io/docs/tasks/tools/#kubectl).

This document offers an overview of some of the most useful commands for debugging your Kubernetes containers.
If you need more options, consult a [complete list of Kubernetes commands](https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#-strong-getting-started-strong-).

## `get pods`

Use the `get pods` command to get a list of all pods that are available and what their status is:

```shell
kubectl get pods
```

This returns a list of pods with their status:

```shell
NAME            READY   STATUS    RESTARTS   AGE
example-abc12   0/1     Failed    1          12m
example-def34   1/1     Running   0          34m
example-ghi56   1/1     Running   0          56m
```

This defaults to the current namespace.
To specify the namespace from which to return pods, use the `namespace` flag:

```shell
kubectl get pods --namespace <POD_NAMESPACE>
```

Replace `<POD_NAMESPACE>` with the namespace from which you want a list of pods.

To get pods from all namespaces, use the `all-namespaces` flag:

```shell
kubectl get pods --all-namespaces
```

## `logs`

If you have a specific pod you want to investigate further,
use the `logs` command to retrive the logs of a specific pod:

```shell
kubectl logs <POD_NAME>
```

Replace `<POD_NAME>` with the pod you want logs from, such as this example:

```shell
kubectl logs example-abc12
```

You get a list of all logs from the given container:

```shell
2023/11/20 15:55:55 [notice] 1#1: start worker processes
2023/11/20 15:55:55 [notice] 1#1: start worker process 42
2023/11/20 15:55:55 [notice] 1#1: start worker process 43
2023/11/20 15:55:55 [notice] 1#1: start worker process 44
2023/11/20 15:55:55 [notice] 1#1: start worker process 45
2023/11/20 15:55:55 [notice] 1#1: start worker process 46
2023/11/20 15:55:55 [notice] 1#1: start worker process 47
```

If there are too many logs, use the `tail` flag to limit the number you get:

```shell
kubectl logs example-abc12 --tail 3
```

You get the most recent logs:

```shell
2023/11/20 15:55:55 [notice] 1#1: start worker process 45
2023/11/20 15:55:55 [notice] 1#1: start worker process 46
2023/11/20 15:55:55 [notice] 1#1: start worker process 47
```

If the pod has multiple containers, use the `container` flag to specify the container:

```shell
kubectl logs <POD_NAME> --container <CONTAINER_NAME>
```

Replace both `<POD_NAME>` and `<CONTAINER_NAME>` with the names you want.

To get logs from all containers, set the `all-containers` flag to `true`:

```shell
kubectl logs <POD_NAME> --all-containers true
```

## `exec`

To debug a container from the inside or explore the enviroment of the container itself,
use the ` exec` command:

```shell
kubectl exec <POD_NAME> -- <COMMAND>
```

Replace `<POD_NAME>` with the pod where you want issue commands and `<COMMAND>` with the commands to issue,
such as this example:

```shell
kubectl exec example-abc12 -- date && time
```

You get the result of the commands you specified as returned from the container,
in this example the date and the time spent executing the `time` command.

If the pod has multiple containers, use the `container` flag to specify the container:

```shell
kubectl exec <POD_NAME> --container <CONTAINER_NAME> -- <COMMAND>
```

Replace both `<POD_NAME>` and `<CONTAINER_NAME>` with the names you want
and `<COMMAND>` with the commands to issue.

## `debug`

To create an interactive container for debugging, use the `debug` command:

```shell
kubectl debug <POD_NAME>
```

Replace `<POD_NAME>` with the pod where you want the debugging container,
such as this example:

```shell
kubectl debug example-abc12
```

This creates an ephemeral container inside the pod you specify
so you can inspect the environment and troubleshoot any issues in your containers.

To create an interactive shell within the new container and then connect your terminal to it,
use the `stdin` and `tty` flags.
Pass these flags together using the `it` shorthand:

```shell
kubectl debug <POD_NAME> -it
```

This creates the container and attaches your terminal to it:

```shell
Defaulting debug container name to debugger-abc12.
If you don't see a command prompt, try pressing enter.
/ #
```
