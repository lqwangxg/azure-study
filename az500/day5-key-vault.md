# 📚 Day 5 学习要点：Azure Key Vault 安全管理

## 🔐 1. Key Vault 权限模型
- Key Vault 支持两种授权方式：
  - **访问策略（Access Policies）**：传统方式
  - **Azure RBAC**：更现代且可细粒度控制
- 推荐使用 Azure RBAC 管理权限。

## 🌐 2. 网络级访问控制
- 启用 **防火墙**：允许特定 IP 或虚拟网络访问
- **Private Endpoint**：将 Key Vault 限制在专属虚拟网络中，阻止公共访问

## 🔄 3. 软删除 (Soft Delete) 与清除保护 (Purge Protection)
- **软删除**：防止误删，可以在 90 天内恢复资源
- **清除保护**：防止资源被永久删除（需启用软删除）

## 🛡 4. Key Vault 中的对象类型
- **Keys**：加密密钥
- **Secrets**：API 密钥、连接字符串等
- **Certificates**：X.509 证书管理（含密钥与公钥）

## 🧩 5. 与其他服务集成
- 可结合 **托管身份（Managed Identity）** 自动安全访问 Key Vault
- 应用程序不需要在代码中存储密钥，减少泄露风险

## 📊 6. 审计与日志
- 启用诊断日志（Diagnostic Logs）到 Log Analytics
- 审计访问行为，满足合规性需求


## ❌ Day 5 错题整理：Azure Key Vault 安全

### ❌ 第 4 题

**题目：** 以下哪些资源可以通过 Azure Key Vault 安全访问密钥或机密？
**你的答案：** AD  
**正确答案：** **ABD**  

**解析：**  
- ✅ Azure VM 可通过系统分配托管身份访问 Key Vault。  
- ✅ Azure Functions 也支持托管身份访问 Key Vault（你漏选了）。  
- ❌ 外部数据库不能直接访问 Azure Key Vault。  
- ✅ App Service 支持访问（你选对了）。  

**知识点总结：**  
Azure 中以下服务可通过托管身份安全地访问 Key Vault：  
- Azure VM  
- Azure Functions  
- App Service  
- Azure Logic Apps 等  
必须在 Key Vault 中授予这些身份相应权限（使用 Access Policy 或 RBAC）。
