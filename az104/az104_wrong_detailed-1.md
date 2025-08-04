# AZ-104 考前错题回顾清单

## ❗ 权限控制与身份认证

### ❌ Function App 最佳权限控制  
- 错选：A. 生成 SAS 链接  
- 正解：**B. 使用 Managed Identity + 角色分配（Storage Blob Data Reader）**  
- ✅ 原因：最小凭据原则，避免硬编码  

### ❌ Initiative 可用于权限控制？  
- 错选：C. 可控制访问权限  
- 正解：**❌ Initiative 仅用于聚合 Policy，无法控制权限**  

---

## ❗ 存储与加密机制

### ❌ 存储默认加密机制  
- 错选：D. 不加密  
- 正解：**C. Microsoft-managed key（平台托管）**  
- ✅ Azure 所有资源默认加密，用户可改为 CMK  

### ❌ VM 是 PaaS？  
- 错选：将 VM 当作 PaaS  
- 正解：**VM 是 IaaS**，而 App Service / SQL / Logic App 是 PaaS
  - Azure SQL Database：完全托管的关系数据库，属于 PaaS。
  - Azure Functions：事件驱动、无服务器计算，PaaS  
  - Azure App Service：托管 Web 应用平台，PaaS  
  - Azure VM：属于 IaaS，不是 PaaS  

---

## ❗ 网络与访问控制

### ❌ NSG 支持 FQDN？  
- 错选：A. NSG 可配置 FQDN  
- 正解：**❌ NSG 不支持 FQDN，仅 IP/Port 级别**  
- ✅ 若需域名过滤，请使用 Azure Firewall

---

## ❗ 告警与监控

### ❌ Function App 报错触发通知  
- 错选：仅 Metric 告警  
- 正解：**Log 告警 + Action Group 可根据日志中错误内容触发邮件/自动化**
  - Metrics、Alerts、Log Analytics 均可用于监控和自动告警。
  - Activity Log 是事件记录，不用于主动告警。

---

## ❌ 2. 虚拟机操作权限控制
**题目：**  
你希望允许某个用户重启 Azure VM，但不允许其删除 VM。你应分配哪种内建角色？  
**你的答案：** A. Contributor  
**正确答案：** B. Virtual Machine Contributor  
**解析：** Contributor 拥有广泛权限，包括删除。Virtual Machine Contributor 专门用于管理 VM，不含删除权限，更符合最小权限原则。

---

## ❌ 3. 限制存储账户访问来源
**题目1：**  
你希望只允许东日本和西日本访问某个存储账户. 应启用：  
**你的答案：** A. Azure Policy  
**正确答案：** D. Network Rules with IP ranges  
**解析：** 限制存储账户只能被特定区域访问，需要通过网络规则（Network Rules）设置允许的 IP 范围或虚拟网络。  
  - A（Azure Policy）无法精确限制区域访问。
  - B（Service Endpoint）、C（Private Endpoint） 是网络连接方式，不解决区域限制问题。

**题目2：**  
你希望防止任何人从公共网络访问某个存储账户，但允许专用网络（VNet）访问。应启用：  
**你的答案：** C. NSG Inbound Rule  
**正确答案：** B. Private Endpoint  
**解析：** NSG 无法限制存储服务的公私网络访问。正确做法是启用 Private Endpoint，从而实现完全关闭公网访问，仅限专用网络访问。

---

## ❌ 4. 托管身份支持的服务
**题目：**  
以下哪些服务支持使用托管身份（Managed Identity）进行身份验证？（多选）  
**你的答案：** BCD  
**正确答案：** ABCD  
**解析：** Azure VM、Logic App、SQL Database、AKS 都支持托管身份。选项 A（VM）漏选。

---

## ❌ 7. Azure Backup 恢复失败的排查
**题目：**  
你在使用 Azure Backup 保护一台 Windows VM，但恢复失败。你最可能要检查的内容是？  
**你的答案：** A. VM 是否在运行  
**正确答案：** C. Azure Backup Agent 是否安装  
**解析：** 使用 Azure Backup 恢复时需依赖代理。未安装或配置异常将导致恢复失败。

---

## ❌ 11. 资源运行状况分析服务
**题目：**  
你希望分析 Azure 上所有资源的运行状况，应使用哪项服务？（多选）  
**你的答案：** A  
**正确答案：** A, B, C  
**解析：** Azure Monitor（A）+ Log Analytics（B）收集与分析日志数据，Service Health（C）用于识别服务层面问题。

