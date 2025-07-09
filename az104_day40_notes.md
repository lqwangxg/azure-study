# AZ-104 学习笔记 - Day 40

## 🌐 混合云与多租户治理方案

---

## 🔗 Azure Arc

### ✅ 功能与优势
- 将本地服务器、Kubernetes 集群、SQL 实例等**注册为 Azure 资源**
- 为非 Azure 资源分配：
  - **RBAC 权限**
  - **策略（Policy）**
  - **标签（Tag）**
- 支持混合云（本地 + AWS + GCP + 私有云）统一治理

### ⚠️ 注意
- Arc 并不控制资源的物理状态（如开关机）
- 属于逻辑层治理方案，不提供底层 IaaS 管理能力

---

## 🧭 Azure Lighthouse

### ✅ 核心特性
- **MSP 场景专用**：服务商可跨租户管理多个客户 Azure 环境
- 服务提供商通过自己的账号访问客户订阅资源
- 权限管理通过：
  - Azure Delegated Resource Management
  - ARM Template 授权配置

### ✅ 优势
- RBAC 控制精细权限，客户可随时撤回
- 管理者操作透明，客户侧可审计操作日志
- 可使用 Azure Monitor / Log Analytics 集中管理

---

## 💡 Azure Advisor

### ✅ 提供 5 类建议
| 类别       | 示例说明                                 |
|------------|------------------------------------------|
| 成本节省   | 降配低利用率 VM、释放未使用的公有 IP      |
| 安全性     | 开启 Defender、关闭不必要端口              |
| 性能优化   | 升级 SKU、增加实例数                       |
| 高可用性   | 增加 Availability Set 或 Zone 冗余         |
| 操作卓越   | 建议标准化部署、标签不一致提醒             |

---

## 🛡 Microsoft Defender for Cloud（原 Azure 安全中心）

### ✅ 功能
- 提供 Azure 和多云环境的 **威胁防护 + 安全建议**
- 支持资源：
  - 虚拟机（Windows/Linux）
  - 容器、Kubernetes
  - 数据库（SQL、Cosmos DB 等）
  - Storage 帐户等

### ✅ 分层结构
| 层级      | 功能说明                                   |
|-----------|--------------------------------------------|
| 免费层    | 安全评分、安全建议（无自动防护）           |
| 付费层    | 启用高级威胁检测、集成 SIEM、安全警报响应  |

### ✅ 联动能力
- 与 **Azure Policy** 联动，自动修复合规性问题
- 可通过 Log Analytics 集成 SIEM
---

## 🌐 Azure、AWS、GCP 关于混合云与多租户治理方案对比

| 维度               | Azure                                              | AWS                                              | GCP                                              |
|--------------------|----------------------------------------------------|--------------------------------------------------|--------------------------------------------------|
| 混合云统一治理     | Azure Arc（本地/多云资源纳管、策略/标签/RBAC）     | AWS Systems Manager、AWS Control Tower（部分多云纳管，主要 AWS 内资源） | Anthos（K8s 多云/本地统一管理）、Config Connector |
| 多租户管理         | Azure Lighthouse（MSP 跨租户集中管理、RBAC、审计） | AWS Organizations（多账户管理、SCP、集中计费）、Resource Access Manager（RAM） | 组织结构（Org/Folder/Project）、IAM、Resource Manager |
| 策略与合规         | Azure Policy、Blueprint、Defender for Cloud         | SCP、Config、Security Hub、GuardDuty              | Org Policy、Policy Constraints、Security Command Center |
| 访问与权限控制     | RBAC、PIM、Delegated Resource Management            | IAM、SCP、RAM、条件策略                            | IAM、条件策略、服务账号、委托管理                  |
| 多云/本地资源支持  | 支持本地、AWS、GCP、私有云等多云资源统一治理        | 部分 AWS 服务支持第三方云纳管（功能有限）           | Anthos 支持多云/本地 K8s，部分服务支持多云集成      |
| 审计与监控         | Activity Log、Monitor、Log Analytics、SIEM 集成     | CloudTrail、CloudWatch、Config、Security Hub       | Audit Logs、Cloud Monitoring、Security Command Center |
| MSP 支持           | Azure Lighthouse（服务商专用，客户可审计/撤权）     | 支持 MSP 方案（需自建多账户/权限委托）              | 支持委托管理（需自建项目/权限委托）                 |

**总结：**
- Azure Arc 和 Lighthouse 提供了本地/多云资源统一治理和多租户集中管理的完整方案，适合企业和 MSP 场景。
- AWS 以 Organizations、RAM、Control Tower 为主，支持多账户集中管理和部分多云纳管，混合云能力逐步增强。
- GCP 以 Anthos 为核心，专注于多云/本地 K8s 管理，组织结构和 IAM 支持多租户治理。
- 策略、合规、审计和权限控制能力三大云厂商均完善，建议结合实际多云/混合云和多租户需求选择最佳方案。
---

## ✅ 小测验复盘（得分：13 / 16）

### 问题 1：Azure Arc
✅ A, C  
❌ B：无法控制电源  
❌ D：Arc 支持多云与本地资源

### 问题 2：Azure Lighthouse
✅ A, B, D  
❌ C：基于 RBAC 控制权限

### 问题 3：Azure Advisor
✅ A, B, C  
❌ D：Advisor 不提供“数据持久性”类建议

### 问题 4：Defender for Cloud
✅ A, B, D  
❌ C：免费层功能有限，付费层提供完整防护

### 问题 5：多云/多租户治理
✅ A, C  
❌ B：Advisor 是建议工具  
❌ D：Site Recovery 属于灾备工具

---

## 📘 下一步预告 - Day 41 模块

| 模块                       | 内容概览                           |
|----------------------------|------------------------------------|
| 自动化模板（ARM/Bicep）   | 基础结构即代码（IaC）实践           |
| 自定义脚本扩展与部署      | 虚机初始化与脚本安装                |
| Azure Blueprints           | 定义一套组合策略 + 模板 + RBAC 快速部署 |
