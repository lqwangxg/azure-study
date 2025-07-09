
# AZ-104 学习笔记 - Day 41

## 🛠 Azure 自动化与模板部署

---

## 📦 ARM Template（Azure Resource Manager）

### ✅ 特点
- 使用 JSON 格式定义资源
- 声明式语法：定义“期望的状态”，由 Azure 负责创建与配置
- 支持：
  - 参数（parameters）
  - 变量（variables）
  - 条件语句（conditions）
  - 输出（outputs）
  - 循环与嵌套模板（linked templates）
- 可部署：VM、存储、VNet、RBAC、策略等所有资源

---

## 🪄 Bicep 模板语言

### ✅ 特性
- 高级抽象语言，**最终会编译为 ARM JSON**
- 语法简洁、可读性强，官方推荐替代 ARM Template
- 支持：
  - 参数默认值、类型校验、枚举
  - `module` 模块化复用
  - `import`、`resource`、`output` 结构清晰

### ✅ 工具集成
- 与 Azure CLI、PowerShell、VS Code 集成良好
- 可自动编译并提交部署：
  ```bash
  az deployment sub create --location japaneast --template-file main.bicep
  ```

---

## ⚙️ 自定义脚本扩展（Custom Script Extension）

### ✅ 场景
- 虚拟机初始化阶段自动执行脚本
- 部署第三方软件（如 nginx、Python、agent）
- 初始配置网络、防火墙、注册计划任务等

### ✅ 脚本来源
- 支持从：
  - Azure Storage 下载脚本
  - GitHub 或公开 URL 加载
  - 直接嵌入 base64 脚本内容

---

## 🧱 Azure Blueprints

### ✅ 功能
- 用于定义一整套**合规部署结构**
- 包含内容：
  - ARM 模板（资源）
  - Azure Policy（策略）
  - RBAC 分配（权限）
  - Resource Group 架构

### ✅ 特点
- 支持版本控制
- 可将 Blueprint 分配至多个订阅
- 常用于大型组织推行“治理结构模板”

---

## ✅ 小测验复盘（得分：13 / 18）

### 问题 1：ARM 模板
✅ B, C  
❌ A：ARM 使用 JSON  
❌ D：支持策略与 RBAC 资源

### 问题 2：Bicep 模板
✅ A, B, D  
❌ C：Bicep 更简单

### 问题 3：Custom Script Extension
✅ A, C, D  
❌ B：诊断数据应使用 Azure Monitor/Agent

### 问题 4：Blueprints
✅ A, B, C  
❌ D：支持所有资源类型，不局限 VM

### 问题 5：Bicep vs ARM
✅ A, D  
❌ B：ARM 支持参数  
❌ C：Bicep 需编译成 JSON 部署

---

## 📘 下一步预告 - Day 42 模块

| 模块                       | 内容概览                           |
|----------------------------|------------------------------------|
| Azure Monitor 深度讲解     | 日志与指标、Log Analytics、诊断设置 |
| 自动化响应与警报           | Metric/Log 警报、自动缩放、Webhook |
| 网络诊断工具               | Connection Troubleshoot、NSG flow log 等 |

