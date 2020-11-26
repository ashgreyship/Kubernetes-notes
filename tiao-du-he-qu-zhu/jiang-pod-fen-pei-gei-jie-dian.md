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

Y 表示一个具有可选的关联命令空间列表的 **LabelSelector**；

与节点不同，因为 pod 是命名空间限定的（因此 pod 上的标签也是命名空间限定的），因此作用于 pod 标签的标签选择器必须指定选择器应用在哪个命名空间。从概念上讲，**X 是一个拓扑域**，如节点，机架，云供应商地区，云供应商区域等。你可以使用 `topologyKey` 来表示它，`topologyKey` 是节点标签的键以便系统用来表示这样的拓扑域。参考以下标签：

* [`kubernetes.io/hostname`](https://kubernetes.io/zh/docs/reference/kubernetes-api/labels-annotations-taints/#kubernetes-io-hostname)
* [`failure-domain.beta.kubernetes.io/zone`](https://kubernetes.io/zh/docs/reference/kubernetes-api/labels-annotations-taints/#failure-domainbetakubernetesiozone)
* [`failure-domain.beta.kubernetes.io/region`](https://kubernetes.io/zh/docs/reference/kubernetes-api/labels-annotations-taints/#failure-domainbetakubernetesioregion)
* [`topology.kubernetes.io/zone`](https://kubernetes.io/zh/docs/reference/kubernetes-api/labels-annotations-taints/#topologykubernetesiozone)
* [`topology.kubernetes.io/region`](https://kubernetes.io/zh/docs/reference/kubernetes-api/labels-annotations-taints/#topologykubernetesiozone)
* [`beta.kubernetes.io/instance-type`](https://kubernetes.io/zh/docs/reference/kubernetes-api/labels-annotations-taints/#beta-kubernetes-io-instance-type)
* [`node.kubernetes.io/instance-type`](https://kubernetes.io/zh/docs/reference/kubernetes-api/labels-annotations-taints/#nodekubernetesioinstance-type)
* [`kubernetes.io/os`](https://kubernetes.io/zh/docs/reference/kubernetes-api/labels-annotations-taints/#kubernetes-io-os)
* [`kubernetes.io/arch`](https://kubernetes.io/zh/docs/reference/kubernetes-api/labels-annotations-taints/#kubernetes-io-arch)

#### topologyKey 的限制

1. 对于亲和与 `requiredDuringSchedulingIgnoredDuringExecution` 要求的 pod 反亲和，`topologyKey` 不允许为空。
2. 对于 `requiredDuringSchedulingIgnoredDuringExecution` 要求的 pod 反亲和，准入控制器 `LimitPodHardAntiAffinityTopology` 被引入来限制 `topologyKey` 不为 `kubernetes.io/hostname`。如果你想使它可用于自定义拓扑结构，你必须修改准入控制器或者禁用它。
3. 对于 `preferredDuringSchedulingIgnoredDuringExecution` 要求的 pod 反亲和，空的 `topologyKey` 被解释为“所有拓扑结构”（这里的“所有拓扑结构”限制为 `kubernetes.io/hostname`，`topology.kubernetes.io/zone` 和 `topology.kubernetes.io/region` 的组合）。
4. 除上述情况外，`topologyKey` 可以是任何合法的标签键。

####  pod 亲和 的示例

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: with-pod-affinity
spec:
  affinity:
    podAffinity:
      requiredDuringSchedulingIgnoredDuringExecution:
      - labelSelector:
          matchExpressions:
          - key: security
            operator: In
            values:
            - S1
        topologyKey: topology.kubernetes.io/zone
    podAntiAffinity:
      preferredDuringSchedulingIgnoredDuringExecution:
      - weight: 100
        podAffinityTerm:
          labelSelector:
            matchExpressions:
            - key: security
              operator: In
              values:
              - S2
          topologyKey: topology.kubernetes.io/zone
  containers:
  - name: with-pod-affinity
    image: k8s.gcr.io/pause:2.0

```

pod 亲和规则表示，仅当节点和至少一个已运行且有键为“security”且值为“S1”的标签的 pod 处于同一区域时，才可以将该 pod 调度到节点上。（更确切的说，如果节点 N 具有带有键 `topology.kubernetes.io/zone` 和某个值 V 的标签，则 pod 有资格在节点 N 上运行，以便集群中至少有一个节点具有键 `topology.kubernetes.io/zone` 和值为 V 的节点正在运行具有键“security”和值“S1”的标签的 pod。）

pod 反亲和规则表示，如果节点已经运行了一个具有键“security”和值“S2”的标签的 pod，则该 pod 不希望将其调度到该节点上。（如果 `topologyKey` 为 `topology.kubernetes.io/zone`，则意味着当节点和具有键“security”和值“S2”的标签的 pod 处于相同的区域，pod 不能被调度到该节点上。）

#### 操作符

`In`，`NotIn`，`Exists`，`DoesNotExist`  


## nodeName

`nodeName` 是节点选择约束的最简单方法

#### 使用 `nodeName` 来选择节点的一些限制

* 如果指定的节点不存在，
* 如果指定的节点没有资源来容纳 pod，pod 将会调度失败并且其原因将显示为，比如 OutOfmemory 或 OutOfcpu。
* 云环境中的节点名称并非总是可预测或稳定的。

#### 例子

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx
spec:
  containers:
  - name: nginx
    image: nginx
  nodeName: kube-01
```

