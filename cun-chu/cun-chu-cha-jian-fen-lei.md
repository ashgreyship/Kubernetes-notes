# 存储插件分类

## In-tree

In-tree 存储插件的代码是在 Kubernetes 核心代码库里，In-tree 存储插件运行在 Kubernetes 核心组件里。Kubernetes 容器平台要使用后端某类存储服务，需要调用相应的 In-tree 存储插件，比如 Kubernetes 容器平台要使用后端 AWS 存储服务，需要调用 In-tree AWS 存储插件才能对接后端 AWS 存储服务。

## Out-of-tree

其代码和 Kubernetes 核心代码相独立。它的部署与 Kubernetes 核心组件的部署相独立，Kubernetes 核心组件可以通过调用某类 Out-of-tree 存储插件对接我们后端的存储服务，比如 Kubernetes 可以通过调用 GCE Out-of-tree 存储插件对接后端 GCE 存储服务。

## 对比

![](../.gitbook/assets/image%20%288%29.png)

## Out-of-tree 存储插件分类

![](../.gitbook/assets/image%20%286%29.png)

Out-of-tree 存储插件现在分为 FlexVolume 和 CSI 两大类。

CSI 方案全称是 Container Storage Interface，它是容器平台里的一种工业标准。CSI 不仅仅是针对 Kubernetes 容器平台开发的，它是一种容器平台的通用解决方案。存储服务商或者存储厂商只要开发支持 CSI 标准的存储插件，就可以供各种容器平台使用。Kubernetes 从 1.9 开始支持 CSI 规范。

CSI 插件方案部署起来比较简单，可以支持容器化部署，在 Kubernetes 容器平台中，我们可以用 Kubernetes 的资源对象直接部署 CSI 插件。CSI 插件方案功能比较强大，除了存储卷管理功能外，还有快照管理功能。CSI 的存储标准方案在持续快速发展中，功能也会不断扩展。今后我们会尽量开发和使用基于 CSI 的存储插件方案。

![](../.gitbook/assets/image%20%287%29.png)

