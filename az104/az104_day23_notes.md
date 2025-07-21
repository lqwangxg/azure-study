
# AZ-104 学习笔记 - Day 23

## ⚙️ 今日主题：Azure 自动化与灾难恢复（DR）策略

---

## ⏱️ 虚拟机自动化任务管理

| 功能         | 工具/方式 |
|--------------|-----------|
| 自动关机     | VM Blade 中直接配置 |
| 自动启动     | Automation Runbook |
| 批量启停     | Tag + Runbook 脚本 |
| 扩缩容       | Azure Monitor 告警 + Runbook/Logic App 联动 |

### 💡 示例：根据标签自动启动 VM 的 PowerShell Runbook

```powershell
$VMs = Get-AzVM -Tag @{ auto-start = "true" }
foreach ($vm in $VMs) {
    Start-AzVM -ResourceGroup $vm.ResourceGroupName -Name $vm.Name
}
```

---

## 🤖 Azure Automation 组件

| 组件               | 功能说明 |
|--------------------|----------|
| Automation Account | 核心托管环境 |
| Runbook            | 可计划执行的 PowerShell/Python 脚本 |
| Update Management  | 虚拟机补丁自动化部署与重启管理 |
| DSC (State Config) | 配置符合性（IaC 模式） |

## 🤖 Azure Automation Account 功能介绍

Azure Automation Account 是 Azure 上的自动化运维平台，主要功能包括：

- **Runbook**：支持 PowerShell、Python 脚本的自动化任务编排与定时执行，可实现批量启停 VM、资源清理、自动化运维等。
- **Update Management**：集中管理和自动部署 Windows/Linux 虚拟机的补丁和重启计划。
- **DSC（Desired State Configuration）**：实现资源配置一致性和合规性，自动纠正配置偏差（IaC 模式）。
- **凭据与变量管理**：安全存储自动化脚本所需的凭据、证书、变量等敏感信息。
- **作业日志与监控**：自动记录执行历史，便于审计和故障排查。

---

## 🛡️ Azure Site Recovery (ASR)

| 项目 | 描述 |
|------|------|
| 功能 | 虚拟机灾难恢复、区域级复制、自动故障转移 |
| 场景 | 数据中心灾害、地震容灾、跨区热备 |
| 与 Backup 区别 | Backup 用于数据恢复，ASR 用于业务连续性 |
| 支持类型 | Azure VM / 物理机 / VMWare / Hyper-V |

---

## 🔄 Azure Backup vs Azure Site Recovery 对比

| 项目 | Azure Backup | Site Recovery |
|------|--------------|---------------|
| 目标 | 数据恢复 | 灾难恢复（业务不中断） |
| 粒度 | 文件/磁盘级别 | VM 级别（整机） |
| 自动切换 | ❌ 不支持 | ✅ 支持 |
| 区域复制 | ❌ 限于同区域 | ✅ 可跨区域复制 |
| 恢复方式 | Portal 还原 | 故障转移至备用机 |

---

## 🧠 成本优化策略

| 场景 | 策略 |
|------|------|
| 长期开机空闲 VM | 设置自动关机 |
| 补丁/重启统一管理 | 使用 Update 管理 |
| 运维合规 | Runbook + DSC |
| 灾备 | 启用 Site Recovery 做热备 |
---

## 🔗 Azure Logic App 功能介绍

Azure Logic App 是一项无代码/低代码的自动化和集成服务，主要功能包括：

- **工作流自动化**：通过可视化设计器编排多步骤业务流程，无需编写代码。
- **事件驱动**：支持触发器（如定时、HTTP 请求、事件中心等）自动启动流程。
- **丰富连接器**：内置数百种连接器，支持与 Azure 服务、Office 365、SAP、SQL、邮件、第三方 API 等集成。
- **条件与分支**：支持条件判断、循环、并行、异常处理等复杂逻辑。
- **自动化运维**：常用于自动化告警响应、审批流、数据同步、跨系统集成等场景。

---

## 🌐 Azure、AWS、GCP 自动化与灾备能力对比

| 维度           | Azure                                    | AWS                                         | GCP                                         |
|----------------|------------------------------------------|---------------------------------------------|---------------------------------------------|
| 自动化平台     | Automation Account、Logic App、Functions | AWS Systems Manager、Lambda、Step Functions | Cloud Functions、Cloud Scheduler、Workflows |
| 自动化脚本     | Runbook（PowerShell/Python）、DSC         | SSM Run Command、Lambda、Step Functions     | Cloud Functions、Workflows                  |
| 工作流编排     | Logic App（可视化、低代码）               | Step Functions（可视化）、SWF               | Workflows（YAML/可视化）                    |
| 补丁/配置管理  | Update Management、DSC                   | Systems Manager Patch Manager、State Manager| OS Patch Management、Config Connector       |
| 灾难恢复       | Site Recovery（ASR）、Backup              | AWS Backup、Elastic Disaster Recovery（DRS）| Backup for GCE/Filestore、第三方方案        |
| 跨区域容灾     | 支持 VM 跨区域复制与自动故障转移          | 支持 EC2/EBS 跨区域复制与自动恢复           | 支持 VM/存储快照跨区域复制                  |
| 备份粒度       | 文件/磁盘/VM/数据库                       | 文件/卷/实例/数据库                         | 文件/磁盘/实例/数据库                       |
| 自动化触发     | 告警、定时、事件、Webhook                 | CloudWatch Events、SNS、定时、API           | Cloud Scheduler、事件、API                  |
| 集成能力       | 与 Azure 服务、第三方 API 深度集成        | 与 AWS 服务、第三方 API 深度集成            | 与 GCP 服务、第三方 API 深度集成            |

**总结：**
- 三大云厂商均提供自动化运维平台和灾备能力，支持脚本、工作流、补丁管理和自动化触发。
- Azure Automation Account 适合批量运维和配置管理，Logic App 强于无代码集成和业务流程自动化。
- AWS Systems Manager、Step Functions 功能丰富，GCP Workflows 适合云原生自动化。
- 灾备方面，Azure Site Recovery、AWS Elastic Disaster Recovery、GCP Backup/快照均支持跨区域容灾，建议结合实际业务需求选择
---

## ✅ 小测验回顾（5 / 5）

### 1. Runbook 可实现功能？
- ✅ B. 定时启动多个 VM

### 2. Automation Account 可管理？
- ✅ A. Runbook 脚本  
- ✅ B. 配置状态（DSC）  
- ✅ D. 补丁管理

### 3. Backup 与 Site Recovery 区别？
- ✅ A. ASR 用于灾难恢复，Backup 用于数据恢复  
- ✅ C. ASR 支持跨区域 VM 复制  
- ✅ D. Backup 可还原单文件，ASR 还原整机

### 4. 推荐的 VM 自动关机方式？
- ✅ B. 直接使用 VM Blade 中的自动关机功能

### 5. Automation 实现功能？
- ✅ A. 补丁更新计划  
- ✅ B. VM 自动启停  
- ✅ C. 自动扩容联动触发

---

## 📅 下一步预告 - Day 24

| 模块 | 内容 |
|------|------|
| Azure Policy 应用 | 管控资源配置，防止越权部署 |
| 蓝图（Blueprint） | 组合策略 + 模板的合规部署 |
| 基于角色访问控制 | RBAC 实践与策略组合 |

