# End-to-end tests

## Exam Curriculum

1. Run end-to-end tests on your cluster.
2. Analyze end-to-end tests results.
3. Run Node end-to-end tests.

## Definition

Running end-to-end tests ensures your application will run efficiently without having to worry about cluster health problems.

## Examples

### Run end-to-end tests on your cluster.

Run a simple nginx deployment:

```text
kubectl run nginx --image=nginx
```

View the deployments in your cluster:

```text
kubectl get deployments
```

View the pods in the cluster:

```text
kubectl get pods
```

Use port forwarding to access a pod directly:

```text
kubectl port-forward $pod_name 8081:80
```

Get a response from the nginx pod directly:

```text
curl --head http://127.0.0.1:8081
```

View the logs from a pod:

```text
kubectl logs $pod_name
```

Run a command directly from the container:

```text
kubectl exec -it $pod_name -- nginx -v
```

### Run Node end-to-end tests.

Create a service by exposing port 80 of the nginx deployment:

```text
kubectl expose deployment nginx --port 80 --type NodePort
```

List the services in your cluster, get `node_port` which is used by `service` to expose nginx `deployment`

```text
kubectl get services
```

Login worker nodes and get a response from the service:

```text
curl -I localhost:$node_port
```

### Analyze end-to-end tests results.

List the nodes' status:

```text
kubectl get nodes
```

View detailed information about the nodes:

```text
kubectl describe nodes
```

View detailed information about the pods:

```text
kubectl describe pods
```

