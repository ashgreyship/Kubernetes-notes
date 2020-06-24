# Pod

一个`pod` 有一个或者多个容器。容器运行在同一个工作节点上，和同一个Linux命名空间。

每一个`pod`就像一个独立的逻辑机器，有自己的IP,主机名和进程。

![](../.gitbook/assets/image%20%281%29.png)

Kubernetes 规划 `pods` 在集群指定的容器运行。当`pods` 被部署后，Kubernetes 规划`pods` 中运行的容器。

## 例子：

#### 创建一个`pod`:

```yaml
cat << EOF | kubectl create -f -
apiVersion: v1
kind: Pod
metadata:
  name: nginx
spec:
  containers:
  - name: nginx
    image: nginx
EOF
```

#### 获取指定`pod`的详细信息：

```text
kubectl describe pod nginx
kubectl describe pod <node-name>

```

#### 删除`pod`

```text
kubectl delete pod nginx 
```



