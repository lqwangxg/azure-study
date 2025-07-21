# 📘 AZ-500 Day 1：Azure AD 身份管理与角色权限控制

---

## 📚 今日重点知识点

### 1. Azure 角色与权限模型概览

| 项目 | 描述 |
|------|------|
| Azure 角色（RBAC） | 用于管理 Azure 资源访问权限（如 VM、存储） |
| Azure AD 角色 | 用于管理 Azure Active Directory（如用户、组） |
| 常见角色 | - **Reader**：只读访问<br>- **Contributor**：读写，但不可赋权<br>- **Owner**：完全控制，包括赋权<br>- **User Access Administrator**：仅可分配权限 |

---

### 2. RBAC 权限模型组成

| 组成部分 | 说明 |
|----------|------|
| 安全主体（Principal） | 用户、组、服务主体、托管身份 |
| 角色定义（Role） | 一组权限动作（Actions） |
| 作用域（Scope） | 管理组 → 订阅 → 资源组 → 资源 |
| 角色分配（Assignment） | 将某个角色授予某个主体，作用于特定作用域上 |

---

### 3. 角色分配方式

- **Azure Portal**：资源页点击“访问控制（IAM）”
- **CLI 示例**：
  ```bash
  az role assignment create \
    --assignee user@example.com \
    --role "Reader" \
    --scope "/subscriptions/xxxxx/resourceGroups/demo-rg"
  ```

---

### 4. Azure AD 角色 vs Azure 资源角色

| Azure AD 角色 | Azure RBAC 角色 |
|---------------|-----------------|
| 管理目录（用户/组/策略） | 管理 Azure 资源（VM、存储等） |
| 示例：Global Admin, User Admin | 示例：Owner, Reader, Contributor |

---

### 5. 服务主体 & 托管身份（MSI）

| 类型 | 描述 |
|------|------|
| 服务主体（Service Principal） | 应用注册后自动创建，可用于脚本或自动化访问 Azure 资源 |
| 托管身份（Managed Identity） | Azure 资源自动获得的身份，可安全访问资源（如 Key Vault）无需凭据 |

---

## ❌ 错题集

---

### ❌ 第 1 题

**问题：**  
你希望某用户能够读取 Azure 存储数据内容，但不能删除。应分配以下哪个角色？

**你的答案：** B. Contributor  
**正确答案：** C. Storage Blob Data Reader

**解析：** Contributor 拥有读、写、删除权限，权限过大。Storage Blob Data Reader 是更符合最小权限原则的选择。

---

### ❌ 第 10 题

**问题：**  
服务主体和托管身份的区别是？

**你的答案：** A  
**正确答案：** A、B

**解析：**  
- 服务主体由用户手动注册，需维护密码/证书  
- 托管身份为 Azure 资源自动创建，不需凭据即可安全访问其他资源（如 Key Vault）

---
