
# AZ-104 学习笔记 - Day 1

## 🎯 学习目标
- 理解 Azure 的资源管理架构（ARM）
- 掌握 Azure 的基于角色的访问控制（RBAC）
- 完成 Portal 实操演练
- 完成随堂测验并分析结果

---

## 🧱 Azure Resource Manager 架构

Azure 的资源管理结构由多个层级构成：

```
Tenant (Azure AD)
└── Management Group（可选）
    └── Subscription（订阅）
        └── Resource Group（资源组）
            └── Resources（具体服务，如 VM、Storage）
```

- **Tenant**：组织的根目录（一个 Azure AD 实例）
- **Management Group**：管理多个订阅的逻辑结构（适用于企业）
- **Subscription**：资源和账单单位
- **Resource Group**：资源逻辑分组，可以跨 Region
- **Resource**：实际的云服务资源（VM、Blob、Function 等）

---

## 🔐 Azure RBAC（Role-Based Access Control）

RBAC 允许你基于“谁、对什么资源、拥有什么权限”来控制访问。

### 核心概念

| 概念       | 说明 |
|------------|------|
| **Principal** | 用户、组、服务主体、托管身份 |
| **Role**      | 权限角色（如 Reader、Contributor、Owner） |
| **Scope**     | 权限作用域（订阅、资源组、资源） |
| **Role Assignment** | 将角色分配给用户或身份对象 |

### 常见角色

| 角色        | 权限说明 |
|-------------|----------|
| Owner       | 所有权限，包括角色分配 |
| Contributor | 创建和管理资源，不能分配权限 |
| Reader      | 只能查看 |
| VM Operator | 可查看并启动/重启/停止虚拟机 |

---

## 🛠️ 实操任务

在 Azure Portal 中：

1. 找到已有 Function App → 访问控制 (IAM)
2. 添加角色指派 → 选择“Reader”
3. 指派给某个用户或服务主体
4. 确认权限生效

---

## 🧪 随堂测验总结

**得分：5 / 5 ✅ 全部正确**

| 题号 | 知识点 | 正确答案 | 解析 |
|------|--------|----------|------|
| 1 | RBAC 权限 | B (Contributor) | 可修改资源，不能改权限 |
| 2 | Scope 限制 | C (Region) | Region 不是作用域 |
| 3 | 最小权限 | C (Reader) | 只能查看，不能重启 |
| 4 | 资源组特性 | D | 可包含不同区域资源 |
| 5 | 权限叠加 | C | 在资源组内是 Contributor，其他为 Reader |

---

## ✅ 结论

- 完成 AZ-900 核心内容巩固，基础扎实
- 完全可以跳过 AZ-900，直接进入 AZ-104 学习
- RBAC 与资源管理概念掌握准确，实操能力强

---

## 🌐 Azure、AWS、GCP 资源管理架构与 RBAC 支持对比

| 维度             | Azure                                   | AWS                                         | GCP                                         |
|------------------|-----------------------------------------|---------------------------------------------|---------------------------------------------|
| 顶层组织         | Tenant（Azure AD）                      | Organization（可选，基于 AWS Organizations）| Organization（GCP Organization）            |
| 管理分组         | Management Group                        | Organizational Unit (OU)                    | Folder                                      |
| 订阅/账户        | Subscription                            | Account                                     | Project                                     |
| 资源分组         | Resource Group                          | 无原生分组，常用标签或 CloudFormation Stack | 无原生分组，Project 内可用标签              |
| 资源             | Resource（VM、Storage、Function 等）     | Resource（EC2、S3、Lambda 等）              | Resource（Compute Engine、Cloud Storage 等） |
| 访问控制模型     | RBAC（Role-Based Access Control）        | IAM Policy（基于策略的访问控制）            | IAM Policy（基于角色的访问控制）             |
| 作用域           | Tenant / Management Group / Subscription / Resource Group / Resource | Organization / OU / Account / Resource      | Organization / Folder / Project / Resource  |
| 角色/权限        | 内置/自定义角色，细粒度权限              | 内置/自定义策略，细粒度权限                 | 内置/自定义角色，细粒度权限                  |
| 最小权限原则     | 支持                                    | 支持                                        | 支持                                        |
| 条件访问         | 支持（条件访问策略、PIM）                | 支持（条件策略、MFA、SCP）                  | 支持（条件 IAM、组织策略）                   |

**说明：**
- 三大云厂商均采用分层资源管理架构，便于大规模组织和权限治理。
- Azure 以 Resource Group 为核心分组，AWS/GCP 以 Project/Account 为资源隔离单元。
- RBAC（角色/策略）在三大平台均有实现，支持最小权限、细粒度授权和条件访问。
- Azure RBAC 以角色为主，AWS/GCP 以策略/角色为主，均可自定义扩展。
---

## 📅 下一步预告 - Day 2

- 学习 VNet / Subnet / NSG 网络结构
- 掌握 Service Endpoint / Private Endpoint 区别
- Portal 实操 + 小测练习