---

## ❌ 12. 限制 SSH 登录来源
**题目：**  
如何限制 VM 只能从特定 IP 段登录 SSH？  
**你的答案：** B. 使用 Azure Firewall:x:    
**正确答案：** A. 配置 NSG 入站规则:o:    
**解析：** 限定访问来源首选 NSG，Firewall 适用于更复杂的流量控制。

---

## ❌ 15. 日志读取权限控制
**题目：**  
你想限制某用户只能读取诊断日志，不允许做其他操作，应该分配的角色是？  
**你的答案：** D. Storage Blob Data Reader :x:   
**正确答案：** B. Monitoring Reader:o:    
**解析：** Monitoring Reader 可读取监控日志，Storage Blob Data Reader 用于读取 Blob 内容，不适合诊断日志访问控制。

---

## ❌ 17. Function App 内网访问控制
**题目：**  
你希望让某个 Function App 仅由 VNet 中的资源访问，应使用：  
C. Private Endpoint:o:    
**解析：** IP 限制不够安全。Private Endpoint 可将 Function App 完全绑定至 VNet，禁止公网访问。

---

## ❌ 1.（第2组第1题）限制用户仅重置 VM 密码

**题目：**  
你希望某个用户只能为虚拟机重置密码，但不能更改其他配置，应使用哪个角色？
 
A. Helpdesk Administrator :o:  

**解析：**  
Helpdesk Administrator 是最小权限模型下专门用于重置用户凭据的角色。  
Virtual Machine Contributor 权限范围更大，可能允许修改配置，不符合要求。

---

## ❌ 2.（第1组第4题）Azure VM 是否需要安装 Backup Agent

**题目：**  
启用 Azure Backup 保护 VM 时，下列哪项不需要你手动配置？

B. Azure Backup Agent:o:  

**解析：**  
Azure VM 的备份通过平台集成，不需要用户手动安装 Azure Backup Agent。该代理只用于本地服务器的备份。

---

## ❌ 3.（第1组第11题）Private Endpoint 的正确流量监控方式

**题目：**  
若你希望监控某 VNet 的出入流量，推荐使用的服务是？（多选）
A NSG Flow Logs :o:  
B Azure Monitor Metrics :o:  
C Network Watcher :o:   
D Traffic Manager :x:

**解析：**  
Traffic Manager 是 DNS 层负载均衡工具，不监控流量。  
推荐使用
- NSG Flow Logs
- Azure Monitor Metrics
- Network Watcher

---

## ❌ 4.（第1组第3题）Azure Backup 的恢复失败原因排查

**题目：**  
你在使用 Azure Backup 保护一台 Windows VM，但恢复失败。你最可能要检查的内容是？

**你的答案：** A. VM 是否在运行  
**正确答案：** C. Azure Backup Agent 是否安装  

**解析：**  
如果是本地物理或异地 VM，需要安装 Azure Backup Agent。对于 Azure 内部 VM 则不会是运行状态问题，而是依赖备份代理（或缺失诊断配置）导致恢复失败。

---
## ❌ 虚拟网络子网限定被指定的虚拟机访问

**题目：**  
你在 Azure 中创建了一个新的虚拟网络子网，并希望它只能被指定的虚拟机访问。你应该配置：  
A. User Defined Route  
B. Application Security Group  
C. Network Security Group:o:  
D. Route Table

**解析：**  
- 要控制子网访问，必须使用 **Network Security Group (NSG)** 来定义入站和出站规则。  
- ASG（Application Security Group）可以逻辑地组织 VM，但**本身不提供控制能力，必须与 NSG 配合使用**。  
- 你只选了 ASG，**缺少核心的 NSG 控制逻辑**。

## ❌ Azure SQL Server 限制 IP 访问
**题目：**  
你希望为某个 Azure SQL Server 设置 IP 限制，只允许公司办公楼 IP 访问。你应在哪设置？  
A. SQL Server 的防火墙规则:o:  
B. SQL 数据库级别设置  
C. Azure Policy  
D. NSG 规则

**解析：**  
- SQL Server 属于 **PaaS 服务**，必须通过“SQL Server 防火墙规则”设置允许的客户端 IP。  
- **NSG 控制的是 VM 网络流量，不能作用于 Azure PaaS 服务**。

