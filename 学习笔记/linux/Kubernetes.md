# 1. 概述

## 1.1. 定义

Kubernetes 是一个开源的 **容器编排平台**，用于自动化部署、扩展和管理容器化应用。  
它可以在多台服务器上统一管理容器，保证应用的高可用、负载均衡和弹性伸缩。

---

## 2. Kubernetes 的目标和解决的问题

K8s 主要解决以下问题：

1. **容器调度与管理**：自动选择合适的节点运行容器。
2. **高可用**：容器或节点异常时自动重启或迁移。
3. **服务发现与负载均衡**：让应用服务可以互相发现并均衡流量。
4. **自动扩缩容**：根据负载动态增加或减少容器实例。
5. **自愈能力**：容器异常自动替换，保持期望状态。
6. **资源隔离与多租户**：通过命名空间、限额管理不同应用或团队资源。
7. **统一管理**：通过声明式 API，管理集群状态。

---

## 3. 核心概念

### （1）节点 Node

- **Node** 是集群中的服务器（物理机或虚拟机）。
- 类型：

- **主节点 Master / Control Plane**：管理集群，调度、控制和存储状态。
- **工作节点 Worker Node**：运行应用容器（Pod）。

### （2）Pod

- Pod 是 K8s 的 **最小部署单元**，一个 Pod 内可以运行一个或多个紧密相关的容器。
- Pod 内容器共享网络 IP、端口和存储卷。

### （3）Deployment

- Deployment 管理 Pod 的生命周期，保证期望副本数。
- 支持滚动升级、回滚等操作。

### （4）Service

- Service 为 Pod 提供 **统一访问入口**。
- 类型：

- ClusterIP（集群内部访问）
- NodePort（节点端口访问）
- LoadBalancer（外部负载均衡）

### （5）ConfigMap / Secret

- 用于管理应用配置和敏感信息（如数据库密码）。
- Secret 数据会以加密形式存储。

### （6）Namespace

- 提供 **逻辑隔离**，适合多团队或多环境共用一个集群。

### （7）Ingress

- 提供 HTTP/HTTPS 的路由和负载均衡，通常结合 Ingress Controller 使用。

---

## 4. 核心组件

### 主节点组件（Control Plane）

|组件|功能|
|---|---|
|kube-apiserver|集群 API 接口，所有操作都通过它|
|etcd|分布式存储，保存集群状态|
|kube-scheduler|调度 Pod 到合适的节点|
|kube-controller-manager|控制器，维护集群期望状态|
|cloud-controller-manager|云资源与集群的集成管理（可选）|

### 工作节点组件（Worker Node）

|组件|功能|
|---|---|
|kubelet|节点 Agent，管理 Pod|
|kube-proxy|负责服务网络代理和负载均衡|
|容器运行时|Docker / containerd，用于实际运行容器|

---

## 5. Kubernetes 特性

- **声明式管理**：用户只需声明想要的状态，K8s 自动维护。
- **自动调度与扩缩容**：根据资源和策略自动调度 Pod。
- **自愈能力**：异常 Pod 自动重建。
- **服务发现与负载均衡**：Pod 间可以互相访问，流量自动均衡。
- **滚动升级与回滚**：支持零停机升级应用。
- **多租户和资源管理**：Namespace + ResourceQuota 管理团队资源。

---

## 6. Kubernetes 工作流程示意

```
用户 → kubectl / API → kube-apiserver → Scheduler → Worker Node
                                              └─ kubelet → Pod → 容器
```

- 用户通过 `kubectl` 或 API 提交应用部署。
- kube-apiserver 接收请求，存储状态到 etcd。
- Scheduler 分配 Pod 到合适的节点。
- kubelet 在节点上创建 Pod，容器运行应用。

---

总结：  
Kubernetes 是一个 **集群级别的容器管理平台**，通过抽象节点、Pod、Service 等概念，实现了容器的自动化部署、扩缩容、负载均衡和自愈能力，使复杂的分布式应用可以更可靠、更高效地运行。

---

明白，你想了解在 **Kubernetes 集群搭建时多台云主机的分配以及主节点（Master/Control Plane）和工作节点（Worker Node）的部署位置**。我给你详细整理一下：

---

# 2. Kubernetes 集群组成

K8s 集群主要有两类节点：

|节点类型|主要功能|典型部署数量|
|---|---|---|
|**主节点（Master / Control Plane）**|管理整个集群，负责调度、API 接口、集群状态等|1~3（高可用）|
|**工作节点（Worker Node）**|运行应用容器（Pod）|1 个以上，根据负载灵活扩展|

### 主节点组件

主节点上运行的核心组件：

- **kube-apiserver**：集群 API 接口
- **etcd**：存储集群状态
- **kube-scheduler**：调度 Pod 到节点
- **kube-controller-manager**：控制器管理集群状态
- **cloud-controller-manager**：云环境下与云资源交互（可选）

### 工作节点组件

工作节点上运行：

- **kubelet**：节点上 Pod 管理
- **kube-proxy**：服务负载均衡和网络代理
- **容器运行时**（Docker、containerd 等）

---

## 二、云主机分配示例

假设你有 5 台云主机：

|云主机|节点角色|说明|
|---|---|---|
|主机 1|主节点|运行 API、Scheduler、Controller、etcd|
|主机 2|主节点（可选 HA）|高可用 Master 节点，etcd 集群的一部分|
|主机 3|主节点（可选 HA）|高可用 Master 节点，etcd 集群的一部分|
|主机 4|工作节点|部署应用 Pod|
|主机 5|工作节点|部署应用 Pod|

### 说明

1. **单主节点**：1 台云主机作为 Master，适合测试或小型集群。
2. **高可用主节点**：建议 3 个 Master 节点，组成 etcd 集群，提高控制平面容错能力。
3. **工作节点可随负载增加**：根据应用数量和性能需求灵活增加 Worker。
4. **主节点不建议跑用户应用**（生产环境），专门用于管理集群。

---

## 三、网络与访问

- **主节点**对外暴露 **API Server**，可以用 LoadBalancer 或 VIP 做高可用入口。
- **工作节点**与主节点内部通信（kubelet、kube-proxy）需网络互通。
- **Pod 间通信**通过 K8s 网络插件（CNI），如 Flannel、Calico。

---

## 四、部署示意

```
┌─────────────┐
          │ Master Node │  ← 控制平面
          │ 1~3台 HA   │
          └─────┬──────┘
                │ API / Scheduler / etcd
        ┌───────┴────────┐
        │                │
   ┌─────────┐      ┌─────────┐
   │Worker 1 │      │Worker 2 │  ← 运行应用 Pod
   └─────────┘      └─────────┘
```

- Master 高可用集群：至少 3 台 Master 节点
- Worker 节点可随需求扩展
- Master 节点和 Worker 节点建议分开，避免资源争用

---