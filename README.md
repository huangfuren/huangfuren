# huangfuren

> 做基础设施的那一类人：网络、负载均衡、集群、监控。
> 顺手把每天重复的活儿做成工具 —— 其中一部分开源在这里。
>
> I work on infrastructure: networking, load balancing, clusters, monitoring.
> I turn the repetitive parts into tools — a few of them are open-sourced here.

---

## 关于我

日常围绕 K8s 集群、网络与监控体系转：写变更文档、排障、做容量规划、处理网络隔离这类线上事件。过程中「自己缺什么就补什么」的小工具，攒成了一部分开源项目。

**在做的方向**

- **网络与负载均衡** —— 在线 / 离线网络、LB、SSL、DNS/CDN 接入、网络隔离事件处置
- **可观测性** —— Prometheus / Grafana / Consul 服务发现、告警链路、监控组件运维
- **Kubernetes** —— 多集群拓扑规划、IP 规划、部署与变更记录
- **工程化** —— 把重复劳动做成 CLI / 插件 / 自动化脚本，附测试与 CI

**技术栈**：Kubernetes · Linux · Prometheus / Grafana · Nginx / LB · Shell / PowerShell · Node.js · Python · Git

---

## 在做什么

### 一、DSH 插件（DeepSeek Harness）

[DeepSeek Harness（dsh）](https://github.com/huangfuren/awesome-dsh-plugin) 是一个可装插件的 Agent 运行时。下面这些插件都是「自己缺什么就补什么」造出来的：把知识库接进对话、给长会话装一个大纲、在两台机器之间搬配置、把界面调成自己喜欢的玻璃质感。

每个插件独立仓库、独立版本、独立 CI，可单独安装与卸载；**都不带安装生命周期脚本**（没有 `postinstall`），发布包已包含构建产物，安装不需要本地构建。

| 插件 | 做什么 | 形态 | 版本 |
| --- | --- | --- | --- |
| [**dsh-outline**](https://github.com/huangfuren/dsh-outline) | 在对话里搜索 / 读取 / 写入自建 Outline 知识库。13 个 `outline_*` 工具；写操作走目录白名单 + 逐次审批，可写目录留空即全库只读 | host 工具 | 0.8.2 |
| [**dsh-conversation**](https://github.com/huangfuren/dsh-conversation) | 会话大纲面板：以你的每次提问为根节点，把回复里的 Markdown 标题挂成子树；搜索、书签、导出、阅读位置记忆 | 客户端 UI | 1.3.1 |
| [**dsh-bg-plugin**](https://github.com/huangfuren/dsh-bg-plugin) | 全窗口背景图：上传、实时预览、透明度 / 亮度 / 遮罩 / 模糊 / 位置 / 填充，配置落盘持久化 | host + client 双包 | 1.0.1 |
| [**dsh-client-ui-aqua**](https://github.com/huangfuren/dsh-client-ui-aqua) | 玻璃拟态主题：磨砂玻璃材质 + 流体 WebGL 背景或自定义壁纸，模糊 / 磨砂 / 亮度可调 | 客户端 UI | 1.1.0 |
| [**dsh-client-ui-balance**](https://github.com/huangfuren/dsh-client-ui-balance) | 会话头部实时显示 provider API 余额 | 客户端 UI | 0.4.0 |
| [**dsh-localsend**](https://github.com/huangfuren/dsh-localsend) | 局域网传文件：LocalSend v2 协议发送、临时 HTTP 下载链接分享、SMB 指定目录推送 | host 工具 | 0.4.1 |
| [**dsh-sync**](https://github.com/huangfuren/dsh-sync) | 把本机 dsh 配置导出成可移植 bundle（settings / 凭据可选 / 插件源码）并生成自安装脚本 | host 工具 | 0.3.1 |

### 二、运维 / SRE 工具链

> 🚧 整理中。脱离 dsh 生态的独立脚本、文档与自动化工具会陆续开源到这里。

**为什么单列这一块**：上面那批插件只是工程能力的一个切面 —— 它们恰好长在 dsh 这个平台上。主线其实是运维：网络、集群、监控、变更管理。这一块会放与具体 Agent 平台无关的东西，比如集群巡检脚本、监控配置生成器、变更记录模板等。

---

## 精选：几个值得一看的

- **[dsh-outline](https://github.com/huangfuren/dsh-outline)** —— 想看**分层降级与安全边界**怎么设计：写操作走白名单 + 逐次审批、宿主 API 缺失时只禁用自身能力而不拖垮整个进程。
- **[dsh-localsend](https://github.com/huangfuren/dsh-localsend)** —— 想看**零第三方依赖**能走多远：零依赖 zip 写入器、LocalSend v2 协议发送端、临时 HTTP 分享、SMB 推送，全部只用 Node 内置模块。
- **[dsh-sync](https://github.com/huangfuren/dsh-sync)** —— 想看**迁移工具怎么设计才不丢东西**：整包 sha256 校验、覆盖前备份、失败自动回滚、凭据 AES-256-GCM 加密搬运。

---

## 安装（dsh 插件）

命令行安装，网页版 profile 是 `web`，桌面版是 `desktop`：

```bash
# 固定到某个发布版（推荐）
dsh plugin --profile web add git+https://github.com/huangfuren/dsh-outline.git#v0.8.2

# 跟随 main 分支
dsh plugin --profile desktop add git+https://github.com/huangfuren/dsh-bg-plugin.git
```

桌面版也可以在「设置 → 插件 → 添加插件」里直接填 GitHub 地址或本地目录。装完重启 dsh。

## 兼容性

在 dsh `0.1.x` 与 `0.2.0-rc.x` 上跑过。插件对宿主 API 变化做了降级：拿不到某个服务就只禁用自身那部分能力并打一条 `console.warn`，不会让整个 dsh 起不来。

## 许可

[MIT](https://opensource.org/license/mit)

---

## English

I work on **infrastructure** — networking, load balancing, clusters and monitoring — and I turn the repetitive parts of that job into open-source tools.

**Focus areas:** network & load balancing (online/offline networks, LB, SSL, DNS/CDN, isolation incidents) · observability (Prometheus / Grafana / Consul) · Kubernetes (multi-cluster topology, IP planning, change records) · engineering (CLIs, plugins and automation, with tests and CI).

**Stack:** Kubernetes · Linux · Prometheus / Grafana · Nginx / LB · Shell / PowerShell · Node.js · Python · Git

### DSH plugins (DeepSeek Harness)

Tools I built out of my own daily needs — each in its own repo with its own version and CI:

- **dsh-outline** — search, read and safely write a self-hosted Outline knowledge base from the chat
- **dsh-conversation** — conversation outline panel (your questions as roots, Markdown headings beneath)
- **dsh-bg-plugin** — persistent full-window background image (two packages: host + client)
- **dsh-client-ui-aqua** — glassmorphism theme with a fluid WebGL or wallpaper backdrop
- **dsh-client-ui-balance** — live provider API balance in the session header
- **dsh-localsend** — LAN file transfer (LocalSend v2, temporary HTTP share links, SMB push)
- **dsh-sync** — export this machine's dsh config as a portable, self-installing bundle

### Ops / SRE toolchain

> 🚧 In progress. Standalone scripts, docs and automation that are not tied to any particular agent platform will land here.

The plugins above are only one facet of what I build — they just happen to live on the dsh platform. The main thread is ops: networking, clusters, monitoring, change management.

```bash
dsh plugin --profile <web|desktop> add git+https://github.com/huangfuren/<repo>.git
```

MIT licensed. Every plugin degrades gracefully when the host API changes instead of taking dsh down.
