
# AZ-104 学习笔记 - Day 37

## 🧩 Azure 权限与治理功能总览

---

## ✅ RBAC（Role-Based Access Control）

### 📌 核心概念
- **角色**：定义权限（如 Reader、Contributor、Owner 等）
- **分配范围**：订阅 > 资源组 > 资源
- **继承性**：上层范围权限自动继承到下层

### 📌 常见内置角色
| 角色名称            | 权限说明                              |
|---------------------|---------------------------------------|
| Owner               | 拥有全部权限，包括权限分配              |
| Contributor         | 可创建/管理资源，**不能分配权限**       |
| Reader              | 仅读取资源                            |
| User Access Admin   | 仅管理权限分配，不能创建资源            |

### ✅ 自定义角色
- 可限制资源类型、名称、区域等
- 仅支持订阅或资源组级别创建
- 使用 JSON 定义操作与限制范围

---

## 📘 Azure Policy

### 📌 作用
- 强制资源符合组织规范（如标签、区域、SKU）
- 检查与评估现有资源合规性
- 支持自动修正（deployIfNotExists、modify）

### 📌 示例策略用途
- 限制部署区域（只能部署在 Japan East）
- 所有资源必须包含 `Environment=Prod` 标签
- 虚拟机必须启用备份或监控

---

## 🎛 Azure Blueprints（蓝图）

### 📌 概要
- 一种组合治理模板工具，包含：
  - ARM 模板
  - Azure Policy
  - Role Assignments
  - 资源锁（Locking）
- 应用于订阅后会自动部署模板资源
- 支持版本控制与锁定（防止手动修改）
## 🎛 Azure Blueprints 使用场景详解

Azure Blueprints 适用于需要在多个订阅或环境中**标准化资源部署、权限分配和合规策略**的场景，常见于企业级、合规性要求高、DevOps 自动化和多团队协作的云治理体系中。主要使用场景包括：

- **企业合规与标准化**：确保所有新建订阅或资源组都自动应用统一的安全策略、标签、网络结构、权限分配等，满足合规要求（如 ISO、CIS）。
- **多环境一致性**：开发、测试、生产等环境可通过 Blueprint 快速复制一致的基础架构和策略，避免人为配置偏差。
- **快速落地 Landing Zone**：为新业务线、子公司或项目组自动化交付一套合规的云基础架构（如网络、监控、策略、权限等）。
- **权限与资源模板一体化交付**：将 ARM/Bicep 模板、Policy、RBAC、资源锁等组合为一体，便于集中管理和版本控制。

### 📦 示例：为新订阅自动部署合规 Landing Zone

**需求**：每个新订阅都必须具备以下要求：
- 资源组结构标准化
- 强制所有资源打标签（如 Environment、Owner）
- 限制 VM 只能部署在指定区域
- 自动分配安全管理员和只读用户角色
- 关键资源加锁防止误删

**Blueprint 结构示意**：

- **Artifacts（蓝图组件）**：
  - Resource Group（如：CoreInfra、AppInfra）
  - Policy Assignment（如：必须有标签、限制区域）
  - Role Assignment（如：安全管理员、只读用户）
  - ARM/Bicep 模板（如：VNet、Log Analytics）
  - Resource Lock（如：防止删除 VNet）

**实际操作流程**：
1. 管理员在 Azure Portal 创建 Blueprint，添加上述组件。
2. 发布 Blueprint 并分配到目标订阅。
3. 系统自动部署资源组、策略、权限和锁，确保新环境合规、标准、可审计。

**效果**：  
- 新订阅上线即具备企业标准的安全、合规和资源结构。
- 管理员可统一变更和版本控制 Blueprint，批量更新所有环境。
- 降低人为配置风险，提高云治理效率
---

## 🛡️ Microsoft Defender for Cloud

