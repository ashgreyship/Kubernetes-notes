# 服务\(Service\)

![](../.gitbook/assets/image%20%285%29.png)

`Service` 允许用户动态访问一组pods副本。由于pods 副本经常被销毁和创建，service 在这些pods副本上创建了一个抽象层 \(`abstraction layer`\) ，从而可以继续负载均衡。用户因此可以访问service，然后service将用户的请求转发给正在运行的 pods 副本。

## 例子:

### 创建一个新的service

```yaml
cat << EOF | kubectl create -f -
kind: Service
# 创建 service
apiVersion: v1
metadata:
  name: nginx-service
  # service name
spec:
  selector:
    app: nginx
  # selector 定义了service 转发流量的目的地
  ports:
  - protocol: TCP
    port: 80
    # pod 的端口
    targetPort: 80
    nodePort: 30080
    # 监听master 和 worker的端口
  type: NodePort
  # 暴露端口给外网
EOF
```

### 获取所有service

```yaml
kubectl get service
```

### 尝试连接端口 30080

```text
curl localhost:30080
```





