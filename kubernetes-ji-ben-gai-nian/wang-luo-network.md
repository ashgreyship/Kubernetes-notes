# 网络 \(Network\)

集群内的每一个`pod` 都有独立的 IP. 并且可以和集群内的其他`pod`通信。

![](../.gitbook/assets/image%20%284%29.png)

## 例子：

1. 创建一个带有两个`nginx` `pods` 的 `deployment`

```yaml
cat << EOF | kubectl create -f -
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx
  labels:
    app: nginx
spec:
  replicas: 2
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:1.15.4
        ports:
        - containerPort: 80
EOF
```

2. 创建一个 `busybox` `pod`

```yaml
cat << EOF | kubectl create -f -
apiVersion: v1
kind: Pod
metadata:
  name: busybox
spec:
  containers:
  - name: busybox
    image: radial/busyboxplus:curl
    args:
    - sleep
    - "1000"
EOF
```

3. 获取所有 pods 的IP

```yaml
kubectl get pods -o wide
```

4. 在得到 `Nginx` `pods`的 IP 之后，运行`busybox` `pod`，使其连接一个`Nginx` `pod`。发现`busybox pod` 可以成功连接 `Nginx pod`。

```yaml
kubectl exec busybox -- curl $nginx_pod_ip
```



