
# AZ-104 学习笔记 - Day 5

## 🎯 学习目标
- 理解 Azure AD、用户、组、服务主体等概念
- 区分系统分配与用户分配的托管身份（Managed Identity）
- 掌握 RBAC（基于角色的访问控制）模型及 Scope 继承机制
- 使用 Azure CLI 分配角色权限
- 通过测验巩固身份与授权机制

---

## 👤 Azure AD 基础概念

| 概念 | 说明 |
|------|------|
| **Tenant（租户）** | 独立的 Azure AD 实例，表示一个组织 |
| **用户（User）** | 拥有账户的实体，可访问资源 |
| **组（Group）** | 用户集合，用于批量分配权限 |
| **服务主体（Service Principal）** | 应用程序或服务在 Azure AD 中的身份 |
| **应用注册** | 注册应用并分配 API 权限的入口 |

### 🧠 为什么需要 Service Principal？
在自动化部署或系统对系统的场景中（如 Terraform、GitHub Actions、Azure DevOps），  
我们不能使用真实用户账户（如 tom@company.com）长期持有权限。
这时候，我们就使用一个「服务主体」账号，它有自己的：
- Client ID（用户名）
- Client Secret（密码）
- 权限角色（如 Reader、Contributor）
### 🔐 它和 Azure AD 的关系？
- Azure AD 中每一个注册的 应用程序（Application）都可以对应一个或多个 服务主体（Service Principal）
- Application 是模板，Service Principal 是实例.  
  当你把应用授权给某个租户使用，就会创建一个服务主体，它才是真正用于身份验证和权限控制的对象。

### 🔧 Service Principal 结构
| 属性                          | 说明                                  |
|-----------------------------|-------------------------------------|
| AppId (Client ID)           | 服务主体的登录 ID，类似用户名                    |
| Client Secret / Certificate | 服务主体的密码或证书，用于认证                     |
| Tenant ID                   | 服务主体所在的 Azure AD 租户                 |
| Object ID                   | 服务主体在 Azure AD 中的唯一 ID              |
| 角色分配                     | 给它分配如 Contributor、Reader 等角色，控制访问权限 |


### ✅ 使用场景总结
| 场景                                     | 用途                                         |
|----------------------------------------|--------------------------------------------|
| CI/CD 工具部署资源                           | GitHub Actions / Azure DevOps 使用 SP 自动部署资源 |
| Terraform、Pulumi 登录 Azure              | 非交互式认证方式                                   |
| Azure SDK/CLI 自动化脚本                    | 让程序安全地访问 Azure API                         |
| 访问 Key Vault / Storage / Graph API 等服务 | 用于程序间的访问控制                                 |

### ⚠️ 安全建议
- 尽量使用 最小权限原则（只赋予需要的角色）
- 使用 短期有效的密码/证书，定期轮换
- 若在云端使用（如 GitHub），建议用 federated identity（身份联合）取代明文密码

### 🔐 安全补充建议
| 项目            | 建议                                                                  |
|---------------|---------------------------------------------------------------------|
| Client Secret | 建议设置过期时间，或使用证书认证                                      |
| 权限           | 不要默认使用 Contributor，仅赋予所需最小权限                          |
| 替代方案       | GitHub Actions、Azure DevOps 等支持用 Federated Identity 登录 Azure，无需明文密码 |

### 创建Service Principle 用terraform创建资源
1. Azure CLI 创建SP
      ```bash
      az ad sp create-for-rbac \
      --name "my-terraform-sp" \
      --role Contributor \
      --scopes /subscriptions/<YOUR_SUBSCRIPTION_ID>
      ```
      示例返回结果：
      ```json
      {
      "appId": "1111-2222-3333-4444",
      "displayName": "my-terraform-sp",
      "password": "xxxxyyyyzzzz",
      "tenant": "aaaa-bbbb-cccc-dddd"
      }
      ```
2.  Terraform 创建资源
    - variables.tf 
      ```hcl
      variable "client_id" {}
      variable "client_secret" {}
      variable "tenant_id" {}
      variable "subscription_id" {}
      ```
    - terraform.tfvars
      ```hcl
      client_id       = "1111-2222-3333-4444"
      client_secret   = "xxxxyyyyzzzz"
      tenant_id       = "aaaa-bbbb-cccc-dddd"
      subscription_id = "xxxx-yyyy-zzzz-wwww"
      ```
    - provider.tf 
      ```hcl
      provider "azurerm" {
            features {}
            client_id       = var.client_id
            client_secret   = var.client_secret
            tenant_id       = var.tenant_id
            subscription_id = var.subscription_id
      }
      ```
    - main.tf
      ```hcl
      resource "azurerm_resource_group" "example" {
            name     = "rg-terraform-sp-demo"
            location = "japaneast"
      }
      ```
3. 运行 Terraform 部署
      ```bash
      terraform init
      terraform apply
      ```
---

## 🆔 托管身份（Managed Identity）

| 类型 | 特点 | 用法 |
|------|------|------|
| **System-assigned** | 与资源绑定，资源删除则身份自动销毁 | 适合单服务资源 |
| **User-assigned** | 独立存在，可复用到多个资源 | 适合共享访问权限的服务 |

✅ 用托管身份可避免硬编码密码，推荐在 Azure Functions / VM / Container 中使用。

---

## 🔐 RBAC：基于角色的访问控制

### 核心结构

```text
RBAC = Role + Scope + Assignment
```

| 组成 | 描述 |
|------|------|
| **Role** | 操作权限集合，如 Reader、Contributor、Owner |
| **Scope** | 权限作用范围：订阅、资源组、资源 |
| **Assignee** | 被授权实体：用户、组、托管身份 |

### Scope 层级（权限可继承）

```
Management Group
   └── Subscription
         └── Resource Group
               └── Resource
```

> 权限从上往下继承，推荐精细化控制。

---

## 🛠️ 实操命令示例

```bash
# 将某用户赋予 Reader 权限到资源组
az role assignment create   --assignee user@example.com   --role "Reader"   --scope /subscriptions/<sub-id>/resourceGroups/my-rg

# VM 绑定系统托管身份
az vm identity assign   --name myVM   --resource-group myRG

# 分配托管身份读取 Key Vault 权限
az role assignment create   --assignee <identityClientId>   --role "Key Vault Reader"   --scope /subscriptions/<sub-id>/resourceGroups/my-rg/providers/Microsoft.KeyVault/vaults/mykv
```

---

## 🧪 Day 5 测验回顾（全对 🎉）

| 题号 | 正确答案 | 说明 |
|------|-----------|------|
| 1 | C | 托管身份用于安全替代密码访问 Azure 资源 |
| 2 | C | RBAC Scope 不包括 Tenant 层级 |
| 3 | A, C, D | Role + Scope + Assignee 是三要素 |
| 4 | C | 权限从订阅向资源组、资源继承 |
| 5 | B | 用户分配身份适合多个 Function App 共享身份场景 |

---

## 📌 关键记忆点

- **RBAC 三要素**：角色、作用域、授权对象
- **作用域继承机制**：权限从上向下自动传播
- **托管身份替代密码**：推荐使用 system/user-assigned identity
- **推荐授权粒度**：尽量使用资源组级别授权而非全订阅

---

## 📅 下一步 - Day 6 预告

| 模块 | 内容 |
|------|------|
| 网络安全 | 网络安全组（NSG）、应用安全组（ASG） |
| 防火墙与 Private Endpoint | Azure Firewall、Private Link、Service Endpoint |
| 实操 | 配置出站 NAT、入站限制、防火墙规则 |
| 测验 | NSG vs Firewall、Private Endpoint 用途题 |
