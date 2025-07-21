# AZ-104 学习笔记 - Day 35

## 📊 Azure Resource Graph 与模板部署工具比较

---

## 🔍 Azure Resource Graph（ARG）

### ✅ 主要用途
- 查询全 Azure 租户/订阅/资源组下资源信息
- 制作资源分布报告、清单
- 审计资源配置状态（非运行状态）

### 💡 特点
- 使用 **KQL（Kusto Query Language）**
- 快速、跨订阅查询能力强
- 支持 Portal、CLI、PowerShell 查询

### 🧪 示例查询

```kusto
Resources
| where type =~ "microsoft.compute/virtualmachines"
| project name, location, resourceGroup, properties.hardwareProfile.vmSize
```

```kusto
Resources
| summarize count() by location
```

---

## 🛠️ Azure 模板部署工具对比

| 特性             | ARM Template     | Bicep              | Terraform              |
|------------------|------------------|---------------------|-------------------------|
| 类型             | JSON 模板        | Azure 原生 DSL      | 第三方跨云工具         |
| 可读性           | 差               | 高                  | 高                      |
| 状态管理         | 无               | 无                  | ✅ 有状态文件（state）  |
| 跨平台           | Azure 专用       | Azure 专用          | ✅ 支持 AWS、GCP、Azure |
| 模块化支持       | 支持但复杂       | ✅ 强                | ✅ 强                   |
| 是否原生         | ✅ 是             | ✅ 是                | ❌ 否                  |

---

## ✅ 工具适配建议

| 场景                           | 推荐工具         |
|--------------------------------|------------------|
| 快速 Azure 资源部署             | Bicep            |
| 大规模多云环境 IaC             | Terraform        |
| 复杂参数化与历史遗留系统        | ARM Template     |
---

## 🌐 Azure、AWS、GCP 关于模板部署工具的对比

| 维度           | Azure                                   | AWS                                         | GCP                                         |
|----------------|-----------------------------------------|---------------------------------------------|---------------------------------------------|
| 原生模板工具   | ARM Template（JSON）、Bicep（DSL）      | CloudFormation（YAML/JSON）                 | Deployment Manager（YAML/JSON/Python）      |
| 可读性         | ARM（一般）、Bicep（高）                | YAML/JSON（较高）                           | YAML（较高）、Python（可扩展）              |
| 状态管理       | 无（需外部管理）                        | 内置堆栈状态管理                            | 内置部署状态管理                            |
| 跨云能力       | 仅 Azure                                | 仅 AWS                                      | 仅 GCP                                      |
| 第三方工具     | 支持 Terraform、Pulumi 等                | 支持 Terraform、Pulumi 等                   | 支持 Terraform、Pulumi 等                   |
| 模块化         | Bicep/ARM 支持，Bicep模块化更强          | 支持模块、嵌套模板                          | 支持模块、模板嵌套                          |
| 生态与集成     | 与 Azure CLI、Portal、DevOps 深度集成    | 与 AWS CLI、Console、CodePipeline 集成      | 与 gcloud、Console、Cloud Build 集成         |
| 资源支持广度   | Azure 全部资源                          | AWS 全部资源                               | GCP 全部资源                                |
| 语法扩展性     | Bicep 支持参数化、循环、条件等           | 支持参数化、条件、宏                        | 支持参数化、模板扩展                        |

**总结：**
- 三大云厂商均有原生模板工具，支持参数化、模块化和自动化部署。
- Azure 推荐 Bicep（可读性高），AWS 推荐 CloudFormation（YAML/JSON），GCP 推荐 Deployment Manager（YAML/Python）。
- 跨云场景建议使用 Terraform、Pulumi 等第三方工具。
- 状态管理 AWS/GCP 原生支持，Azure 需借助第三方（如 Terraform）。
- 建议结合平台生态、团队技能和自动化需求选择最佳模板工具。
---

## 🧪 小测验解析（得分 4 / 5）

### 1. Azure Resource Graph 的典型用途：
- ❌ 你的答案：B, C, D  
- ✅ 正确答案：**B, C**  
> 资源成本分析属于 Azure Cost Management，不是 ARG 的功能

### 2. 哪些语言可用于模板部署：
- ✅ A. JSON（ARM）  
- ✅ C. Bicep（Azure DSL）  
- ✅ D. HCL（Terraform）  
> YAML 不适用 Azure Resource Manager 模板

### 3. Bicep vs ARM Template：
- ✅ B. Bicep 是对 ARM 的简化封装  
- ✅ D. Bicep 支持模块化更强  
> ARM 可读性差，且 Bicep 使用 `.bicep` 语法，而非 JSON

### 4. Terraform 与 Bicep 区别：
- ✅ B. Terraform 支持多云（AWS, GCP, Azure）

### 5. Resource Graph 正确说法：
- ✅ A. 可通过 Portal 查询  
- ✅ C. 查询资源配置元数据  
- ✅ D. 支持订阅、管理组级别  
> ❌ B 错：ARG 使用 KQL，而非 SQL

---

## 🔮 下一步预告：Day 36 - Azure Monitor 高级功能

| 模块                     | 内容                                  |
|--------------------------|---------------------------------------|
| Log Analytics 查询       | 自定义日志分析                        |
| Metrics vs Logs 对比     | 两者采集频率与用途差异                |
| 自定义指标（Custom Metrics） | 自建采集器上报指标                     |
| Application Insights 深度使用 | 分布式追踪、Live Metrics              |
