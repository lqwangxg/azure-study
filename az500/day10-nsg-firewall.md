# 🌐 AZ-500 学习笔记 - Day 10：网络安全控制

---

## 📘 网络安全知识点整理

### 网络安全组（NSG）
- 控制 Azure 虚拟网络中资源的入站和出站流量。
- 基于五元组（源/目标 IP、源/目标端口、协议）过滤。

### 应用安全组（ASG）
- 为虚拟机提供逻辑分组，以便 NSG 中规则动态引用。
- 便于大规模虚拟机管理。

### Azure Firewall
- 状态检测型托管防火墙服务。
- 支持网络规则（L3-L4）、应用规则（FQDN）、NAT 转发。
- 可与 **Firewall Policy** 结合使用以实现集中策略管理。

### Firewall Policy
- 用于集中控制多个防火墙策略。
- 可包含多个规则集合组，支持层级结构与子策略继承。

### Private Endpoint / Private Link
- 将 Azure PaaS 资源（如 Azure Storage, SQL DB）注入虚拟网络中。
- 通过分配专用 IP 提供私有访问。

### Azure DDoS Protection
- Basic：所有资源默认启用，提供基本防护。
- Standard：付费版，提供增强防护、警报、指标和攻击日志。

### Network Watcher
- Azure 提供的网络监控和诊断服务集合，功能包括：
  - **IP Flow Verify**：检查 NSG 是否阻挡流量。
  - **Connection Monitor**：持续连接测试。
  - **Flow Logs**：记录流量日志（基于 NSG）。
  - **Packet Capture**：抓包分析。
  - **Topology Viewer**：网络拓扑图。

---

## ❌ 错题回顾

### 第 8 题：IP Flow Verify

**题干：**  
Network Watcher 中用于检查 NSG 是否阻挡某条通信路径的功能是？

**选项：**
- A. Connection Monitor  
- B. Flow Logs  
- C. IP Flow Verify  
- D. Packet Capture  

**你的答案：** A  
**正确答案：** C  

**解析：**  
IP Flow Verify 是用来验证从 VM 发出的流量是否被 NSG 拒绝的工具。可以指定源 VM、目标 IP/端口等信息，返回是否被允许。

---

### 第 9 题：Flow Logs

**题干：**  
你要分析某 VM 的实际入站/出站连接记录，建议启用？

**选项：**
- A. NSG Diagnostic Logs  
- B. Flow Logs  
- C. Application Insights  
- D. DDoS Metrics  

**你的答案：** A  
**正确答案：** B  

**解析：**  
Flow Logs 是记录虚拟网络中通过 NSG 的真实网络流量数据的服务。它可以帮助你了解有哪些连接尝试了通信、被允许/拒绝的原因。

---
# 🔍 网络监控与安全控制要点整理

## 📡 Network Watcher 工具种类与用途

| 工具名称             | 功能用途                                                                 |
|----------------------|--------------------------------------------------------------------------|
| IP Flow Verify       | 检查指定流量是否被 NSG 允许或拒绝（基于五元组）                          |
| Next Hop             | 分析某个 VM 发出的流量将被路由到哪里                                     |
| Effective Security Rules | 查看作用于某个网卡（NIC）的最终生效 NSG 规则（合并结果）              |
| Connection Troubleshoot | 验证从某 VM 到目标地址/端口是否可连通                                   |
| Packet Capture       | 抓取虚拟机网络数据包，用于流量分析与排障                                 |
| Connection Monitor   | 持续监控端到端连接状态和延迟                                             |
| NSG Flow Logs        | 记录 NSG 的允许/拒绝流量日志（适合存储到 Storage 进行分析）              |
| Topology             | 可视化展示虚拟网络的拓扑结构                                             |
| Security Group View  | 查看子网或网卡上配置的 NSG 规则（简化可视化）                             |

---

## 📊 Flow Logs 与 NSG Logs 区别

| 对比项           | Flow Logs（NSG Flow Logs）                                | NSG Diagnostic Logs（旧称）                    |
|------------------|------------------------------------------------------------|------------------------------------------------|
| 记录内容         | 实际的网络流量通过/被拒的记录，包含源/目标 IP 和端口等详细信息 | 记录 NSG 配置变化和操作日志                    |
| 用途             | 分析安全事件、流量分析、异常检测                            | 审计用途、合规检查                              |
| 启用位置         | Network Watcher 中启用，绑定到 NSG                         | Azure Monitor 中启用 Diagnostic Settings       |
| 支持输出         | 支持输出到 Storage / Log Analytics / Event Hub            | 支持输出到 Log Analytics / Storage / Event Hub |
| 计费             | 按数据量计费                                               | 免费或按日志存储计费                            |

📝 小结：  
- **Flow Logs** 更适合做流量行为分析与攻击排查。  
- **NSG Logs（诊断日志）** 更适合做配置审计与变更跟踪。

---

## 🔥 Azure Firewall vs NSG / ASG 的适用场景

| 特性项              | Azure Firewall                           | NSG                                   | ASG（配合 NSG 使用）                  |
|---------------------|-------------------------------------------|----------------------------------------|----------------------------------------|
| 层级                | L3-L7                                     | L3-L4                                  | L3-L4（逻辑分组）                      |
| 状态检测           | ✅ 有状态                                  | ❌ 无状态                               | ❌ 无状态                               |
| 支持协议           | 全协议（TCP, UDP, ICMP, HTTP/S 等）       | TCP/UDP                                | TCP/UDP                                |
| 支持 URL/FQDN 规则 | ✅ 支持（通过 Application Rule）           | ❌ 不支持                               | ❌ 不支持                               |
| 网络地址转换（NAT）| ✅ 支持出站 SNAT 和入站 DNAT               | ❌ 不支持                               | ❌ 不支持                               |
| 日志记录           | ✅ 支持详细日志（通过诊断设置）            | ✅ 支持（需配合 NSG Flow Logs）         | ❌ 不提供日志本身                       |
| 管理方式           | 基于规则集合和 Firewall Policy 管理        | 每个 NSG 单独配置                      | 提供逻辑分组便于 NSG 编写              |
| 成本               | 💰 有额外费用（每小时 + 流量）            | ✅ 免费（内置资源）                     | ✅ 免费（NSG 扩展）                    |

🔎 适用场景：
- **Azure Firewall**：用于中心防火墙、跨区域控制、统一策略管理（Hub-and-Spoke 架构）。
- **NSG**：适合应用在子网或 NIC 上进行入/出站控制。
- **ASG**：与 NSG 搭配，用于 VM 动态分组（不依赖固定 IP）。

