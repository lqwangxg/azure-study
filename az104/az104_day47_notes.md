# AZ-104 学习笔记 - Day 47

## 🛡️ Azure Policy 与资源合规控制

---

## ✅ Azure Policy vs RBAC 区别

| 项目               | Azure Policy                              | Azure RBAC（角色权限控制）             |
|--------------------|-------------------------------------------|----------------------------------------|
| 控制对象           | 资源的部署行为（资源 *怎么建*）           | 访问权限（资源 *谁能访问*）            |
| 示例功能           | 限制 VM SKU、必须加标签、禁止某区域部署   | 谁能读写虚拟机、谁能删除存储账户       |
| 应用场景           | 合规性、强制标准                          | 授权与身份验证                         |
| 可组合性           | 可通过 Initiative 实现策略集               | 可通过 Role 绑定多个权限               |

---

## 📚 Azure Policy 结构组成

- **Policy Definition**：定义规则内容（如：不允许 East Asia 区域）
- **Policy Assignment**：将定义套用到范围（如：订阅、资源组）
- **Parameters**：定义策略参数（如 region 列表）
- **Initiative Definition**：策略集，组合多个策略统一管理

---

## 🧠 常见内建 Policy 示例

| 策略类型                         | 功能说明                                 |
|----------------------------------|------------------------------------------|
| 限定 Region 部署                 | 限制只能部署在特定区域                   |
| 限定资源 SKU                     | 限制只能使用某些 VM 等规格               |
| 强制打标签                       | 没有标签的资源无法部署                   |
| 强制开启诊断日志                 | 确保资源生成监控日志                     |

---

## 🌐 Azure、AWS、GCP 关于 Policy 与资源合规控制的对比

| 维度             | Azure Policy & Initiative                      | AWS Organizations SCP & Config & Config Rules         | GCP Organization Policy & Policy Constraints         |
|------------------|------------------------------------------------|------------------------------------------------------|------------------------------------------------------|
| 策略定义         | Policy Definition（JSON），可参数化            | SCP（策略）、Config Rule（合规规则，支持自定义 Lambda） | Org Policy（YAML/JSON）、Policy Constraints          |
| 策略组合         | Initiative（策略集，统一分配与管理）           | 多策略组合，SCP+Config+Tag Policies                  | 多约束组合，支持层级继承                             |
| 作用范围         | 管理组、订阅、资源组、资源                     | 组织、OU、账户、资源                                 | 组织、文件夹、项目、资源                             |
| 合规性评估       | 合规报告、自动修正（DeployIfNotExists/Modify）  | 合规报告、自动修正（Config Rule + Lambda）           | 合规报告、自动修正（Constraint + 自动化脚本）        |
| 资源标签强制     | 支持（Policy 强制打标签）                      | 支持（Tag Policies）                                 | 支持（标签策略）                                     |
| 内建策略         | 丰富的内建策略库，支持自定义                   | 丰富的内建规则库，支持自定义                         | 丰富的内建约束，支持自定义                           |
| 审计与追踪       | Activity Log、Policy 合规报告                   | CloudTrail、Config 合规报告                          | Audit Logs、Policy Analyzer                          |
| 自动化集成       | 可与 Logic App、Automation、Monitor 集成        | 可与 Lambda、SNS、CloudWatch 集成                    | 可与 Cloud Functions、Cloud Monitoring 集成          |
| 多云/混合治理    | 支持（Azure Arc 可扩展到多云/本地资源）         | 部分支持（Config/Control Tower 可扩展部分第三方）     | Anthos/Config Connector 支持多云部分资源             |

**总结：**
- 三大云厂商均支持策略定义、合规评估、自动修正和审计追踪，适合企业级资源治理与合规管控。
- Azure Policy 强调参数化、策略集（Initiative）和自动修正，AWS 以 SCP+Config+Lambda 组合灵活，GCP 以 Policy Constraints 和层级继承为特色。
- 多云/混合治理 Azure Arc 能力最强，AWS/GCP 也有部分多云扩展能力。
- 建议结合组织结构、合规要求和自动化集成需求选择最佳实
---

## 🧪 小测验复盘（得分：13 / 20）

### 问题 1：Azure Policy 的作用  
✅ B, C  
❌ A：Policy 控制资源部署，不控制访问权限  
❌ D：与 RBAC 控制对象不同

### 问题 2：Policy 组成部分  
✅ A, B  
❌ C：也正确，应包括在内  
❌ D：诊断设置不是 Policy 组成

### 问题 3：Assignment 行为  
✅ A, B, D  
❌ C：Assignment 不会修改定义内容

### 问题 4：内建策略  
✅ A, B, C  
❌ D：用户登录 VM 权限归 RBAC 控制

### 问题 5：Initiative 优势  
✅ A, B, D  
❌ C：可重用策略，无需重新写 Definition

---

## 🔜 下一步预告 - Day 48 模块

| 模块                     | 内容说明                               |
|--------------------------|----------------------------------------|
| Azure Resource Graph     | 用于跨订阅查询资源                     |
| 使用 KQL 查询资源        | 学习基础语法、聚合、筛选等操作         |
| 与 Azure Policy 整合     | 可配合做合规性报告和非一致性审计       |
