# 📝 AZ-104 强化练习错题详解（共 3 题）

---

## ❌ 第 1 题

**题目：**  
你希望某用户仅能注册 Azure AD 应用，而不能管理其他用户，应分配哪种角色？

- A. Application Developer  
- B. Global Administrator  
- C. User Administrator  
- D. Application Administrator  

**你的答案：** `D`  
**正确答案：** `A`  
**解析：**  
应选择 A：`Application Developer`。它允许用户注册应用而不会授予不必要的权限。  
而 D（Application Administrator）权限过大，会允许用户管理所有企业应用，超出题意。

---

## ❌ 第 5 题

**题目：**  
以下关于 Diagnostic Settings 的说法，哪项是正确的？（多选）

- A. 它只能发送日志到 Log Analytics  
- B. 它可将日志发送到 Event Hub、Storage、Log Analytics  
- C. VM 必须启用 Diagnostic Extension 才能启用 Diagnostic Settings  
- D. 可设置多个目标同时接收诊断日志  

**你的答案：** `BCD`  
**正确答案：** `BD`  
**解析：**  
B 和 D 正确。C 错：虽然某些 VM 性能指标确实依赖扩展，但 Diagnostic Settings 本身适用于多种资源，并不依赖 VM Diagnostic Extension。  
A 错：除了 Log Analytics，还可以发到 Storage、Event Hub。

---

## ❌ 第 6 题

**题目：**  
你希望让开发者上传 blob，但不允许其删除或列出 blob，应如何操作？

- A. 使用 SAS token 限制权限  
- B. 分配 Storage Blob Data Contributor  
- C. 创建自定义角色仅包含上传权限  
- D. 使用 Storage Queue 控制权限  

**你的答案：** `A`  
**正确答案：** `AC`  
**解析：**  
A 是临时授权的一种方式，但最合理做法是结合 C，使用**自定义角色**明确授予上传权限（例如 Put Blob），并排除删除、列出权限。  
B 的权限过宽，包含删除；D 无关。

# 📝 AZ-104 强化练习错题详解（第 2 轮，共 4 题）

---

## ❌ 第 5 题

**题目：**  
以下哪些 Azure Policy 定义可用于资源一致性检查？（多选）

- A. 限制 VM SKU  
- B. 强制标签存在  
- C. 控制 NSG 规则  
- D. 限制资源创建时间窗口  

**你的答案：** `ABD`  
**正确答案：** `AB`  
**解析：**  
A、B 正确：可用于策略定义限制资源类型与标签要求。  
C 不常见，但可以通过策略自定义。  
**D 错误**：Azure Policy 不能限制“时间窗口”，那属于计划任务类需求，应通过部署流程控制。

---

## ❌ 第 6 题

**题目：**  
你希望开发者仅能上传 blob 文件且不可读取或删除，应使用：

- A. 存储 SAS token  
- B. 存储访问策略  
- C. 自定义角色  
- D. Storage Blob Data Contributor  

**你的答案：** `A`  
**正确答案：** `C`  
**解析：**  
虽然 A 可临时授权上传权限，但长期安全方案是使用 **自定义角色** 明确授予 Put Blob 权限，移除 List/Delete。  
B 是绑定 SAS 的基础，不是控制策略。D 权限过大。

---

## ❌ 第 8 题

**题目：**  
哪项功能允许你限制资源组中 VM 只能部署到某个子网？

- A. Azure Blueprints  
- B. NSG  
- C. Azure Policy  
- D. Role Assignment  

**你的答案：** `A`  
**正确答案：** `C`  
**解析：**  
应使用 Azure Policy 创建规则，如限制 VM 创建时使用的子网 ID。  
Blueprint 是组合部署模板的框架，但不能单独限制子网部署行为。

---

## ❌ 第 10 题

**题目：**  
你希望记录 Key Vault 中的每次读取 secret 操作，应设置：

- A. Activity Log  
- B. Diagnostic Settings  
- C. NSG Flow Logs  
- D. Application Insights  

**你的答案：** `C`  
**正确答案：** `B`  
**解析：**  
只有通过启用 **Diagnostic Settings**，才能记录对 Key Vault 中 secret 的读取行为。  
Activity Log 不含数据面操作，NSG Flow Logs 是网络级别，Insights 不适用于 KV。

# ✅ AZ-104 错题回顾题集（含题干）

---

## ❌ 第 3 题

**题目**：哪两项可以在不影响运行中的虚拟机的情况下扩展其性能？（选择两个）

A. 更换 OS 磁盘  
B. 更改虚拟机大小  
C. 添加数据磁盘  
D. 更换 VNet  

**你的答案**：C,D  
**正确答案**：B,C  
**解析**：  
- ✅ **更改虚拟机大小**：Azure 支持动态调整计算资源（有时需重启但不会重建 VM）。  
- ✅ **添加数据磁盘**：可以挂载额外磁盘来增加存储性能，不会影响当前运行。  
- ❌ 更换 VNet 会造成网络连接断开，属于重构性操作。  

---

## ❌ 第 6 题

**题目**：以下哪一项可以实现对 Azure 上__服务__运行状况的集中查看？

A. Azure Monitor  
B. Service Health  
C. Azure Metrics  
D. Network Watcher  

**你的答案**：A  
**正确答案**：B  
**解析**：  
- ✅ **Service Health** 提供 Azure 区域内服务状态、故障和维护通知，是服务级运行状态的集中入口。  
- ❌ Azure Monitor 主要用于自定义监控和日志。  

---

## ❌ 第 7 题

