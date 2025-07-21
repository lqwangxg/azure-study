# 📝 AZ-104 强化练习错题详解

## ❌ 第 2 题

**题目**：你希望确保某些资源只能在“日本东部”和“日本西部”区域创建，其他区域将被阻止。你应该使用哪种 Azure 服务来实现这一要求？

A. 创建一个 Azure Policy 来限制资源区域  
B. 启用 VM 的备份  
C. 将策略附加到包含订阅的管理组  
D. 自动修复漏洞  

**你的答案**：B  
**正确答案**：A, C  
**解析**：使用 Azure Policy 可以限制资源只能在特定区域创建，并可将该策略附加到管理组来影响多个订阅。启用 VM 备份与区域控制无关，自动修复漏洞是安全防护功能。

---

## ❌ 第 11 题
**题目**： 你有多个资源组，希望跨组集中应用相同的访问控制策略，应使用？
A. Azure Blueprint
B. Management Group
C. Azure Policy
D. Resource Tag
**你的答案**：C  
**正确答案**：B  
**解析**：想跨多个资源组复用访问控制策略，应使用 Management Group。
Azure Policy 适用于策略限制和合规性，不是 RBAC 权限控制复用工具。

## ❌ 第 12 题
**题目**：你有一个 Web 应用部署在 Azure App Service 上，近期收到带宽限制警告。你想要确认当前 App Service Plan 的出站带宽配额（Outbound Bandwidth Quota）。你应该在 Azure Portal 的哪里查找？

A. 应用服务的“监视”页  
B. App Service Plan 的“配额和使用情况”页  
C. 应用服务的“诊断和解决问题”页  
D. 资源组的“用量 + 配额”页  

**你的答案**：C  
**正确答案**：B  
**解析**：出站带宽限制是应用服务计划（App Service Plan）级别的配额，必须在 Plan 的“Quota（配额和使用情况）”页面查看。其他选项不提供此类详细指标。

## ❌ 第 2 题

**题目**：你想要限制用户只能在特定资源组中创建虚拟机。应该使用哪两个 Azure 功能？（多选）

A. 角色访问控制（RBAC）  
B. Azure Policy  
C. Azure Blueprints  
D. Resource Graph

**你的答案**：B  
**正确答案**：A、B  
**解析**：RBAC 用于控制访问范围（如仅限资源组），Azure Policy 用于限制资源类型（如只能创建虚拟机）。两者配合实现控制目标。

---

## ❌ 第 4 题

**题目**：Azure 资源管理器模板（ARM Template）可用于哪些操作？（多选）

A. 执行 Azure CLI 命令  
B. 自动化资源部署  
C. 运行 Bash 脚本  
D. 定义资源之间的依赖关系

**你的答案**：B、C、D  
**正确答案**：B、D  
**解析**：ARM 模板用于声明式部署 Azure 资源，支持自动部署和资源依赖。无法直接执行脚本或 CLI 命令。

---

## ❌ 第 10 题

**题目**：你需要限制某个用户只能重启虚拟机，但不能删除虚拟机。应分配哪种角色？

A. Reader  
B. Virtual Machine User Login  
C. Virtual Machine Contributor  
D. Contributor

**你的答案**：B  
**正确答案**：C  
**解析**：VM Contributor 允许管理（如重启）虚拟机但不包含删除权限；User Login 仅限登录权限，无法执行重启操作。

---

## ❌ 第 15 题

**题目**：你想要将存储账号的日志发送到另一个 Log Analytics 工作区，应使用什么机制？

A. Event Grid  
B. Diagnostic Settings（诊断设置）  
C. Activity Log  
D. Log Search

**你的答案**：A  
**正确答案**：B  
**解析**：Diagnostic Settings 可配置将资源日志发送至 Log Analytics，而 Event Grid 是用于事件驱动的通知服务。

---

## ❌ 第 18 题

**题目**：你在配置 Application Insights 时，默认会自动收集哪些类型的数据？（多选）

A. Request  
B. Exception  
C. Custom Metrics  
D. Dependency

**你的答案**：A、B  
**正确答案**：A、B、D  
**解析**：默认会自动收集请求（Request）、异常（Exception）和依赖项（Dependency）。Custom Metrics 需手动定义。

---

## ❌ 第 20 题

**题目**：你想创建 Azure VM 并将其加入现有虚拟网络的指定子网，应使用哪个参数？

A. --vnet  
B. --subnet  
C. --network-interface  
D. --vnet-subnet-id

**你的答案**：B  
**正确答案**：D  
**解析**：`--vnet-subnet-id` 是用于指定子网的参数，需提供完整的子网资源 ID。选项 B 不存在于命令中。
