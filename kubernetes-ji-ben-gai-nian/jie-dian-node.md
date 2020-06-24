# 节点\(Node\)

![](../.gitbook/assets/image%20%283%29.png)

一个Kubernetes 集群有一个或者多个控制服务器 \(control servers\). 他们用于管理和控制集群以及 Kubernetes API. 

一个Kubernetes 集群也有一个或者多个工作节点 \(`worker`\). 他们主要用于运行应用程序。

## 例子：

#### 获取节点列表:

```text
kubectl get nodes
```

获取一个node的详细信息

```text
kubectl describe node $node_name
```





