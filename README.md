# huangfuren

> 给 DeepSeek Harness（dsh）写插件。
> Plugins for **DeepSeek Harness** — the ones I actually use every day.

---

## 我在做什么

DeepSeek Harness（简称 dsh）是一个可以装插件的 Agent 运行时。下面这些插件都是「自己缺什么就补什么」造出来的：
把知识库接进对话、给长会话装一个大纲、在两台机器之间搬配置、把界面调成自己喜欢的玻璃质感。

每个插件独立仓库、独立版本、独立 CI，可以单独安装与卸载；**都不带安装生命周期脚本**（没有 `postinstall`），
发布包已包含构建产物，安装不需要本地构建。

## 插件

| 插件 | 做什么 | 形态 | 版本 |
| --- | --- | --- | --- |
| [**dsh-outline**](https://github.com/huangfuren/dsh-outline) | 在对话里搜索 / 读取 / 写入自建 Outline 知识库。13 个 `outline_*` 工具；写操作走目录白名单 + 逐次审批，可写目录留空即全库只读 | host 工具 | 0.8.2 |
| [**dsh-conversation**](https://github.com/huangfuren/dsh-conversation) | 会话大纲面板：以你的每次提问为根节点，把回复里的 Markdown 标题挂成子树；搜索、书签、导出、阅读位置记忆 | 客户端 UI | 1.3.1 |
| [**dsh-bg-plugin**](https://github.com/huangfuren/dsh-bg-plugin) | 全窗口背景图：上传、实时预览、透明度 / 亮度 / 遮罩 / 模糊 / 位置 / 填充，配置落盘持久化 | host + client 双包 | 1.0.1 |
| [**dsh-client-ui-aqua**](https://github.com/huangfuren/dsh-client-ui-aqua) | 玻璃拟态主题：磨砂玻璃材质 + 流体 WebGL 背景或自定义壁纸，模糊 / 磨砂 / 亮度可调 | 客户端 UI | 1.1.0 |
| [**dsh-client-ui-balance**](https://github.com/huangfuren/dsh-client-ui-balance) | 会话头部实时显示 provider API 余额 | 客户端 UI | 0.4.0 |
| [**dsh-localsend**](https://github.com/huangfuren/dsh-localsend) | 局域网传文件：LocalSend v2 协议发送、临时 HTTP 下载链接分享、SMB 指定目录推送 | host 工具 | 0.4.1 |
| [**dsh-sync**](https://github.com/huangfuren/dsh-sync) | 把本机 dsh 配置导出成可移植 bundle（settings / 凭据可选 / 插件源码）并生成自安装脚本 | host 工具 | 0.3.0 |

## 安装

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

Plugins for **DeepSeek Harness (dsh)**, built out of my own daily needs:

- **dsh-outline** — search, read and safely write a self-hosted Outline knowledge base from the chat
- **dsh-conversation** — conversation outline panel (your questions as roots, Markdown headings beneath)
- **dsh-bg-plugin** — persistent full-window background image (two packages: host + client)
- **dsh-client-ui-aqua** — glassmorphism theme with a fluid WebGL or wallpaper backdrop
- **dsh-client-ui-balance** — live provider API balance in the session header
- **dsh-localsend** — LAN file transfer (LocalSend v2, temporary HTTP share links, SMB push)
- **dsh-sync** — export this machine's dsh config as a portable, self-installing bundle

```bash
dsh plugin --profile <web|desktop> add git+https://github.com/huangfuren/<repo>.git
```

MIT licensed. Every plugin degrades gracefully when the host API changes instead of taking dsh down.