**题目**：你有一个运行在 Windows Server 的 Azure VM，你希望在不登录的情况下重置其管理员密码。应该使用哪项？

A. Azure Automation  
B. VM Run Command  
C. Azure CLI  
D. Azure Policy  

**你的答案**：C  
**正确答案**：B  
**解析**：  
- ✅ **VM Run Command** 可远程执行 PowerShell 或 shell 脚本，包括密码重置等任务。  
- ❌ Azure CLI 通常是调用 Run Command 的工具，但不是直接手段。  

---

## ❌ 第 9 题

**题目**：哪两项操作可以触发 Azure Monitor 中的警报？（选择两个）

A. Log Analytics 查询  
B. Application Insights 警告  
C. 网络带宽计费  
D. 活动日志  

**你的答案**：B,D  
**正确答案**：A,D  
**解析**：  
- ✅ **Log Analytics 查询**：可以通过查询结果设定警报条件。  
- ✅ **活动日志 (Activity Log)**：系统事件可作为警报触发器。  
- ❌ Application Insights 是应用层监控，通常通过日志集成，而非直接触发 Azure Monitor 警报。  

---

## ❌ 第 11 题

**题目**：你想知道某台 VM 的过去 30 天的 CPU 使用率，应查看哪一项？

A. Azure Advisor  
B. 活动日志  
C. Azure Monitor  
D. Azure Resource Graph  

**你的答案**：D  
**正确答案**：C  
**解析**：  
- ✅ **Azure Monitor** 提供性能指标查询，支持历史 CPU、内存、磁盘 IO。  
- ❌ Resource Graph 用于查询资源结构、状态，不含时间序列数据。  

---

## ❌ 第 13 题

**题目**：你在多个订阅中部署资源，想集中管理策略并继承层级，应使用：

A. 资源组  
B. 管理组  
C. Azure Policy  
D. Azure Advisor  

**你的答案**：C  
**正确答案**：B  
**解析**：  
- ✅ **管理组 (Management Group)** 可跨订阅设定权限、策略、标签等统一管理。  
- ❌ Azure Policy 用于定义规则，但需要绑定在订阅或管理组等范围上。

---

## ❌ 第 16 题

**题目**：你希望将应用部署到受控环境中，具有自动扩展、负载均衡和零停机部署能力，应选择哪项服务？

A. Azure App Service  
B. Azure VM Scale Sets  
C. Azure Functions  
D. Azure Container Instances  

**你的答案**：C  
**正确答案**：A  
**解析**：  
- ✅ **App Service** 提供完整的 Web 应用托管平台，支持部署槽位、自动扩展、负载均衡和零停机发布。  
- ❌ Azure Functions 适合事件驱动，无部署槽位等完整托管特性。

# AZ-104 第二轮错题回顾集

---

## ❌ 第 1 题

**题目**：你要在 VM 崩溃时自动恢复它。应使用哪项功能？

A. 可用性集（Availability Set）  
B. 自动修复（Automatic Repair）  
C. 应用程序网关  
D. Azure Site Recovery  

**你的答案**：A  
**正确答案**：B  
**解析**：自动修复是 Platform-initiated 的自愈功能，适用于 VM 崩溃或挂起情况；Availability Set 是为了防止单点硬件故障，不包括故障恢复逻辑。

---

## ❌ 第 8 题

**题目**：你希望 VM 在某区域不可用时自动转移到另一区域，应该使用什么？

A. 可用性区域  
B. 虚拟机规模集  
C. Azure Site Recovery  
D. 高可用性负载均衡器  

**你的答案**：D  
**正确答案**：C  
**解析**：只有 Azure Site Recovery 支持将 VM 从一个区域故障时切换到另一个区域，实现灾难恢复。

---

## ❌ 第 11 题

**题目**：你想为某个 Function App 设置每日最大出站流量，应如何实现？

A. 配置配额限制  
B. 设定 Azure Policy  
C. 设置应用服务计划限制  
D. 使用服务终结点  

**你的答案**：A  
**正确答案**：C  
**解析**：Function App 使用的 App Service Plan 可设置出站带宽配额，Function 本身没有单独的流量限制配置项。

---

## ❌ 第 15 题

**题目**：你希望将 Azure VM 连接到本地 Active Directory，首先需要做什么？

A. 配置 ExpressRoute  
B. 设置本地 DNS 转发  
C. 安装 Azure AD Connect  
D. 在 VM 上加入域  

**你的答案**：A  
**正确答案**：B  
**解析**：Azure VM 若要加入本地域，必须能解析域名，DNS 是前提。ExpressRoute/VPN 是网络路径，但不是首要配置。

---

## ❌ 第 16 题

**题目**：Azure 资源的默认限制（例如 VM 数量）如何提升？

A. 提交配额请求  
B. 更改订阅类型  
C. 修改 Azure Policy  
D. 升级为 Dev/Test 订阅  

**你的答案**：B  
**正确答案**：A  
**解析**：配额是通过 Azure Portal/CLI 提交 "配额增加请求" 来提升的，与订阅类型无关。

---

## ❌ 第 18 题

**题目**：哪项服务可以用于集中查看 Azure、AWS 和 GCP 的安全建议？

A. Azure AD Identity Protection  
B. Azure Policy  
C. Microsoft Defender for Cloud  
D. Azure Lighthouse  

**你的答案**：D  
**正确答案**：C  
**解析**：Defender for Cloud 跨云支持 AWS/GCP/Azure 的安全 posture 管理和建议；Lighthouse 是用于 MSP 的资源访问代理方案。
