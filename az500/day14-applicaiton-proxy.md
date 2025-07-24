# AZ-500 Day 14 学习总结与题库

---

## 📘 今日知识点总结：应用程序安全（Application Security）

### 1. Azure AD Application Proxy  
- 允许安全发布本地应用，无需暴露公网端口。  
- 用户通过 Azure AD 认证后访问本地资源。  
- 需安装本地连接器，并可支持自定义域名。

### 2. Azure App Service 身份验证/授权（Authentication/Authorization）  
- 可在无需代码更改的情况下启用用户身份验证。  
- 支持多种身份提供商（Azure AD、Facebook、Google 等）。  
- 身份验证功能可以灵活开启或关闭。

### 3. 服务主体（Service Principal）  
- 代表非交互式的应用或服务身份，用于自动化和应用认证。  
- 用于分配最小权限原则，控制访问 Azure 资源。

### 4. 托管标识（Managed Identity）  
- 提供 Azure 资源对其他 Azure 服务的安全访问。  
- 分为系统分配和用户分配两种类型。  
- 系统分配与资源生命周期绑定，用户分配可复用。  
- 支持多种资源类型，不限于虚拟机。

### 5. Azure Key Vault 权限控制  
- 支持访问策略和基于角色的访问控制（RBAC）。  
- 可以与托管标识结合，自动安全地访问密钥和机密。  
- 不默认开放给所有订阅用户。

### 6. Azure Defender for App Service  
- 监控异常行为，如上传未知模块、异常脚本等。  
- 帮助识别潜在安全威胁，提高应用安全性。

### 7. 安全存储敏感信息的最佳实践  
- 使用 Azure Key Vault 存储数据库连接字符串、API 密钥等机密。  
- 应用通过托管标识安全读取密钥，避免明文配置。

### 8. DevOps 环境中的安全认证  
- 使用服务主体进行自动化服务身份认证。  
- 避免使用用户密码或个人访问令牌（PAT）。

---

## 📝 Day 14 练习题库

1. 【单选】  
你希望通过 Azure 发布一个只能企业员工访问的本地 Web 应用，而无需将其完全暴露在互联网上。你应该使用哪种服务？  
A. Azure VPN  
B. Azure Bastion  
C. Azure AD Application Proxy  ✅  
D. Azure ExpressRoute  

2. 【多选，选 2 项】  
关于 Azure App Service 的身份验证/授权（Authentication/Authorization）功能，说法正确的是哪些？  
A. 它允许你启用无需代码更改的用户身份验证  ✅  
B. 它可以直接使用 Azure Key Vault 管理用户身份  
C. 可以与 Facebook、Google、Azure AD 等身份提供商集成  ✅  
D. 启用后无法禁用  

3. 【单选】  
你创建了一个 Azure Function，并希望它安全地访问存储账号，而不使用明文凭据。你应该采用哪种身份方式？  
A. 服务主体（Service Principal）  
B. 托管标识（Managed Identity）  ✅  
C. 用户身份验证  
D. SAS Token  

4. 【多选】  
关于 Azure Key Vault 的访问权限控制，说法正确的是哪些？  
A. Key Vault 支持基于角色的访问控制（RBAC）  ✅  
B. Key Vault 可以使用访问策略控制细粒度权限   ✅  
C. 所有订阅用户默认拥有 Key Vault 管理权限  
D. 可以将 Key Vault 与托管标识结合使用实现自动认证  ✅  

5. 【单选】  
在 Azure 中，服务主体的主要用途是？  
A. 管理用户登录会话  
B. 用于非交互式应用或服务进行身份验证  ✅  
C. 加密 Key Vault 中的机密  
D. 向 Azure Portal 提供多因素认证  

6. 【多选】  
关于托管标识（Managed Identity），下列哪些说法是正确的？  
A. 分为系统分配和用户分配两种  ✅  
B. 系统分配托管标识随资源生命周期创建和销毁  ✅  
C. 用户分配托管标识可用于多个资源  ✅  
D. 托管标识仅支持 Azure Virtual Machines  

7. 【单选】  
Azure Defender for App Service 检测到的哪种行为会被标记为高风险？  
A. 应用服务被频繁重启  
B. 尝试上传未知扩展模块  ✅  
C. 日志写入失败  
D. 运行时间超过默认阈值  

8. 【多选】  
你希望确保 Web 应用中的敏感信息（如数据库连接字符串）不以明文形式存在，以下做法合理的是哪些？  
A. 使用 Azure App Configuration 明文存储连接字符串  
B. 将敏感数据加密后硬编码进应用  
C. 将敏感信息保存在 Azure Key Vault 中  ✅  
D. 应用使用托管标识从 Key Vault 中读取机密 ✅    

9. 【单选】  
你创建了一个服务连接用于 DevOps 访问 Azure，推荐的认证机制是？  
A. 使用 Azure AD 用户名和密码  
B. 使用服务主体并配置权限  ✅  
C. 使用个人访问令牌（PAT）  
D. 使用 MFA 登录并缓存 Cookie  

10. 【多选】  
你在使用 Azure AD Application Proxy 时，必须确保下列哪些前提条件？  
A. 已为 Azure AD 配置自定义域名  ✅  
B. 安装并配置本地的 Application Proxy 连接器  ✅  
C. 本地应用必须使用 HTTPS  ✅  
D. 用户必须在 Azure AD 中进行身份验证  ✅  
