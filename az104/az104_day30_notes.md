
# AZ-104 学习笔记 - Day 30

## 🌟 Azure App Service 与 DevOps 部署实务

---

## 🏗️ Azure App Service 平台特性

### 🔹 路径映射 (Path-based Routing)
- 支持将不同 URL 路径映射到不同后端
- 示例：
  - `contoso.com/api/*` → API 后端
  - `contoso.com/web/*` → Web 前端

### 🔹 应用配置 (App Settings & Connection Strings)
- 配置环境变量、数据库连接字符串等
- 支持不同部署槽 (Slot) 拥有不同配置

### 🔹 运行时隔离 (Runtime Isolation)
- 基于 App Service Plan 提供资源隔离
- 支持多平台（Windows / Linux）与多语言运行时

---

## 🚀 部署机制与 DevOps 集成

### 🔸 Deployment Slots
- 实现蓝绿部署，支持零中断发布
- 示例：Production / Staging / QA 槽
- 可随时交换槽，实现快速回滚

### 🔸 CI/CD 集成
- 支持 Azure DevOps、GitHub Actions 等
- 实现自动构建、测试、部署流程

### 🔸 支持的部署方式
- ✅ Git Push
- ✅ FTP / SFTP
- ✅ ZIP Deploy
- ✅ 容器镜像 (Docker / ACR)

---

## 🔐 身份验证与安全连接

### 🔸 Private Endpoint
- 将 App Service 接入 VNet 内
- 仅允许 VNet 内部访问，避免公网暴露

### 🔸 Managed Identity
- 为 App Service 分配托管身份
- 可安全访问 Key Vault、Storage、SQL 等 Azure 资源
- 避免硬编码密码

### 🔸 身份验证集成
- 支持 Azure AD、Google、Facebook 等主流身份平台
- 可直接启用用户登录功能
---

## 🌐 Azure、AWS、GCP 关于 App Service 与 DevOps 的对比

| 维度               | Azure App Service                              | AWS Elastic Beanstalk / App Runner / Lambda         | GCP App Engine / Cloud Run / Cloud Functions        |
|--------------------|------------------------------------------------|-----------------------------------------------------|-----------------------------------------------------|
| 托管应用平台       | App Service（Web/API/容器，PaaS）              | Elastic Beanstalk（PaaS）、App Runner、Lambda       | App Engine（PaaS）、Cloud Run、Cloud Functions      |
| 支持语言           | .NET、Java、Node.js、Python、PHP、Go、容器等    | Java、.NET、Node.js、Python、Go、Ruby、容器等        | Java、Node.js、Python、Go、PHP、.NET、容器等         |
| 部署槽/蓝绿发布    | 支持 Deployment Slots（多槽、热切换）           | 支持环境版本切换（Beanstalk）、App Runner 支持蓝绿   | App Engine 支持流量拆分，Cloud Run 支持修订流量分配  |
| CI/CD 集成         | Azure DevOps、GitHub Actions、FTP、ZIP、容器    | CodePipeline、CodeBuild、GitHub Actions、CLI、容器   | Cloud Build、Cloud Source Repos、GitHub Actions、容器|
| 身份与安全         | Managed Identity、Private Endpoint、AD 集成     | IAM 角色、VPC Endpoint、Secrets Manager、Cognito    | IAM、VPC Connector、Secret Manager、Identity Platform|
| 网络隔离           | 支持 Private Endpoint 接入 VNet                | 支持 VPC Endpoint、私有子网部署                     | 支持 VPC Connector、私有服务访问                    |
| 自动扩缩容         | 支持基于规则的自动扩缩容（App Service Plan）    | 支持自动扩缩容（Beanstalk、App Runner、Lambda）     | 支持自动扩缩容（App Engine、Cloud Run、Functions）  |
| 日志与监控         | App Insights、Log Analytics、Kudu               | CloudWatch、X-Ray、Elastic Beanstalk 日志            | Stackdriver（Cloud Logging/Monitoring）、Error Reporting|
| DevOps 工具链      | Azure DevOps、GitHub Actions、CLI、ARM/Bicep    | CodePipeline、CodeDeploy、CloudFormation、SAM        | Cloud Build、Cloud Deploy、Deployment Manager        |

**总结：**
- 三大云厂商均提供托管应用平台，支持多语言、自动扩缩容、DevOps 工具链和安全集成。
- Azure App Service 独有 Deployment Slots，蓝绿发布体验优秀，DevOps 与 Azure DevOps/GitHub Actions 深度集成。
- AWS Elastic Beanstalk、App Runner、Lambda 支持多种部署方式和自动扩缩容，CI/CD 工具丰富。
- GCP App Engine/Cloud Run 支持流量拆分、修订管理，DevOps 与 Cloud Build/Deploy 集成紧密。
- 网络隔离、身份集成、日志监控等能力均完善，建议结合业务需求和平台生态选择最佳方案
---

## ✅ 小测验回顾（5 / 5）

### 1. Deployment Slots 的作用？
- ✅ A. 实现蓝绿部署减少发布风险

### 2. 支持的部署方式？
- ✅ A. Git Push  
- ✅ B. FTP/SFTP  
- ✅ D. ZIP Deploy  
- ❌ C. 直接修改 VM 文件系统（不支持）

### 3. Managed Identity 用途？
- ✅ A. 允许 App Service 自动获取访问 Azure 资源的权限

### 4. Private Endpoint 的作用？
- ✅ A. 提供对 App Service 的私有访问，避免公网暴露

### 5. 多语言支持方式？
- ✅ A. 通过选择不同的运行时环境和应用堆栈

---

## 📅 下一步预告 - Day 31

| 模块 | 内容 |
|------|------|
| 应用诊断与日志收集 | App Service Logs / Kudu / App Insights |
| 性能监控与可用性分析 | Auto Scale / CPU Memory Rules |
| 故障排查与重启机制 | Restart Policies / Health Checks |
