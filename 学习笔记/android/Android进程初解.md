## 相关阅读

[初始Android进程](https://mp.weixin.qq.com/s/3X_M04lAe3AVkyALIuWDfQ)

Android 是一个 **基于 Linux 内核的操作系统**，其进程体系结合了 **Native 进程** 与 **Java/Kotlin 应用进程**，整体包括系统启动进程、关键服务进程、应用孵化进程以及后台守护进程。下面逐层展开。

---

## 一、Linux 内核基础

- Android 内核基于 **Linux**，所有进程都是 **Native Linux 进程**。
- 每个进程都有：

- **PID**（进程标识）
- **PPID**（父进程 PID）
- **UID/GID**（权限标识）

- 内核负责：

- CPU 调度
- 内存分配和管理
- 文件描述符和 I/O 管理

- **Native 进程**：由 C/C++ 编写，例如系统服务
- **应用进程**：运行在 ART 虚拟机中，但底层仍是 Native 进程

---

## 二、系统启动核心进程

### 1. `init` 进程

- **PID = 1**，系统启动的第一个进程
- 负责：

- 初始化 Linux 系统环境（挂载文件系统、启动守护进程）
- 启动系统关键服务（如 servicemanager、surfaceflinger、zygote、lmkd 等）
- 读取 `init.rc` 配置文件，执行启动脚本

- **重要性**：Android 所有进程的祖先，负责整个系统启动流程

---

### 2. `servicemanager`

- 系统服务注册中心
- **作用**：

- 提供 **Binder IPC** 服务注册和发现
- 各系统服务（如 ActivityManagerService、WindowManagerService）向其注册
- 客户端通过它查找服务

- **运行方式**：Native 进程，由 init 启动
- **位置**：系统层
- **依赖**：Binder 驱动

---

### 3. `surfaceflinger`

- **显示系统服务**
- **作用**：

- 管理屏幕缓冲区
- 窗口合成与 GPU 渲染
- 为应用提供统一的显示输出

- **运行方式**：Native 进程
- **特点**：

- 高性能渲染
- 通常 PID 较小，常驻内存

- **依赖**：GPU 驱动和硬件加速

---

### 4. `lmkd`（Low Memory Killer Daemon）

- **内存管理守护进程**
- **作用**：

- 监控系统内存占用
- 清理后台不活跃应用，释放内存

- **运行方式**：Native 进程
- **目标**：确保前台应用流畅运行
- **机制**：结合 Linux 内核内存回收机制（OOM Killer）

---

## 三、Zygote 与应用进程

### 1. Zygote

- **特殊 Native 进程 + ART 虚拟机**
- **启动**：由 init 启动，PID 较小
- **职责**：

1. **初始化 ART 虚拟机**
2. **预加载系统类和资源**（如常用 Java 类、Framework 资源）
3. **孵化应用进程**（fork）

- **fork 优势**：

- 内存共享（写时复制 COW）
- 启动速度快

- **进程生命周期**：

- 长期驻留内存
- 为多个应用提供孵化服务

---

### 2. 应用进程

- **由 Zygote fork 出来的子进程**
- **PID**：独立，PPID = Zygote PID
- **内部运行**：ART 虚拟机，执行应用 Java/Kotlin 逻辑
- **Linux 角度**：Native 进程
- **ART 角度**：独立虚拟机实例
- **父子关系示例**：

```
Zygote (PID 100)
    ├─ App1 (PID 101, PPID 100)
    └─ App2 (PID 102, PPID 100)
```

- **多实例启动**：

- 每个 App 都是独立进程
- 共享 Zygote 预加载资源
- 避免重复加载系统类，提高启动效率

---

## 四、其他系统核心进程

|进程名称|类型|功能|说明|
|---|---|---|---|
|`init`|Native|系统初始化和启动|进程祖先，读取 init.rc 配置|
|`servicemanager`|Native|系统服务注册中心|Binder IPC 服务发现|
|`surfaceflinger`|Native|显示服务|窗口合成、GPU 渲染|
|`zygote`|Native + ART|应用孵化器|fork 出应用进程，初始化 ART|
|`lmkd`|Native|内存管理守护|后台应用清理，保证前台流畅|
|`logd`|Native|日志收集|收集系统日志|
|`mediaserver`|Native|多媒体处理|音视频编解码服务|
|`netd`|Native|网络守护|网络管理与策略执行|

---

## 五、进程间关系与管理工具

- **Linux 工具可见性**：

- `ps -ef`、`top`、`htop` 都可以看到所有 Native 进程
- 应用进程 PID 和 PPID 显示 Zygote 父子关系

- **系统结构示意**：

```
init (PID 1)
  ├─ servicemanager
  ├─ surfaceflinger
  ├─ lmkd
  ├─ zygote
  │    ├─ App1
  │    └─ App2
  ├─ logd
  └─ mediaserver
```

---

## 六、总结理解

1. **Android 是 Linux 内核 + 系统服务 + ART 虚拟机的结合**
2. **init** 是祖先进程，启动系统服务
3. **servicemanager** 提供 Binder 服务注册与发现
4. **surfaceflinger** 提供显示管理，**lmkd** 提供内存管理
5. **Zygote** 是应用孵化器，fork 出独立 App 进程
6. **应用进程** 是 Native + ART 的组合，运行 Java/Kotlin 代码
7. **所有进程都是 Linux 内核可见的 Native 进程**，父子关系和 PID/PPID 可用系统工具查看

---

![](https://cdn.nlark.com/yuque/0/2025/png/23135285/1755743346064-0b3c527d-1caa-40ca-afc3-5c597de15110.png "Android 系统进程完整架构图")