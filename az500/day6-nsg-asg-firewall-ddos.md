# 📘 AZ-500 Day 6 学习笔记与错题集

## 📌 学习重点：Azure 网络安全机制

### 1. 网络安全组（NSG: Network Security Group）
- 控制 Azure 虚拟网络中资源的出入站网络流量。
- 每条 NSG 规则包含：优先级（数值越小越高）、方向、协议、端口、源/目标、允许/拒绝。
- NSG 可以作用于子网或 NIC。
- **默认规则**包括 VNet 内部通信、Internet 出站、Azure 负载均衡探测器等。

### 2. Application Security Group（ASG）
- 可将多个 VM 按应用角色进行逻辑分组。
- 在 NSG 规则中可引用 ASG，而不是 IP 地址。

### 3. Azure Firewall
- 完全托管、状态检测型的防火墙。
- 支持应用规则（FQDN）、网络规则、NAT 规则。
- 可集成 Log Analytics、Threat Intelligence、Policy 管理。
- 能与 NSG 同时使用，建议用于集中策略控制。

### 4. DDoS Protection
- **Basic**：默认启用，保护 Azure 平台基础设施。
- **Standard**：需要手动启用，对指定 VNet 中资源进行保护。
  - 提供 DDoS 报告与分析、成本保障、自动缓解。
  - 不自动与 Application Gateway 集成，需绑定 VNet。

### 5. 日志与监控
- 网络安全服务日志通常可导出至 Log Analytics、Storage Account、Event Hub。
- 通过诊断设置启用日志审计，便于分析入侵行为。

---

## ❌ Day 6 错题回顾

### ❌ 第 1 题

**题干：**  
你希望对所有 Web 服务器统一管理 NSG 规则，应该使用什么机制？

**你的答案：** C  
**正确答案：** B

**解析：**  
若要将一组 VM 应用于相同 NSG 规则，应使用 **Application Security Group (ASG)**，它允许按逻辑角色而非 IP 进行分组。选择 Firewall 是过度设计。

---

### ❌ 第 6 题

**题干：**  
DDoS Protection Standard 的特点有哪些？（选择三项）

**你的答案：** A, B, D  
**正确答案：** B, C, D

**解析：**  
- ✅ B：提供攻击期间和之后的日志报告。
- ✅ C：具备自动缓解能力，基于流量动态检测。
- ✅ D：可与 Azure Monitor 集成进行告警和可视化。
- ❌ A：错误。DDoS Standard 不会自动与 Application Gateway 绑定，需手动启用并绑定 VNet。

---
