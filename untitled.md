# Kubectl

`Kubectl CLI` 客户端用于Kubernete进行交互。

## 相关命令：

展示集群信息：

```text
kubectl cluster-info
```

列出集群节点：

```bash
kubectl get nodes
```

```bash
kubectl get nodes -n <name-space>
```

查看对象的相关信息：

```bash
 kubectl describe node <node-name> 
```

以kubia镜像来部署一个应用程序

```bash
kubectl run kubia --image==blackbird698/kubia --port=8080 --generator=run/v1
#generator 让Kubernetes 创建一个ReplicationController
```





