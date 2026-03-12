
# 🛡️ Yokan - 自动化 Web 渗透与外网打点平台

**Yokan** 是一款专为网络安全从业人员、研究机构和企业红队设计的一体化 Web 渗透测试与外网打点平台。系统采用完全自定义的工具流架构，深度集成 ICP 官方查询、AI 智能 Fuzzing 推断以及“指纹到漏洞（Fingerprint-to-POC）”的自动化工作流。

<img width="1148" height="697" alt="Yokan Dashboard" src="https://github.com/user-attachments/assets/aed216fb-cd4a-41e6-984a-4eb9594af6ee" />

<img width="1509" height="746" alt="Yokan Workflow" src="https://github.com/user-attachments/assets/5313606e-551b-4214-83ef-acb4dc8137df" />

> **⚠️ 免责声明 (Disclaimer)**
> 本工具的开发初衷仅限于**安全研究、防御技术验证及已获授权的合规性安全测试**。
> * **合法授权：** 在使用本工具对目标系统进行探测或漏洞利用（Exploit）前，必须确保已获得目标所有者的明确书面授权。严禁用于非法入侵或数据窃取。
> * **责任豁免：** 本工具按“原样”提供。因使用本工具（包括集成的第三方脚本/模块）导致的任何直接或间接损害（如业务中断、系统瘫痪），开发者不承担任何法律及连带责任。风险由使用者自行承担。
> * **使用即同意：** 下载、安装或使用本工具即视为您已完全理解并同意本声明。
> 
> 

---

## 🚀 快速开始

基于 Docker 的一键式部署方案，包含前端 (Next.js)、后端 (Go) 与数据库。

```bash
# 构建并后台启动所有服务
docker compose up -d --build

```

**访问地址**：`http://localhost:3159`

### ⚙️ 初始化配置

1. **全局配置：** 首次登录后，请前往「全局配置」页面完成基础运行环境设置。
<img width="1503" height="741" alt="Global Configuration" src="https://github.com/user-attachments/assets/c32798db-d384-424e-b807-b4fdc616e07f" />

2. **修改默认密码：**  容器运行期间，在宿主机执行以下命令进行交互式密码修改：

```
docker exec -it yokan-backend ./change_password
```

*(密码强度要求：长度 ≥ 8 位，须包含大写/小写/数字/特殊字符中的至少 3 种)*

---

## 🧩 核心功能模块

### 1. 自动化攻击链 (工作流编排)

在「信息收集 → 攻击链管理」中，通过可视化拖拽将多个独立模块串联为自动化渗透流程。

* **可视化编排：** 支持树状分支逻辑，上游节点的输出自动作为下游节点的输入。
* **多模块支持：** 涵盖 9 大核心节点（子域名 → 存活检测 → 端口扫描 → 指纹识别 → 漏洞扫描等）。
* **内置场景：** 提供「智能目标识别攻击链」模板，实现从公司名称到漏洞扫描的全自动纵深探测。

### 2. 自定义工具生态

支持将任意本地命令行安全工具接入 Yokan 平台进行统一调度。

| 接入模式 | 说明 | 示例命令格式 |
| --- | --- | --- |
| **外部二进制** | 直接导入 Linux(amd64)/Windows 可执行文件 | `nmap -T4 {参数} -p {参数} --open` |
| **脚本解释型** | 支持 Python（需附带 `requirements.txt`）、Java | `python3 main.py -u {url}` |

*(注：占位符 `{参数名}` 由平台在执行时自动动态填充，支持高级参数映射与输出正则提取)*
<img width="1313" height="744" alt="Custom Tools" src="[https://github.com/user-attachments/assets/5ec746f6-f7ef-4df2-8b4e-ed969f0e2397](https://github.com/user-attachments/assets/5ec746f6-f7ef-4df2-8b4e-ed969f0e2397)" />

### 3. 全维度资产与信息收集

