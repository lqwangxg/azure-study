
# AZ-104 学习笔记 - Day 32

## 🌐 应用安全配置与混合网络集成

---

## 🔐 App Service 与 Key Vault 集成

### 📌 目标
- 避免在代码或 App Settings 中硬编码密码
- 实现集中、安全、可轮换的密钥管理

### 📌 实现步骤
1. 创建 Key Vault 并添加机密（Secrets）
2. 为 App Service 启用系统托管身份（System Assigned Managed Identity）
3. 将该身份授予访问 Key Vault 的权限（如 Secrets Reader）
4. 在 App Settings 中使用以下语法引用 Key Vault：
```plaintext
@Microsoft.KeyVault(SecretUri=https://<vault-name>.vault.azure.net/secrets/<secret-name>/)
```

---

## 🔗 App Service 的混合网络连接

### 🔹 Regional VNet Integration
- 适用于 PremiumV2/V3 或 Elastic Premium SKU
- 仅支持出站（Outbound）访问 VNet 内部资源
- 无法用于接收来自 VNet 的入站访问

### 🔹 Private Endpoint（私有接入）
- 为 App Service 分配一个专属的 VNet IP
- 实现私密访问 App Service
- 需要绑定 DNS：`privatelink.azurewebsites.net`
- 可搭配 NSG 限制入站/出站流量
- 完全关闭公网访问可能导致管理操作受限

### 🔹 Hybrid Connection
- App Service → 本地资源的 TCP 连接
- 使用 Azure Relay（Service Bus Relay）进行通信中转
- 不要求对端暴露公网 IP
- 仅支持 TCP，**不支持 UDP / ICMP / Ping**

---

## 🌍 App Service 的高可用部署策略

### 🔸 多区域部署（Active-Active）
- 在多个 Azure 区域部署冗余 App Service 实例
- 结合 DNS 服务实现智能分发

### 🔸 Azure Traffic Manager（DNS 层负载均衡）
- 非 IP 层负载服务
- 支持多种路由策略：

| 策略       | 说明 |
|------------|------|
| 性能优先    | 按响应速度路由至最近区域 |
| 优先级      | 主备模式 |
| 加权轮询    | 自定义权重比例分发 |
| 地理位置    | 按国家或地区路由 |
---

## 🌐 Azure、AWS、GCP 关于应用安全配置与混合网络集成的对比

| 维度               | Azure                                              | AWS                                              | GCP                                              |
|--------------------|----------------------------------------------------|--------------------------------------------------|--------------------------------------------------|
| 应用安全配置       | 支持 App Service/VM 的 NSG、WAF、Private Endpoint、Managed Identity、Key Vault、RBAC | 支持 Security Group、WAF、VPC Endpoint、IAM 角色、Secrets Manager | 支持 Firewall Rules、WAF、Private Service Connect、IAM、Secret Manager |
| 身份与访问管理     | Azure AD、Managed Identity、RBAC、条件访问策略      | IAM、Cognito、IAM 角色、条件策略                  | IAM、Identity Platform、服务账号、条件策略        |
| 证书与密钥管理     | Key Vault（集中管理证书/密钥/机密）                | Secrets Manager、KMS、Certificate Manager         | Secret Manager、KMS、Certificate Manager          |
| 应用与私有网络集成 | Private Endpoint（App Service/Function/Vault等）    | VPC Endpoint（Interface/Gateway）、PrivateLink    | Private Service Connect、VPC Connector            |
| 混合网络集成       | 支持 VPN Gateway、ExpressRoute、VNet Peering、Site-to-Site VPN、AD DS同步 | 支持 VPN Gateway、Direct Connect、VPC Peering、Site-to-Site VPN、AD Connector | 支持 Cloud VPN、Interconnect、VPC Peering、AD同步  |
| 访问控制粒度       | 支持到资源组/资源级别（RBAC/NSG/ASG）              | 支持到资源/实例/服务级别（IAM/SG/标签）           | 支持到项目/资源级别（IAM/标签/防火墙规则）        |
| 审计与合规         | Activity Log、Monitor、Policy、Blueprint            | CloudTrail、Config、Organizations SCP             | Audit Logs、Policy Analyzer、Org Policy           |

**总结：**
- 三大云厂商均支持丰富的应用安全配置（WAF、密钥管理、身份集成）和混合网络集成（VPN、专线、私有接入）。
- Azure 强调 RBAC、Key Vault、Private Endpoint 与混合网络无缝集成，适合企业级安全与合规需求。
- AWS 以 IAM、Security Group、PrivateLink、Secrets Manager 为核心，混合网络能力成熟。
---

## ✅ Day 32 小测验总结（4 / 5）

### 1. App Service 如何使用 Key Vault 密钥？
- ❌ 你的答案：A（直接调用 API）
- ✅ 正确答案：B（使用 Key Vault 引用语法自动注入）

### 2. 允许 App Service 访问 VNet 的机制？
- ✅ C. Regional VNet Integration

### 3. Hybrid Connection 特点？
- ✅ A. 支持 TCP
- ✅ C. 使用 Service Bus Relay
- ❌ B/D 错误（不需对端开端口、不支持 Ping）

### 4. Private Endpoint 正确说法？
- ✅ B. 可通过 NSG 控制访问
- ✅ D. 需要绑定 private DNS zone

### 5. Traffic Manager 特点？
- ✅ A. 性能路由
- ✅ B. 区域容灾
- ✅ D. 权重/地理分流
- ❌ C. 它不是 IP 层负载服务

---

## 📅 下一步预告 - Day 33

| 模块 | 内容 |
|------|------|
| 高可用性与灾难恢复（HA/DR） | Availability Sets / Zones / Region Pairs |
| Azure Backup 与 Site Recovery | VM 备份 / 故障恢复演练 |
| SLA 保证与故障转移策略 | 设计高可用服务 |
