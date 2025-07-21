# AZ-104 学习笔记 - Day 29

## 🚦 应用交付与全球流量管理

---

## 🏗️ Application Gateway（AGW）

- L7 层（应用层）负载均衡服务
- 适用于 HTTP/HTTPS 流量控制
- 支持 URL 路由、SSL 卸载、会话保持、WAF（Web 应用防火墙）

### 核心功能：

| 功能           | 描述 |
|----------------|------|
| 路径转发       | 按 URL 路径将请求分发到不同后端池 |
| 多站点托管     | 一个网关支持多个域名（SNI） |
| SSL 卸载       | 在 AGW 层终止 HTTPS，减轻后端压力 |
| Cookie 会话保持 | 支持黏性会话绑定用户与后端 |
| Web 应用防火墙（WAF） | 防护 SQL 注入、XSS、命令注入等攻击 |

---

## 🚀 Azure Front Door（AFD）

- 全球边缘分发服务，L7 路由优化
- 面向公网用户设计，不适用于 VNet 内部通信

### 功能特性：

| 功能           | 描述 |
|----------------|------|
| 全球接入       | 用户请求就近进入 Microsoft Edge 节点 |
| 路由优化       | 最优路径转发，提高可用性与性能 |
| 静态内容缓存   | 类似 CDN，支持缓存常见资源 |
| 健康探测与容错 | 自动探测后端，失效时快速切换 |
| 支持 SSL 卸载 | 统一 HTTPS 接入点，安全合规 |

---

## 📦 Azure CDN

- 内容分发网络，缓存静态文件（图片、视频、JS/CSS 等）
- 提高访问速度，减少源站负载
- 支持多厂商（Microsoft、Akamai、Verizon）

---

## 🧭 DNS 流量控制：Traffic Manager

- Azure 的 DNS 层全局负载服务
- 不处理实际流量，只返回最佳 IP 地址（DNS 层分发）

### 路由方法：

| 策略类型     | 描述 |
|--------------|------|
| 性能优先      | 返回延迟最低的后端地址 |
| 加权分发      | 按权重分配流量 |
| 地理路由      | 按用户地理位置返回最近区域 IP |
| 优先级路由    | 设置主备策略，主服务不可用时切换至备份 |
---

## 🌐 Azure、AWS、GCP 应用交付与全球流量管理对比

| 维度               | Azure                                              | AWS                                              | GCP                                              |
|--------------------|----------------------------------------------------|--------------------------------------------------|--------------------------------------------------|
| L7 应用网关        | Application Gateway（支持 WAF、SSL 卸载、路径路由）| Application Load Balancer（ALB，支持WAF、SSL等）  | HTTP(S) Load Balancer（全球、WAF、SSL、智能路由） |
| L4 负载均衡        | Load Balancer（Basic/Standard，TCP/UDP）           | Network Load Balancer（NLB，TCP/UDP）             | Network Load Balancer（TCP/UDP）                  |
| 全球流量管理       | Traffic Manager（DNS 级，支持多种路由策略）        | Route 53（DNS 级，地理/加权/延迟等路由策略）      | Cloud DNS 负载均衡、HTTP(S) LB（全球智能调度）    |
| Anycast 支持       | 支持（Front Door、Traffic Manager）                | 支持（Global Accelerator、Route 53）              | 支持（HTTP(S) LB、Cloud CDN）                     |
| CDN 集成           | Azure CDN、Front Door（全球加速+WAF+SSL）          | CloudFront（全球加速+WAF+SSL）                    | Cloud CDN（全球加速+SSL）                         |
| 智能路由           | Front Door（基于健康、性能、地理等智能路由）        | Route 53、Global Accelerator（健康、地理、性能）  | HTTP(S) LB（基于地理、延迟、健康等智能路由）      |
| 多区域部署         | 支持多区域后端池、自动故障转移                    | 支持多区域目标组、自动故障转移                    | 支持多区域后端服务、自动故障转移                  |
| WAF 集成           | Application Gateway WAF、Front Door WAF            | ALB WAF、CloudFront WAF                          | HTTP(S) LB WAF                                   |
| SSL 终结           | 支持（App Gateway、Front Door、CDN）               | 支持（ALB、CloudFront、NLB）                      | 支持（HTTP(S) LB、Cloud CDN）                     |

**总结：**
- 三大云厂商均支持 L4/L7 负载均衡、全球流量管理、CDN 加速、WAF 和 SSL 终结。
- Azure Front Door、AWS Global Accelerator、GCP HTTP(S) LB 均可实现全球智能流量调度和高可用。
- DNS 级流量管理（Traffic Manager、Route 53、Cloud DNS）适合多区域灾备和跨云场景。
- 建议结合业务全球分布、性能、合规和安全需求选择最佳应用交付与流量管理方案
---

## ✅ 小测验回顾（4 / 5）

### 1. AGW 哪项功能属于 L7 负载均衡？
- ✅ A. 基于路径的路由

### 2. Azure Front Door 正确说法？
- ✅ A. 工作于全球边缘节点，提供最低延迟访问  
- ✅ B. 可缓存静态文件  
- ✅ D. 支持 SSL 卸载与健康探测  
- ❌ C. 错：Front Door 不支持 VNet 内部通信

### 3. CDN 的主要作用？
- ✅ A. 提高内容交付速度

### 4. Traffic Manager 正确描述？
- ✅ B. 基于 DNS 层实现全球流量分发

### 5. AGW 的 WAF 防护内容：
- ✅ A. SQL 注入  
- ✅ B. 跨站脚本（XSS）  
- ✅ D. Cookie 注入  
- ❌ C. 操作系统漏洞需靠补丁与主机安全

---

## 📅 下一步预告 - Day 30

| 模块                       | 内容 |
|----------------------------|------|
| Azure App Service 平台特性 | 路径映射、配置管理、运行时隔离 |
| DevOps 与部署机制         | Deployment Slots, CI/CD |
| 身份验证与混合连接         | Private Endpoint + 身份平台接入 |