| 功能模块 | 核心能力描述 |
| --- | --- |
| **子域名与端口** | 多引擎子域枚举（字典爆破 + API）；内置扫描器结合外部工具实现服务识别 |
| **Web 指纹与资产** | 精准识别 Web 框架、CMS 及中间件；支持按项目维度批量管理与归档资产 |
| **企业拓扑发现** | 基于企业工商信息，自动化查询子公司及关联资产 |

### 4. 漏洞扫描与 POC 管理

* **POC 生态：** 兼容并支持导入基于 Nuclei 格式的 YAML POC，支持去重与本地缓存。
* **任务调度：** 支持“批量目标 × 批量 POC”的交叉扫描，内置高效的任务队列管理引擎。
* **漏洞追踪：** 扫描结果自动化归档，支持按项目、严重性维度进行交叉筛选分析。

### 5. 智能文件泄露检测 (AI Fuzz)

基于传统目录扫描的革命性升级，引入大语言模型进行推断式 Fuzzing：

* **AI Fuzz 引擎：** 自动识别目标站点的命名风格（驼峰/下划线等），横向扩展业务模块（如 `user` → `role`），并推断 CRUD 变体（如 `list` → `add/export`）。
* **精准扫描：** 13 类高危字典分类并发（备份/源码/日志/数据库等），自动提取前端 JS/HTML 中的隐藏路由。
* **流量重放：** 支持 HTTP 原始报文 Replay，使用 `§payload§` 标记注入点，实时渲染执行进度。

### 6. 辅助渗透工具集

* **密码爆破组件：** 支持 SSH/FTP/RDP/MySQL/Redis 等主流协议，允许挂载自定义字典。
* **资产与泄露探测：** 集成 GitHub 敏感信息搜集（Token/密钥）、Swagger/OpenAPI 自动化解析、Host 头碰撞检测（内网域名发现）。
* **数据处理套件：** 批量正则提取、大文本去重、Diff 差异对比工具。
* **网络空间测绘：** 深度集成 Fofa、Hunter、Quake 官方 API，提供统一检索与资产导入界面。

### 7. AI 辅助分析助手

深度接入 DeepSeek 与智谱 GLM 大模型 API：

* 提供实时的渗透测试思路推演与漏洞成因分析。
* **MCP 协议支持：** 允许通过自然语言对话，直接调度平台内的安全工具执行扫描任务。

---

## 💻 技术架构

Yokan 采用前后端分离架构，兼顾高并发处理能力与现代化交互体验。

| 架构分层 | 核心技术栈 |
| --- | --- |
| **前端展现** | Next.js 15, React, Ant Design, Tailwind CSS |
| **后端引擎** | Go, Gin, GORM |
| **数据存储** | PostgreSQL 16 |
| **部署容灾** | Docker Compose |
| **AI 驱动** | DeepSeek API / 智谱 GLM API |

### 📁 源码目录树

```text
Yokan2/
├── yokantest/              # 前端 (Next.js)
│   └── src/app/
│       ├── tools/          # 自定义工具与执行引擎
│       ├── info-gathering/ # 资产收集与攻击链编排
│       ├── file-leak/      # 目录扫描与 AI Fuzz
│       ├── vulnerabilities/# 漏洞扫描与 POC 管理
│       ├── misc-tools/     # 协议爆破与杂项工具
│       ├── search-engines/ # 空间测绘 API 集成
│       ├── fingerprints/   # 指纹识别库
│       ├── projects/       # 渗透项目管理
│       ├── assets/         # 目标资产库
│       └── ai/             # AI 智能助手
├── yokantest2/             # 后端 (Go)
│   ├── cmd/
│   │   ├── server/         # 主服务入口
│   │   └── change_password/# 密码管理 CLI 模块
│   └── internal/
│       ├── api/            # 路由与控制器
│       ├── services/       # 核心业务逻辑
│       └── models/         # 数据库 ORM 模型
├── docker-compose.yml      # 容器编排配置
├── Dockerfile.yokantest    # 前端构建镜像
└── Dockerfile.yokantest2   # 后端构建镜像

```