### 📌 功能模块
- 安全评分：基于资源配置打分
- 安全建议：如开放端口、无加密存储等
- 合规检查：评估 ISO、NIST、CIS 等标准的合规性
- 威胁防护：入侵检测、防挖矿、漏洞分析等

### 📌 支持对象
- Azure 原生资源（VM, Storage, App Service 等）
- 多云与本地资源（AWS、GCP、On-Prem VM）

---

## 🌐 Azure、AWS、GCP 关于云安全中心（类似 Microsoft Defender for Cloud）的对比

| 维度                 | Azure Defender for Cloud                      | AWS Security Hub & GuardDuty                | GCP Security Command Center (SCC)            |
|----------------------|-----------------------------------------------|---------------------------------------------|----------------------------------------------|
| 威胁检测             | 支持（内置威胁情报、行为分析）                | 支持（GuardDuty，威胁检测、异常分析）       | 支持（SCC，威胁检测、异常分析）              |
| 安全评分与建议       | 安全评分、合规建议、修复指导                  | 安全评分、合规建议、修复指导                | 风险评分、合规建议、修复指导                 |
| 多云/混合支持        | 支持 AWS、GCP、On-Prem 资源                   | 支持部分第三方集成                          | 支持 AWS、Azure 资源集成（部分功能）         |
| 合规性评估           | 内置多种合规标准（CIS、ISO、PCI 等）           | 内置多种合规标准（CIS、PCI、GDPR 等）        | 内置多种合规标准（CIS、PCI、GDPR 等）         |
| 自动修正             | 支持（建议+自动修正脚本/逻辑应用）            | 支持（自动修正建议、部分自动化）             | 支持（自动修正建议、部分自动化）              |
| 集成能力             | 与 Azure Policy、Sentinel、Monitor、Logic App | 与 CloudWatch、Config、SNS、Lambda 集成      | 与 Cloud Logging、Cloud Functions 集成        |
| 资产发现             | 自动发现 Azure 及多云资产                     | 自动发现 AWS 资产                            | 自动发现 GCP 资产                            |
| 定制检测规则         | 支持（自定义警报、规则、工作簿）              | 支持（自定义检测、警报）                     | 支持（自定义检测、警报）                      |
| 费用模式             | 按资源/功能计费，部分功能免费                  | 按资源/功能计费，部分功能免费                | 按资源/功能计费，部分功能免费                 |

**总结：**
- 三大云平台均提供统一的安全中心，集成威胁检测、合规评估、安全评分和自动修正能力。
- Azure Defender for Cloud 支持多云和本地资源，集成度高，自动修正和合规能力突出。
- AWS Security Hub + GuardDuty 组合实现统一安全态势感知和威胁检测，自动化能力强。
- GCP Security Command Center 提供风险发现
---

## ✅ 小测验复盘（得分：11 / 13）

### 问题 1：RBAC 正确说法
✅ A, B, C  
❌ D：内置角色不可删除或修改

### 问题 2：拥有全部权限角色
✅ C：Owner

### 问题 3：Azure Policy 的功能
✅ A, D  
❌ B：修正是正确的，应纳入答案  
❌ C：Policy 不能替代 RBAC 管理权限

### 问题 4：Blueprint 特点
✅ A, B, D  
❌ C：Blueprint 包含 Policy，但作用更广

### 问题 5：Microsoft Defender
✅ B, D  
❌ A：NSG 检查也是 Defender 功能，漏选  
❌ C：免费版功能有限，无法覆盖所有高级防护

---

## 📌 下一步预告 - Day 38 重点模块

| 模块                           | 内容                                           |
|--------------------------------|------------------------------------------------|
| Azure Bastion 与 JumpBox       | 安全远程访问 Azure VM                         |
| 防火墙与网络安全组（NSG）      | 出入站规则、端口过滤、默认优先级              |
| DDoS 防护                     | Basic vs Standard 差异                        |
| 网络隔离与 Private Endpoint   | 服务私网接入方式、安全域划分                   |