## ❌  VM 镜像和容器镜像的保存方式
**题目：**  
你需要将一批 VM 镜像保存并在多个区域部署，应使用以下哪项？  
A. Azure Container Registry  
B. Azure Image Gallery (SIG) :o:  （新名字：Azure Compute Gallery）  
C. Azure DevTest Labs  
D. Shared Access Signature (SAS)

**解析：**  
- **Azure Compute Gallery（SIG）** 允许创建、管理并跨区域共享自定义 VM 镜像。  
- Azure Container Registry 是存放容器镜像（如 Docker 镜像），**不适合 VM 映像**。

## ❌  长期保留日志，超过 90 天
**题目：**  
你要让 Azure Monitor 中的日志可以保留超过 90 天，应进行以下操作？  
A. 修改诊断设置的保留期  
B. 将日志导出至 Log Analytics Workspace  
C. 将日志保存到 Storage Account :o:  
D. 在 Azure Monitor 中启用“长期保留”选项
**解析：**  
- Log Analytics Workspace 默认的日志保留期是 30~730 天（最多 2 年），并非“长期”备份。  
- 如果你希望满足合规性需求（例如保存 5～10 年），应通过诊断设置将日志导出到 Azure Storage Account。  
- A 选项只适用于简单诊断数据；
- B （将日志导出至 Log Analytics Workspace ）是用于分析，而不是备份；
- D 是不存在的选项。

## ❌  Azure CLI 创建 VM 时，指定使用托管身份的参数
**题目：**  
你在使用 Azure CLI 创建 VM 时，希望指定使用托管身份以便访问 Key Vault。你应该使用以下哪个参数？  
A. `--assign-identity`:o:  
B. `--enable-managed-identity`  
C. `--identity`  
D. `--aad-role`

**解析：**  
- Azure CLI 中，`az vm create` 启用托管身份（MSI）使用的参数是 `--assign-identity`。  
- 该参数可用于启用 system-assigned 或 user-assigned 身份。  
- `--enable-managed-identity` 不存在，并非有效参数，会导致 CLI 报错。  
- `--identity` 用于定义 user-assigned identity 的 ID，但需先 `--assign-identity`。

## ❌   Azure 存储帐户, 限制虚拟网络子网访问
**题目：**  
你创建了一个新的 Azure 存储帐户，并希望限制某个虚拟网络子网才能访问它。你应该采取以下哪些步骤？  
A. 启用服务终结点并配置防火墙规则:o:   
B. 在子网中配置网络安全组允许存储服务出站    
C. 启用存储帐户的防火墙和虚拟网络访问控制 :o:   
D. 将存储帐户设置为私有端点专用访问:o:   

### 解析：
- 要限制某子网访问存储账户，需：
  - 在目标子网启用 **服务终结点（Service Endpoint）**
  - 在存储账户启用 **“选定虚拟网络访问”**
  - 允许特定子网通过虚拟网络规则访问。
- **D (私有端点)** 也是可选方案，但会创建新的 NIC，适用于完全隔离的访问方式，不是唯一方案。

## ❌  跨区域 VM 间低延迟通信首选 VNet Peering
**题目：**  多地部署虚拟机，确保它们之间低延迟、专线连接  
A. VNet Peering :o:   
B. Application Gateway  
C. VPN Gateway    
D. ExpressRoute  

### 解析：
- 跨区域 VM 间低延迟通信首选 **VNet Peering**。
- **D (ExpressRoute)** 提供与本地网络的专线连接，**不是 Azure 区域间通信的解决方案**。

### 🌐 VNet Peering（对等互连）

- **用途**：将两个 VNet 虚拟网络互连
- **类型**：
  - 区域内 Peering：同一区域内，零延迟
  - 跨区域 Peering：不同区域，需额外带宽费用
- **特点**：
  - 无需公网 IP
  - 默认自动互通（不需要路由表）
  - 可设置流量转发（Allow Forwarded Traffic）

## ❌  资源部署 强制使用特定的标签

**题目：** 公司希望所有资源部署都强制使用特定的标签键值组合，如何实现？
A. 使用 Azure Policy 限制和自动添加标签:o:  
B. 使用锁资源组策略防止更改
C. 使用 ARM 模板嵌套标签定义
D. 配置资源诊断设置自动补齐标签


### 解析：
- **只有 A 是正确**：Azure Policy 可以强制标签存在并设置默认值。
- **C、D 错误**：ARM 模板不能全局强制标签，诊断设置不处理标签。