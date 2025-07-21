# AZ-104 学习笔记 - Day 13

## 🎯 今日主题：NSG、ASG 与 UDR

---

## 🔐 NSG（Network Security Group）

### ⛳ 用途
- 控制子网或 NIC 层级的网络流量
- 基于五元组规则：源 IP / 源端口 / 目标 IP / 目标端口 / 协议

### 🧱 规则结构

| 项目 | 示例 |
|------|------|
| 名称 | Allow-HTTPS-In |
| 优先级 | 100~4096，数值越小优先级越高 |
| 协议 | TCP / UDP / Any |
| 操作 | Allow / Deny |
| 端口 | 单个 / 范围 |
| 作用对象 | 子网或 NIC（网络接口） |

### 📌 默认规则（系统保留）

| 规则 | 描述 |
|------|------|
| Allow VNet Inbound | 允许同 VNet 通信 |
| Allow Azure LB | 允许 Azure 负载均衡探测 |
| Deny All Inbound | 拒绝所有来自 Internet 的入站流量 |
| Allow All Outbound | 默认允许所有出站流量 |

---

## 🧑‍🤝‍🧑 ASG（Application Security Group）

- 用于将 VM 动态分组（逻辑上类似标签）
- 在 NSG 规则中使用 ASG 名称替代具体 IP，简化管理
- 可动态识别新增 VM，无需更新规则

**示例：**
```
源：Web-ASG → 目标：DB-ASG，端口：1433
```

---

## 🧭 UDR（用户定义路由）与 VM/Subnet/NSG 的关系详解

用户定义路由（UDR, User Defined Route）是 Azure 虚拟网络中用于自定义流量转发路径的功能。UDR 主要应用于子网（Subnet）层级，作用是覆盖或补充系统默认路由，实现如强制流量经过防火墙、NAT、VPN 设备等场景。

### UDR 与 VM/Subnet/NSG 的关系说明

- **UDR 必须关联到子网（Subnet）**  
  UDR 只能应用于子网，不能直接绑定到单个 VM。子网内所有 VM/资源的出入流量都会受到该子网 UDR 的影响。

- **VM 受 UDR 影响**  
  当 VM 所在的子网被分配了 UDR，VM 的网络流量会按照 UDR 的路由规则转发。例如，所有出站流量都可以被定向到防火墙或 NVA。

- **UDR 与 NSG 可同时使用，但作用不同**  
  - **UDR** 控制“流量路径”，决定数据包如何转发。
  - **NSG**（网络安全组）控制“流量访问权限”，决定哪些流量允许或拒绝。
  - 两者可以同时应用于同一子网或 VM（NSG 可绑定子网或网卡），但互不冲突。实际流量需同时满足 UDR 路由和 NSG 访问规则。

### 典型用法举例

1. 在子网上绑定 UDR，将所有 0.0.0.0/0 流量下一跳指向防火墙 IP，实现强制出站流量经过安全设备。
2. 同时在该子网或 VM 网卡上配置 NSG，仅允许特定端口/协议的流量通过，提升安全性。

**总结：**
- UDR 必须与子网一起使用，不能直接作用于 VM。
- NSG 可与 UDR 结合，分别负责“路径”和“权限”。
- 推荐在设计网络安全和流量控制时，合理搭配 UDR 和 NSG。

---

## ✅ 小测验回顾（5 / 5）

### 1. NSG 中优先级判断
- ❓ 条件：Rule A 允许 443，优先级 200；Rule B 拒绝 443，优先级 100  
- ✅ 正确：Rule B 生效（优先级低 → 更优先）

### 2. 默认 NSG 规则允许哪些流量？
- ✅ A：VNet 内通信允许  
- ✅ C：允许 LB 探测  
- ✅ D：允许所有出站  
- ❌ B：SSH 入站流量被默认拒绝

### 3. 使用 ASG 的典型场景
- ✅ A：组织大量 VM 统一管理  
- ✅ B：为 Web/DB/Cache 设定逻辑分组  
- ✅ C：自动识别新加入 VM  
- ❌ D：DNS 名称解析由 Azure DNS 实现，与 ASG 无关

### 4. 强制子网出站走防火墙配置
- ✅ 正确设置：地址前缀 0.0.0.0/0，下一跳类型 Virtual Appliance，指定 Azure Firewall IP

### 5. NIC 与 Subnet 同时绑定 NSG，规则生效逻辑
- ✅ 结果：**规则联合评估**，两者都允许，才通过

---

## 🌐 Azure 与 AWS 网络安全资源对比

| 资源/功能         | Azure 名称/机制                       | AWS 名称/机制                          | 主要区别说明                                   |
|-------------------|---------------------------------------|----------------------------------------|------------------------------------------------|
| 网络安全组        | NSG（Network Security Group）         | Security Group                         | Azure 支持子网/NIC 双层绑定，AWS 仅实例/接口级 |
| 应用安全组        | ASG（Application Security Group）      | 无直接对应，常用标签+SG组合             | AWS 无原生 ASG，需用标签和 SG 组合实现         |
| 用户定义路由      | UDR（User Defined Route）              | Route Table                            | 功能类似，均支持自定义路由和黑洞路由           |
| 默认规则          | 默认拒绝入站、允许出站                 | 默认拒绝入站、允许出站                  | 行为一致                                       |
| 规则优先级        | 明确优先级数值，数值越小优先级越高      | 按规则顺序评估，无优先级数值            | Azure 更直观，AWS 依赖规则顺序                 |
| 作用对象          | 子网、NIC                              | 实例、弹性网卡（ENI）                   | Azure 支持更细粒度绑定                         |
| 动态分组          | ASG 动态分组，自动识别新 VM            | 需手动维护 Security Group 和标签         | Azure 管理更自动化                             |
| 下一跳类型        | 支持 Virtual Appliance、Internet、None | 支持 NAT Gateway、Internet Gateway、NAT | 命名不同，功能类似                             |
| 远程管理          | Azure Bastion                          | AWS Systems Manager Session Manager     | 均可实现无公网 IP 远程管理                     |

**总结：**
- Azure NSG/ASG 组合可实现更灵活的动态分组与批量规则管理，AWS 需结合标签和 Security Group 实现类似效果。
- 路由、规则、远程管理等核心能力两者均支持，命名和细节略有差异，建议结合实际场景选择。

---

## 🧠 今日重点记忆

- NSG 基于优先级 + 五元组判断流量
- 默认 NSG 拒绝入站、允许出站
- ASG 帮助动态分组，便于批量管理 NSG 规则
- UDR 用于强制路由流量，典型用途是接入 Azure Firewall 或 NAT Gateway

---

## 📅 下一步预告 - Day 14

| 模块 | 内容 |
|------|------|
| Azure Bastion | 无需公有 IP 远程管理 VM |
| JumpBox / 管理子网 | 管理访问策略设计 |
| 网络架构小测验 | 综合场景题 |

