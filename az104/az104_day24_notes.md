
# AZ-104 学习笔记 - Day 24

## 🛡️ 今日主题：Azure Policy、Blueprint 与 RBAC 权限控制

---

## 📌 Azure Policy 简介

Azure Policy 是资源治理工具，可用于控制资源部署行为与强制策略合规性。

| 类型       | 描述 |
|------------|------|
| 内建策略   | Microsoft 提供的常见策略（禁止特定 SKU、强制加密等） |
| 自定义策略 | 使用 JSON 自定义规则 |
| 修正功能   | 可选择自动修正已有资源 |
| 合规报告   | 提供资源扫描与不合规项详情 |

### 示例策略片段：限制创建非特定 SKU 的虚拟机

```json
"if": {
  "allOf": [
    { "field": "type", "equals": "Microsoft.Compute/virtualMachines" },
    { "field": "Microsoft.Compute/virtualMachines/sku.name", "notIn": ["Standard_DS1_v2", "Standard_B2s"] }
  ]
},
"then": {
  "effect": "deny"
}
```

---

## 📦 Azure Blueprint

Blueprint 是策略、权限与资源模板的组合包，用于标准化部署与合规性保障。

| 组成内容 | 说明 |
|----------|------|
| Azure Policy | 强制策略 |
| RBAC        | 分配访问权限 |
| ARM 模板    | 部署资源 |
| 资源锁      | 防止关键资源误删 |

适用于：多订阅/多环境的一致性部署。

---

## 👥 Role-Based Access Control（RBAC）

Azure 中的权限控制体系，基于角色赋予用户/服务主体操作权限。

### 关键组件

| 组件             | 说明 |
|------------------|------|
| Role Definition  | 权限集合（如 Contributor） |
| Role Assignment  | 将角色赋予用户、组、服务主体 |
| Scope            | 权限作用范围（管理组/订阅/资源组/资源） |

### 常用内建角色

| 角色                          | 权限说明 |
|-------------------------------|----------|
| Owner                         | 所有权限（含分配权限） |
| Contributor                   | 所有资源操作，但不可分配权限 |
| Reader                        | 查看权限 |
| User Access Administrator     | 管理权限分配 |
| Storage Blob Data Reader      | 对 Blob 的读取权限 |

---

## 🧰 自定义角色

用于满足特定需求的权限定义：

```json
{
  "Name": "CustomVMStarter",
  "Actions": [ "Microsoft.Compute/virtualMachines/start/action" ],
  "AssignableScopes": [ "/subscriptions/xxxx-xxxx" ]
}
```

---

## ✅ 最小权限设计策略

- 使用 **Reader/Contributor** 等角色代替 Owner
- 控制 **Scope** 范围（资源组级、资源级）
- 建立 **自定义角色** 精确控制权限
- 搭配 **Policy + RBAC + Blueprint** 做整体治理
---

## 🌐 Azure、AWS、GCP 管理策略与权限控制对比

| 维度             | Azure                                   | AWS                                         | GCP                                         |
|------------------|-----------------------------------------|---------------------------------------------|---------------------------------------------|
| 策略治理         | Azure Policy（资源合规、自动修正）      | AWS Organizations SCP、Config、Service Control Policies | Organization Policy、Org Policy Constraints |
| 组合治理         | Blueprint（策略+权限+模板+资源锁）      | Control Tower（Landing Zone、SCP、模板）    | 组织策略+Resource Manager模板               |
| 权限控制模型     | RBAC（角色/作用域分配）                 | IAM Policy（策略/角色/资源作用域）          | IAM Policy（角色/成员/资源作用域）          |
| 自定义角色       | 支持，精细到 API 操作                   | 支持，精细到 API 操作                       | 支持，精细到 API 操作                       |
| 作用域           | 管理组/订阅/资源组/资源                 | 组织/账户/资源                              | 组织/文件夹/项目/资源                       |
| 最小权限原则     | 支持，推荐自定义角色+精细 Scope         | 支持，推荐最小权限策略                      | 支持，推荐最小权限策略                      |
| 合规与审计       | Policy 合规报告、Activity Log            | Config、CloudTrail、合规报告                | Audit Logs、Policy Analyzer                  |
| 自动修正         | 支持（DeployIfNotExists、Modify等）      | 支持（Config Rules、Lambda自动修正）         | 支持（Policy Constraints、自动修正脚本）     |
| 模板化部署       | ARM/Bicep、Blueprint                    | CloudFormation、Control Tower               | Deployment Manager、Terraform                |

**总结：**
- 三大云厂商均支持策略治理、权限分配、合规审计和自动修正，均可实现企业级资源与权限统一管理。
- Azure Policy/Blueprint、AWS SCP/Config/Control Tower、GCP Org Policy/Resource Manager 各有特色，建议结合组织规模、合规需求和自动化程度选择最佳实践。
- RBAC/IAM 支持自定义角色和多层级作用域，便于实现最小权限和精细化授权
---

## ✅ 小测验回顾（4.5 / 5）

### 1. Azure Policy 正确说法？
- ✅ A. 可阻止不合规资源被创建

### 2. Blueprint 可包含内容？
- ✅ A. Azure Policy  
- ✅ B. 角色权限分配  
- ✅ D. ARM 模板部署

### 3. RBAC 正确说法？
- ✅ A. 一个用户可拥有多个角色  
- ✅ B. Role Assignment 可作用于资源组  
- ✅ D. 自定义角色支持 Assignable Scope

### 4. 哪个不是内建角色？
- ✅ C. CustomAdminRole

### 5. 最小权限设计应使用？
- ✅ A. 自定义角色  
- ✅ B. RBAC 限制作用范围  
- ✅ C. Blueprint 统一部署  
- ❌ D. ❌（Owner 权限太高，不推荐）

---

## 📅 下一步预告 - Day 25

| 模块 | 内容 |
|------|------|
| Azure 文件与对象存储 | Blob、File、Share 差异与场景 |
| 生命周期管理与加密策略 | 自动归档、SSE 加密 |
| 安全控制策略 | 防火墙、私有端点、SAS/角色权限 |

