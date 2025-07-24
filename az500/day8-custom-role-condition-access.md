# 🛡️ AZ-500 学习笔记：Day 8 - Azure 角色访问控制与 PIM（进阶）

## ✅ 今日知识点总结

### 1. 自定义角色（Custom Role）
- 自定义角色允许精细化控制 Azure 资源的访问权限。
- 可设置字段：
  - `Actions`：允许的操作（如启动 VM、读取资源信息）
  - `NotActions`：不允许的操作（仅控制平面）
  - `DataActions`：允许的数据层操作（如读取 blob、访问 Key Vault 密钥）
  - `NotDataActions`：排除的数据层操作
  - `AssignableScopes`：该角色在哪些作用域（订阅/资源组）内可用

### 2. PIM（Azure Privileged Identity Management）
- 控制对 **Azure AD 角色** 和 **Azure Resource 角色** 的临时访问权限。
- 支持配置：
  - 激活审批流程（如需管理员审批）
  - 多重身份验证（MFA）
  - 限时访问
  - 自动通知与审核日志

### 3. 最小权限原则（Least Privilege）
- 推荐使用：
  - 自定义角色，剥离不必要权限
  - `NotActions` 和 `NotDataActions` 限制敏感操作
- 示例：
  - 运维人员只读日志，不可删除资源
  - 使用 PIM 配置为“按需访问”，而非长期授权

---

## ❌ 错题集（Day 8）

### ❌ 第 3 题

**题目：**  
哪种类型的权限可控制对 Key Vault 中机密数据的访问？

**你的答案：** A  
**正确答案：** C  

**解析：**  
控制 Key Vault、Storage blob 等数据层的访问权限，必须使用 `DataActions` 字段。例如，读取 Key Vault 的密钥、读取 blob 数据等都属于数据操作。`Actions` 控制的是“管理操作”（如创建资源、修改配置），无法控制数据读取/写入。

---

### ❌ 第 9 题

**题目：**  
如果你想阻止某人访问某个资源的数据内容（如读取 blob 数据），应在哪定义中排除权限？

**你的答案：** B  
**正确答案：** D  

**解析：**  
数据访问层权限需通过 `NotDataActions` 排除，`NotActions` 仅适用于控制面权限（如启动 VM、创建资源等），不能控制存储、Key Vault 的数据读取行为。使用错误字段会导致数据访问权限未被正确限制。

---

## 🔚 小结

- 本日答题正确率：**8/10（80%）**
- 建议复习点：
  - 自定义角色的四个关键字段差异（Actions vs DataActions）
  - PIM 的作用对象（Azure AD 角色 & Resource 角色）
  - 数据访问控制需通过 DataActions/NotDataActions 实现

