# 将Pod 分配给节点

## nodeSelector

#### 当前pod 配置:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx
  labels:
    env: test
spec:
  containers:
  - name: nginx
    image: nginx
```

####  添加 nodeSelector:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx
  labels:
    env: test
spec:
  containers:
  - name: nginx
    image: nginx
    imagePullPolicy: IfNotPresent
  nodeSelector:
    disktype: ssd
```

#### 节点的标准标签：

* [`kubernetes.io/hostname`](https://kubernetes.io/zh/docs/reference/kubernetes-api/labels-annotations-taints/#kubernetes-io-hostname)
* [`failure-domain.beta.kubernetes.io/zone`](https://kubernetes.io/zh/docs/reference/kubernetes-api/labels-annotations-taints/#failure-domainbetakubernetesiozone)
* [`failure-domain.beta.kubernetes.io/region`](https://kubernetes.io/zh/docs/reference/kubernetes-api/labels-annotations-taints/#failure-domainbetakubernetesioregion)
* [`topology.kubernetes.io/zone`](https://kubernetes.io/zh/docs/reference/kubernetes-api/labels-annotations-taints/#topologykubernetesiozone)
* [`topology.kubernetes.io/region`](https://kubernetes.io/zh/docs/reference/kubernetes-api/labels-annotations-taints/#topologykubernetesiozone)
* [`beta.kubernetes.io/instance-type`](https://kubernetes.io/zh/docs/reference/kubernetes-api/labels-annotations-taints/#beta-kubernetes-io-instance-type)
* [`node.kubernetes.io/instance-type`](https://kubernetes.io/zh/docs/reference/kubernetes-api/labels-annotations-taints/#nodekubernetesioinstance-type)
* [`kubernetes.io/os`](https://kubernetes.io/zh/docs/reference/kubernetes-api/labels-annotations-taints/#kubernetes-io-os)
* [`kubernetes.io/arch`](https://kubernetes.io/zh/docs/reference/kubernetes-api/labels-annotations-taints/#kubernetes-io-arch)

## 亲和功能

### 节点亲和

* requiredDuringSchedulingIgnoredDuringExecution （将 pod 调度到一个节点上_必须_满足的规则）
* preferredDuringSchedulingIgnoredDuringExecution （尝试执行但不能保证的_偏好_）

#### 例子

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: with-node-affinity
spec:
  affinity:
    nodeAffinity:
      requiredDuringSchedulingIgnoredDuringExecution:
        nodeSelectorTerms:
        - matchExpressions:
          - key: kubernetes.io/e2e-az-name
            operator: In
            values:
            - e2e-az1
            - e2e-az2
      preferredDuringSchedulingIgnoredDuringExecution:
      - weight: 1
        preference:
          matchExpressions:
          - key: another-node-label-key
            operator: In
            values:
            - another-node-label-value
  containers:
  - name: with-node-affinity
    image: k8s.gcr.io/pause:2.0
```

#### 操作符：

 `In`，`NotIn`，`Exists`，`DoesNotExist`，`Gt`，`Lt`

{% hint style="warning" %}
* 如果你同时指定了 `nodeSelector` 和 `nodeAffinity`，_两者_必须都要满足，才能将 pod 调度到候选节点上。
* 如果你指定了多个与 `nodeAffinity` 类型关联的 `nodeSelectorTerms`，则**如果其中一个** `nodeSelectorTerms` 满足的话，pod将可以调度到节点上。
* 如果你指定了多个与 `nodeSelectorTerms` 关联的 `matchExpressions`，则**只有当所有** `matchExpressions` 满足的话，pod 才会可以调度到节点上。 
{% endhint %}

### pod 间亲和/反亲和

#### 规则的格式：

“如果 X 节点上已经运行了一个或多个 满足规则 Y 的pod，则这个 pod 应该（或者在非亲和的情况下不应该）运行在 X 节点”。

Y 表示一个具有可选的关联命令空间列表的 LabelSelector；



