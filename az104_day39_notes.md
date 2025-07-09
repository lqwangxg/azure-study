
# AZ-104 学习笔记 - Day 39

## 🧩 Azure 资源治理与成本控制

---

## 🔐 资源锁（Resource Locks）

### ✅ 两种类型
| 类型            | 功能描述                                      |
|------------------|-----------------------------------------------|
| CanNotDelete     | 禁止删除资源，**但可修改资源配置**             |
| ReadOnly         | 禁止修改和删除资源（相当于资源只读模式）       |

- 应用层级：可绑定在资源组或单个资源上
- 作用范围：适用于 Azure Portal、CLI、ARM Template、PowerShell 等所有接口
- 常用于生产环境、关键资源防误删

---

## 🔄 资源移动（Resource Move）

### ✅ 支持内容
- 跨资源组或订阅移动资源
- 必须在**同一租户（tenant）**内移动
- 大多数资源在移动时**保持可用**

### ⚠️ 限制与注意事项
- 并非所有资源类型都支持移动（如部分托管服务）
- 某些资源（如 Azure Key Vault）移动时需暂停服务
- 建议先在测试订阅验证再在生产环境移动

---

## 🏢 管理组（Management Group）

### ✅ 作用
- 多订阅的统一治理（应用策略与权限）
- 可嵌套多层结构（最多 6 层）
- 单个订阅仅可归属一个管理组
- 管理组可分配：
  - Azure Role-Based Access Control (RBAC)
  - Azure Policy（如统一限制部署区域）

### ❌ 注意
- 管理组**不用于资源迁移或数据流转**

---

## 💰 成本管理（Cost Management + Billing）

### ✅ 功能模块
- **预算设置**：设定预算阈值，发送告警（Email、Webhook）
- **成本分析**：可视化费用趋势、按资源组、服务分类查看
- **费用预测**：基于历史趋势，预估未来支出
- **成本报表导出**：支持自定义维度（标签、部门等）

### 🔒 权限说明
- 成本视图权限由 RBAC 控制，**不限制为管理员专用**

---

## 🛡 防止意外删除资源的机制

| 方法                 | 描述说明                              |
|----------------------|---------------------------------------|
| 资源锁（Locks）      | 物理防误删机制，限制删除/修改         |
| RBAC 权限控制        | 通过角色控制谁有权限删除资源           |
| Azure Policy          | 设定策略限制某类资源被删除或更改       |
| 自动备份（如 VM Backup） | 是灾难恢复手段，**不属于预防机制本身** |

---

## 🌐 Azure、AWS、GCP 关于资源治理与成本控制的对比

| 维度             | Azure                                              | AWS                                              | GCP                                              |
|------------------|----------------------------------------------------|--------------------------------------------------|--------------------------------------------------|
| 资源分层治理     | Management Group、Subscription、Resource Group      | Organization、Account、OU                        | Organization、Folder、Project                    |
| 策略与合规       | Azure Policy、Blueprint、Cost Management + Advisor  | Organizations SCP、Config、Budgets、Trusted Advisor | Organization Policy、Budgets、Policy Constraints  |
| 权限与分配       | RBAC、PIM、条件访问策略                            | IAM、SCP、条件策略、MFA                          | IAM、条件策略、MFA                               |
| 成本分析         | Cost Management（成本分析、预算、建议）            | Cost Explorer、Budgets、Cost & Usage Report      | Billing、Budgets、Cost Table、建议                |
| 预算与告警       | 支持预算、超支告警、成本分摊                       | 支持预算、超支告警、成本分摊                     | 支持预算、超支告警、成本分摊                     |
| 资源标签         | 支持多级标签、策略强制、成本归集                   | 支持标签、资源分组、成本归集                     | 支持标签、标签策略、成本归集                     |
| 预留与节省计划   | Reserved VM、Savings Plan、Advisor 节省建议         | Reserved Instance、Savings Plan、Spot 实例        | Committed Use Discount、Sustained Use Discount    |
| 自动关停与优化   | 自动关机、Advisor 优化建议、自动化脚本              | Instance Scheduler、Trusted Advisor 优化建议      | Recommender、自动关停脚本、优化建议              |
| 多云/混合治理    | 支持 Azure Arc、Defender for Cloud 多云治理         | 支持部分多云集成（Security Hub等）               | 支持部分多云集成（SCC等）                        |

**总结：**
- 三大云厂商均提供资源分层治理、策略合规、细粒度权限、成本分析与预算告警等能力。
- Azure Cost Management + Advisor、AWS Cost Explorer + Trusted Advisor、GCP Billing + Recommender 均可实现成本可视化与优化建议。
- 资源标签、预算、预留实例/节省计划等功能完善，建议结合组织规模、合规和成本优化需求选择最佳实
---

## ✅ 小测验回顾（得分：15 / 19）

### 问题 1：资源锁
✅ A, C, D  
❌ B：ReadOnly 锁不阻止读取

### 问题 2：资源移动
✅ A, C, D  
❌ B：大多数资源移动期间是可用的

### 问题 3：管理组
✅ A, B, C  
❌ D：管理组不用于数据迁移

### 问题 4：成本管理
✅ A, C, D  
❌ B：并非仅管理员才能查看费用数据

### 问题 5：防误删机制
✅ A, B, C  
❌ D：备份属于恢复手段，非预防机制

---

## 📘 下一步预告 - Day 40 模块

| 模块                             | 内容概览                                 |
|----------------------------------|------------------------------------------|
| Azure Arc                        | 混合与多云环境中的资源统一治理            |
| Azure Lighthouse                 | MSP（管理服务商）跨租户管理方案           |
| Azure Advisor + 安全中心建议     | 优化成本与安全的智能建议                  |
