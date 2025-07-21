
# AZ-104 学习笔记 - Day 3

## 🎯 学习目标
- 掌握 Azure 虚拟机（VM）架构与核心组件
- 理解高可用性机制（Availability Set, Availability Zone, VM Scale Set）
- 理解磁盘类型、VM 映像和自定义扩展脚本的作用
- 通过测验巩固 HA 概念

---

## 🧱 Azure VM 核心概念

| 组件 | 描述 |
|------|------|
| **SKU (Size)** | 定义计算资源规格（vCPU、内存）如 `Standard_B2s`、`D4s_v5` |
| **镜像（Image）** | 官方镜像（Windows/Linux）或自定义映像 |
| **OS Disk** | 可自动创建，也支持使用 Shared Image Gallery 或 VHD 映像 |
| **数据磁盘** | 附加磁盘，适合存放业务数据，可选 Premium SSD、Standard SSD 等 |
| **NIC（网络接口）** | VM 的网卡，绑定子网、NSG、公网 IP |
| **扩展（Extension）** | 如 Custom Script Extension，可用于自动化安装/初始化任务 |

### 🔷 SKU 命名结构
#Standard_[Family][Size][Optional features]_v[Version]#

```yaml
Standard_D2s_v3
         ┬ ┬ ┬   ┬
         │ │ │   └─ v3: 第3代版本
         │ │ └──── s: 支持 Premium SSD（optional）
         │ └────── 2: CPU 数量 / VM 尺寸
         └──────── D: 系列（用途/CPU/内存比）
```
### 🧩 各部分含义详解
1. 系列（Family）

| 系列名 | 用途说明              |
|-----|-------------------|
| A   | 入门级测试用            |
| B   | 可突发型实例，低成本        |
| D   | 通用型，均衡 CPU/内存     |
| E   | 内存优化型             |
| F   | 计算优化型（高 CPU 比例）   |
| G   | 内存和存储极大（Godzilla） |
| H   | 高性能计算（HPC）        |
| L   | 存储优化              |
| M   | 超大内存型             |
| N   | GPU 加速型（如 NVIDIA） |

2. Size 数字（大小）
   - 表示 CPU 核心数（大致上）
   - 比如：D2 是 2 vCPU，D4 是 4 vCPU，等等

3. 可选后缀（Features）

| 后缀   | 含义                   |
|------|----------------------|
| s    | 支持 Premium SSD       |
| ms   | 支持内存优化 + Premium SSD |
| b    | 突发性能（burstable）      |
| d    | 本地临时磁盘               |
| vCPU | 只读访问（特殊情况）           |

4. 版本号（vN）
   - 表示这个 SKU 是第几代，比如：
   - v1：原始版本（有时不写）
   - v2、v3、v5 等是后续更新，通常是 CPU 升级（Intel → AMD → ARM），或新硬件支持


### 🔎 示例分析

| SKU                 | 含义                                                |
|---------------------|---------------------------------------------------|
| Standard_B1s        | 可突发型，1 vCPU，支持 Premium SSD                        |
| Standard_D4as_v5    | 第5代 D 系列，4 vCPU，使用 AMD CPU，支持 Premium SSD         |
| Standard_E32-16s_v4 | E 系列（内存优化），32 核（其中 16 vCPU 可用），支持 Premium SSD，第4代 |
| Standard_NV12s_v3   | GPU 型，N 系列，第3代，12 vCPU，支持 Premium SSD             |

---

## 🛡️ 高可用性机制（High Availability）

### 🔹 Availability Set（可用性集）

- 区域内部署多个 VM，**分布于不同的机架和更新域**
- 包括：
  - ✅ 故障域（Fault Domain）：机架级别隔离
  - ✅ 更新域（Update Domain）：计划更新批次隔离
- 可用性保障：**99.95% SLA**

### 🔹 Availability Zone（可用性区域）

- 跨物理数据中心（Zone）部署 VM，Zone 之间互为容灾
- 高级别物理隔离和网络电力冗余
- 可用性保障：**99.99% SLA**

### 🔹 Virtual Machine Scale Set（VMSS）

- 自动扩展 VM 数量（基于 CPU/内存/队列等指标）
- 可设置为 Zone-aware，结合负载均衡器使用
- 适合 web 应用、计算密集型服务、批处理

---

## 🧰 Custom Script Extension（自定义脚本扩展）

- 可在 VM 启动后注入脚本（如 shell / PowerShell）执行任务
- 场景示例：安装 nginx、配置 agent、安全初始化等
- 支持 CLI、Portal、Terraform 等多种方式部署

---

## 🚦 测验结果（Day 3）

| 题号 | 正确答案 | 说明 |
|------|-----------|------|
| 1 | C | Availability Set 提升可用性，适用于单区域内部署 |
| 2 | B, C | Availability Set 包含 Fault Domain 和 Update Domain |
| 3 | B | VM 支持使用镜像部署 |
| 4 | C | VMSS 支持自动扩缩容 |
| 5 | C | Custom Script 可用于部署后初始化，如安装软件 |

🧠 注意：
- 地理区域 ≠ Availability Set 使用的域
- 容灾域不是 Azure 定义的概念

---

## ✅ 实操任务参考

1. 创建 Availability Set（avset-day3）
2. 创建 2 台 VM 放入该可用性集中
3. 使用 Custom Script 安装 nginx 并开放 80 端口
4. 验证 VM 分布在不同 Fault Domain / Update Domain 中

---

## 📌 关键记忆点

| 概念 | 描述 |
|------|------|
| Availability Set | 区域内分布容灾，99.95% SLA |
| Availability Zone | 跨物理数据中心，99.99% SLA |
| VMSS | 自动扩缩容服务 |
| Extension | 可部署后注入初始化脚本 |
| OS Disk 来源 | 可用 Marketplace 镜像或自定义映像 |

---

## 📅 下一步：Day 4 预告

- 学习 Azure Storage Account 类型与复制机制
- 掌握 Blob / Table / File / Queue 各自用途
- 理解授权模型（Shared Key / SAS / RBAC / 防火墙）
- CLI 或 Terraform 操作 Blob 上传下载
- 完成 Day 4 随堂测验
