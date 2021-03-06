# Kubernetes 是什么？

## Kubernetes 的定义

#### 一句话解释：

自动化管理容器换的应用，也可以说是容器编排工具。

一个可移植的、可扩展的开源平台，用于管理容器化的工作负载和服务，可促进声明式配置和自动化。Kubernetes 是支持docker，rkt和其他容器类型的容器编排系统。

## 为什么需要Kubernetes？

单体应用容易部署，但随着时间的推移，难以维护

基于微服务的应用程序结构使得各个组件的开发变得容易，但很难配置和部署。

Docker 可以更加高效地管理容器换应用和它的操作系统。

开发人员可以通过Kubernetes管理容器，部署应用程序。

## 核心功能

Kubernetes 系统有一个主节点和若干个工作节点组成。开发者将一个工作列表提交到主节点上，Kubernetes 将他们部署到集群的工作节点上。开发者和运维人员不需要关心组件被部署到哪一个节点上。

开发者可以指定某些应用必须在一起运行。K8s 会将这些应用部署到一个工作节点上。其他的应用会被分散部署到集群。

## 功能

1. **服务发现和负载均衡** Kubernetes 可以使用 DNS 名称或自己的 IP 地址公开容器，如果到容器的流量很大，Kubernetes 可以负载均衡并分配网络流量，从而使部署稳定。
2. **存储编排** Kubernetes 允许您自动挂载您选择的存储系统，例如本地存储、公共云提供商等。
3. **自动部署和回滚** 您可以使用 Kubernetes 描述已部署容器的所需状态，它可以以受控的速率将实际状态更改为所需状态。例如，您可以自动化 Kubernetes 来为您的部署创建新容器，删除现有容器并将它们的所有资源用于新容器。也可以指定K8s只在集群中特定的节点运行容器，比如说某些程序必须运行在带有SSD的节点上。
4. **自动二进制打包** Kubernetes 允许您指定每个容器所需 CPU 和内存（RAM）。当容器指定了资源请求时，Kubernetes 可以做出更好的决策来管理容器的资源。
5. **自我修复** Kubernetes 重新启动失败的容器、替换容器、杀死不响应用户定义的运行状况检查的容器，并且在准备好服务之前不将其通告给客户端。
6. **密钥与配置管理** Kubernetes 允许您存储和管理敏感信息，例如密码、OAuth 令牌和 ssh 密钥。您可以在不重建容器镜像的情况下部署和更新密钥和应用程序配置，也无需在堆栈配置中暴露密钥。

## 



