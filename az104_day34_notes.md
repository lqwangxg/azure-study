
# AZ-104 学习笔记 - Day 34

## 🏛️ Azure Policy、治理机制与访问控制总结

---

## 📜 Azure Policy

### 🔹 用途
- 实施资源配置与部署规则
- 实现合规性控制与自动修正

### 🔹 核心元素

| 概念             | 说明 |
|------------------|------|
| Policy Definition | 策略定义，JSON 格式，描述规则与操作 |
| Initiative        | 多策略集合（策略包） |
| Assignment        | 策略应用范围（如资源组、订阅） |
## 📦 Azure Policy Initiative 使用场景详解

**Initiative**（策略集）是将多个相关的 Policy Definition 组合在一起，便于批量分配和统一管理合规要求。适用于以下场景：

- **企业合规与标准化**：企业需同时强制多项合规要求（如标签、区域、加密、SKU 限制等），通过 Initiative 一次性分配，确保所有资源统一受控。
- **多环境一致性治理**：开发、测试、生产等环境需应用相同的合规策略，使用 Initiative 可批量下发，避免遗漏和配置偏差。
- **合规报告与审计**：Initiative 可统一生成合规性报告，便于集中审计和整改。
- **Landing Zone 快速落地**：为新订阅或资源组自动应用一组基础治理策略（如强制诊断日志、限制公网 IP、必须打标签等）。

### 📋 示例：为订阅分配“基础合规策略集” Initiative

**需求**：所有资源必须满足以下要求：
- 必须包含 `Environment` 和 `Owner` 标签
- 虚拟机只能部署在 Japan East 区域
- 存储账户必须启用加密

**Initiative 结构**：

- Initiative 名称：`基础合规策略集`
- 包含 Policy Definitions：
  1. `必须有 Environment 标签`
  2. `必须有 Owner 标签`
  3. `仅允许 Japan East 区域部署`
  4. `存储账户必须启用加密`

**实际操作流程**：
1. 管理员在 Azure Portal 创建 Initiative，添加上述 Policy Definitions。
2. 将 Initiative 分配（Assignment）到目标订阅或资源组。
3. 所有新建或现有资源自动接受合规性检查，不合规项会被阻止或记录，支持自动修正。

**效果**：
- 一次性下发多项策略，提升治理效率和一致性。
- 合规性报告集中展示，便于持续审计和整改。
- 降低人为疏漏风险，实现企业级资源合
### 🔹 常见策略用途
- 限定资源部署区域（如只允许日本东部）
- 限定 VM SKU 大小（如只允许 B 系列）
- 强制资源包含标签（如 `Environment` 标签）
- 自动部署诊断设置或开启日志（使用 deployIfNotExists）

### 🔹 Policy Effect 类型

| Effect             | 作用说明 |
|--------------------|----------|
| deny               | 拒绝部署不符资源 |
| audit              | 记录非合规行为，不阻止部署 |
| disabled           | 策略失效 |
| modify             | 自动修改资源配置 |
| deployIfNotExists  | 自动部署关联资源（如诊断设置） |

---

## 🏢 管理组（Management Groups）

- 用于组织多个订阅，统一管理和策略下发
- 支持多层嵌套结构，策略和权限自顶向下继承
- 限定在 **同一租户** 内使用，不能跨 Tenant
- 示例结构：
```
Root MG
├── Prod MG
│   ├── Sub-A
│   └── Sub-B
└── Dev MG
    └── Sub-C
```

---

## 🔒 资源锁（Locks）

- 保障关键资源不被意外删除或修改
- 两种类型：
  - **CanNotDelete**：允许修改，不允许删除
  - **ReadOnly**：禁止修改与删除

📌 即使用户为 Owner，也必须先解锁才能修改/删除！

适用范围：订阅、资源组、单一资源

---

## 🔐 权限控制机制总结（IAM）

### 🔸 RBAC（Role-Based Access Control）

