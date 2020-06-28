# Lab 2

### 要求:

Your team manages an online storefront. They want to have a simple service in their Kubernetes cluster that is able to provide a list of products. Other pieces of the application, running as other pods in the cluster, will use this service in the future. For now, all you need to do is deploy the service's pods to the cluster and create a Kubernetes service to provide access to those pods. The team estimates that you will need four replicas of the service pod for the time being. There is already a busybox testing pod in the cluster that you can use to test your new service once it is created.

There is a public Docker image for the store-products app called `linuxacademycontent/store-products:1.0.0`.

You will need to do the following:

* Create a deployment for the store-products service with four replicas.
* Create a store-products service and verify that you can access it from the busybox testing pod.

### 为 store-products service 创建一个带有四个副本的`deployment`。

```yaml
cat <<EOF | kubectl create -f -
apiVersion: apps/v1
kind: Deployment
metadata:
  name: store-products
  labels:
    app: store-products
spec:
  replicas: 4
  selector:
    matchLabels:
      app: store-products
  template:
    metadata:
      labels:
        app: store-products
    spec:
      containers:
      - name: store-products
        image: linuxacademycontent/store-products:1.0.0
        ports:
        - containerPort: 80
EOF
```

### 创建一个 store-products `service`, 并验证`busybox testing pod` 可以访问它。

```yaml
cat << EOF | kubectl create -f -
kind: Service
# 创建 service
apiVersion: v1
metadata:
  name: store-products
  # service name
spec:
  selector:
    app: store-products
  # selector 定义了service 转发流量的目的地
  ports:
  - protocol: TCP
    port: 80
    # pod 的端口
    targetPort: 80
  type: NodePort
  # 暴露端口给外网
EOF
```

###  使用 `busybox pod` 连接 `store-product service`。

```
kubectl exec busybox -- curl -s store-products
```

