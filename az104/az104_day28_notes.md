
# AZ-104 学习笔记 - Day 28

## 🌐 网络通信机制：Service Endpoint / Private Endpoint / DNS / Load Balancer

---

## 🔌 Service Endpoint vs Private Endpoint

| 特性                  | Service Endpoint                         | Private Endpoint                            |
|-----------------------|-------------------------------------------|---------------------------------------------|
| 连接方式              | VNet → Azure 服务（微软骨干网络）         | VNet → Azure 服务（专属私有 IP）            |
| IP 类型              | 公网服务 IP（增强路径安全）               | VNet 内专属私有 IP                          |
| DNS 支持              | 默认公网 DNS                             | 推荐搭配 Private DNS Zone                   |
| 安全性                | 中等                                      | 高（服务从公网中完全隐藏）                 |
| 支持服务              | Storage, SQL, CosmosDB 等多数 PaaS 服务   | 同上，支持列表不断扩展                     |
| 使用场景              | 简单网络隔离、快速启用                    | 企业内网、敏感数据、封闭网络                |

---

## ⚖️ Azure Load Balancer 类型对比

| 类型                    | 特点 |
|-------------------------|------|
| Basic Load Balancer     | 区域内、L4 层（TCP/UDP）、非高可用、开发测试使用 |
| Standard Load Balancer  | 支持区域冗余、高可用、NSG 集成、诊断支持、建议生产使用 |
| Internal Load Balancer  | 仅在 VNet 内部访问，用于 VM 之间私有通信 |

- L4 层工作原理：基于 IP 和端口进行 TCP/UDP 转发（不分析应用内容）
- 不支持 HTTPS 路由、Cookie 关联等 L7 功能
- L7 层 路由通过Application gateway实现

---

## 🧭 Azure DNS 与 Private DNS Zone

### Azure DNS
- 公网 DNS 服务
- 支持 A、CNAME、MX、TXT 等记录
- 可托管自定义域名（如 contoso.com）

### Private DNS Zone
- VNet 内部解析私有域名（如 blob.core.windows.net → 私有 IP）
- 通常用于配合 Private Endpoint
- 可自动注册记录（例如 Azure VM）
---

## 🌐 Azure、AWS、GCP 关于 Endpoint、DNS、负载均衡的对比

| 维度             | Azure                                      | AWS                                         | GCP                                         |
|------------------|--------------------------------------------|---------------------------------------------|---------------------------------------------|
| 私有接入 Endpoint | Service Endpoint（增强路径安全，公网 IP）<br>Private Endpoint（专属私有 IP，完全私网） | VPC Endpoint（Gateway/Interface，专属私有 IP） | Private Service Connect（PSC，专属私有 IP） |
| DNS 服务         | Azure DNS（公网）、Private DNS Zone（VNet 内部） | Route 53（公网/私有）、Private Hosted Zone   | Cloud DNS（公网/私有）、Private DNS Zone    |
| DNS 集成         | Private Endpoint 推荐配合 Private DNS Zone  | VPC Endpoint 自动注册私有 DNS                | PSC 支持自动注册私有 DNS                    |
| 负载均衡类型     | Load Balancer（L4，Basic/Standard）<br>Application Gateway（L7）<br>Traffic Manager（DNS 级） | Classic/Network Load Balancer（L4）<br>Application Load Balancer（L7）<br>Global Accelerator、Route 53 | Network Load Balancer（L4）<br>HTTP(S) Load Balancer（L7，全球）<br>Cloud DNS 负载均衡 |
| 内部负载均衡     | Internal Load Balancer（VNet 内部）         | Internal NLB/ALB（VPC 内部）                 | Internal TCP/UDP/HTTP(S) Load Balancer      |
| 公网负载均衡     | Public Load Balancer、Application Gateway   | NLB/ALB、Elastic Load Balancer              | HTTP(S) Load Balancer（全球）、TCP/UDP LB   |
| 流量管理         | Traffic Manager（DNS 级流量分发）           | Route 53（DNS 级流量策略）、Global Accelerator | Cloud DNS 轮询、HTTP(S) LB 智能路由         |
| 安全集成         | 支持 NSG、WAF、Private Endpoint             | 支持 Security Group、WAF、VPC Endpoint      | 支持防火墙规则、WAF、PSC                    |

**总结：**
- 三大云厂商均支持私有 Endpoint（专属私有 IP）、公网/私有 DNS、L4/L7 负载均衡和流量管理。
- Azure Private Endpoint、AWS VPC Endpoint、GCP PSC 实现方式类似，均可实现服务私网化和 DNS 自动集成。
- 负载均衡能力均覆盖 L4/L7，GCP HTTP(S) LB 支持全球流量调度，Azure Application Gateway 支持 WAF。
- DNS 服务均支持公网和私有解析，建议结合业务安全、性能和全球分
---

## ✅ 小测验回顾（3 / 5）

### 1. 关于 Service Endpoint：

- ✅ B. 它将流量通过 Azure 骨干网转发（提升安全性但仍使用服务公网 IP）

### 2. Private Endpoint 正确说法：

- ✅ A. 绑定服务到 VNet 的私有 IP  
- ✅ B. 推荐配置 DNS，确保正确解析  
- ✅ C. 提供更高的隔离性与安全性  
- ❌ D. 错，支持多个服务（不止 Blob）

### 3. 内部服务的负载均衡方式：

- ✅ B. Internal Load Balancer（仅用于私网访问）

### 4. Azure DNS 与 Private DNS Zone 区别：

- ✅ A. Azure DNS 用于公网解析，Private DNS Zone 用于 VNet 内部解析

### 5. Standard Load Balancer 的优势：

- ✅ A. 支持 NSG 集成  
- ✅ B. 支持多区域负载均衡  
- ❌ C. 错误：不支持 L7 应用层路由，属 Application Gateway  
- ✅ D. 支持诊断与日志输出

---

## 📅 下一步预告 - Day 29

| 模块 | 内容 |
|------|------|
| Azure 应用路由服务 | Application Gateway 与 WAF |
| Azure Front Door 和 CDN | 全球分发加速解决方案 |
| 统一通信管理 | DNS 负载均衡 vs 流量控制 |