| 角色        | 权限说明 |
|-------------|----------|
| Owner       | 完全控制 + 分配权限 |
| Contributor | 可修改资源，无权限控制 |
| Reader      | 只读 |
| 自定义角色  | 精细控制资源与操作权限 |

适用范围：管理组 > 订阅 > 资源组 > 资源

---

### 🔸 PIM（Privileged Identity Management）

- 为高权限角色提供临时、受控访问
- 核心功能：
  - Just-in-time 激活
  - MFA 多重验证
  - 审核日志记录
  - 避免长期 Owner 权限暴露
---

## 🌐 Azure、AWS、GCP 关于治理机制与访问控制的对比

| 维度             | Azure                                              | AWS                                              | GCP                                              |
|------------------|----------------------------------------------------|--------------------------------------------------|--------------------------------------------------|
| 资源分层         | Management Group → Subscription → Resource Group → Resource | Organization → Account → OU → Resource           | Organization → Folder → Project → Resource       |
| 策略治理         | Azure Policy（资源合规、自动修正）、Blueprint      | Organizations SCP、Config、Control Tower          | Organization Policy、Policy Constraints           |
| 权限模型         | RBAC（基于角色的访问控制）、自定义角色             | IAM Policy（基于策略/角色）、自定义策略           | IAM Policy（基于角色/成员）、自定义角色           |
| 作用域           | 管理组/订阅/资源组/资源                            | 组织/OU/账户/资源                                 | 组织/文件夹/项目/资源                            |
| 条件访问         | 支持条件访问策略、PIM、MFA                         | 支持条件策略、MFA、SCP                            | 支持条件 IAM、组织策略、MFA                      |
| 审计与合规       | Activity Log、Monitor、Policy 合规报告              | CloudTrail、Config、合规报告                      | Audit Logs、Policy Analyzer、合规报告             |
| 自动修正         | 支持（DeployIfNotExists、Modify等）                 | 支持（Config Rules、Lambda自动修正）              | 支持（Policy Constraints、自动修正脚本）          |
| 模板化部署       | ARM/Bicep、Blueprint                               | CloudFormation、Control Tower                    | Deployment Manager、Terraform                     |
| 访问控制粒度     | 支持到资源组/资源级别（RBAC/NSG/ASG）              | 支持到资源/实例/服务级别（IAM/SG/标签）           | 支持到项目/资源级别（IAM/标签/防火墙规则）        |

**总结：**
- 三大云厂商均支持分层资源治理、策略合规、细粒度访问控制和自动修正。
- Azure 强调 RBAC、Policy、Blueprint 组合治理，AWS 以 IAM、SCP、Config、Control Tower 为核心，GCP 以 IAM、Org Policy、Policy Constraints 为主。
- 审计、合规、模板化部署和条件访问能力均完善，建议结合组织规模、合规和自动化需求选择最佳治理方
---

## ✅ Day 34 小测验（3 / 5）

### 1. Azure Policy 的用途：
- ❌ 你的答案：A, B, C, D
- ✅ 正确答案：A, C, D  
> B 项为 Azure AD 中的条件访问控制，不属于 Policy 范畴

### 2. 部署诊断设置需用哪个 Effect？
- ✅ C. deployIfNotExists

### 3. 关于资源锁描述：
- ✅ A. ReadOnly 锁禁止修改与删除  
- ✅ B. CanNotDelete 锁允许修改但禁止删除  
- ✅ D. Owner 也不能绕过锁

### 4. 管理组相关：
- ❌ A 错（不可跨租户）  
- ✅ B. 可集中治理  
- ✅ C. 策略继承至订阅

### 5. PIM 功能：
- ✅ A. Just-in-time 激活  
- ❌ B 错（PIM 不提供永久权限）  
- ✅ D. 避免长期高权限  
- ❌ 你漏选 C（审核记录）

---

## 📅 下一步预告 - Day 35

| 模块 | 内容 |
|------|------|
| Azure Resource Graph | 资源检索与分析 |
| 模板部署比较         | ARM Template / Bicep / Terraform |
| Azure Blueprint      | 治理模板自动化（可选） |

