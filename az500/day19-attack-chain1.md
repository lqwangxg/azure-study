# 🛡️ Azure 模拟入侵演练手册

## 📌 演练目标

通过模拟攻击行为验证 Defender for Cloud 与 Microsoft Sentinel 的日志捕捉、告警能力，提升蓝队安全监控与响应能力。

## 🎯 攻击路径总览（Attack Chain）

Recon → Initial Access → Privilege Escalation → Lateral Movement → Data Exfiltration


## 🧱 演练准备

| 项目         | 内容说明 |
|--------------|----------|
| 🔐 权限要求   | 执行人员需具备资源组 Owner 权限或模拟子账户权限 |
| 🧪 演练环境   | 非生产测试环境，建议快照 VM 和资源以便恢复 |
| 📄 日志配置   | 启用以下日志记录组件：<br>- Azure Defender for Cloud<br>- Microsoft Sentinel<br>- Log Analytics<br>- NSG Flow Logs<br>- Azure Diagnostics Settings |

## 🧨 攻击模拟步骤与命令

### ✅ Step 1. 外部端口扫描（Reconnaissance）

**模拟行为**：通过外部扫描工具识别 Azure 虚拟机暴露的端口

```bash
nmap -Pn -p 22,3389 <your-vm-public-ip>
```

#### 验证日志：
- NSG Flow Logs：查看入站扫描流量
- Azure Firewall Logs：查看威胁行为（需部署防火墙）
- Defender for Cloud：应提示“暴露的端口”警告

### ✅ Step 2. SSH 弱口令攻击（Initial Access）
模拟行为：尝试暴力破解 SSH 登录凭据
```bash
hydra -l azureuser -P ./passwords.txt ssh://<your-vm-public-ip>
```
#### 验证日志：
- Microsoft Sentinel：触发暴力破解规则（如 SSH Brute Force）
- Defender for Cloud：应有安全警告，可能为“SSH brute-force attack detected”
- Azure AD Identity Protection：若通过 Azure Bastion 登录，可能触发异常登录警告

### ✅ Step 3. 权限提升（Privilege Escalation）
模拟行为：提升本地权限或更改用户组
```bash
# 添加用户到 sudoers
sudo usermod -aG sudo newuser

# 查看 sudo 权限
sudo -l
```
#### 验证日志：
- Defender for Endpoint（需启用并关联 VM）：检测本地提权行为
- Sentinel 自定义规则：可通过 syslog/sysmon 捕获 sudo 修改记录

### ✅ Step 4. 横向移动（Lateral Movement）
模拟行为：连接内网另一台虚拟机
```bash
ssh newuser@10.0.2.5
```
#### 验证日志：
- NSG Flow Logs：查看内部 IP 间通信流量
- Sentinel：检测异常内部连接行为
- Defender for Cloud：可能识别横向移动行为

### ✅ Step 5. 数据导出（Data Exfiltration）
#### 💾 (A) 通过 AzCopy 下载 Blob Storage 数据

```bash
azcopy copy "https://<storage-account>.blob.core.windows.net/data?<SAS>" "./downloaded" --recursive
```

#### 💾 (B) 通过 RDP 下载敏感文件
```powershell
# 在 RDP 会话中使用 WinSCP 下载文件到外部服务器
```
#### 验证日志：
- Storage Analytics Logs（需启用诊断）：查看访问者 IP、SAS 使用情况
- Sentinel：监控 AzureStorageAnalytics 日志，异常下载行为
- Defender for Cloud：标记为“潜在数据泄露”风险事件

### 🔍 日志验证清单（蓝队参考）
| 日志源                      | 应捕获事件或行为                                              |
| ------------------------ | ----------------------------------------------------- |
| ✅ NSG Flow Logs          | 端口扫描、横向连接行为                                           |
| ✅ Azure Firewall         | 来自恶意 IP 的入站尝试、异常协议流量                                  |
| ✅ Azure Activity Log     | 用户登录、权限修改、资源操作                                        |
| ✅ Defender for Cloud     | 提示暴力破解、暴露端口、异常登录、可疑数据导出                               |
| ✅ Microsoft Sentinel     | 自定义或默认规则告警：<br>- SSH/RDP 暴力破解<br>- 异常数据流量<br>- 内部横向访问 |
| ✅ Log Analytics / Syslog | 提权行为日志、用户组修改、执行历史                                     |

### 🧪 建议补充内容（可选）
- 部署 Honeytoken 文件在 VM 或 Blob Storage 中，用于检测文件访问
- 配置 Sentinel Notebook 自动响应规则（Playbooks）
- 加入 MITRE ATT&CK 映射标识每步攻击

### ✅ 注意事项
- 本演练适用于教学与蓝队防御验证，请勿在生产环境中执行。
- 所有脚本应提前备案，避免误触 SIEM 实时响应或影响合规性。
- 使用模拟攻击工具（如 Kali、Hydra、nmap）需明确测试用途。

>作者注：以上脚本及清单可以用于日常安全演练、攻防实战训练或安全团队培训，推荐结合 Azure Lighthouse 管理多订阅、统一审计和响应。

---
