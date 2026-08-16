---
title: CC-Switch接入Codex教程
category: kehuduan
order: 10
description: 使用 CC-Switch 管理 Z-API Codex 配置并验证命令行连接
updated: 2026-08-16T13:29
---

CC-Switch 可以集中管理 Codex CLI 的供应商配置。切换供应商后，工具会更新 Codex 的本地配置，适合需要在多个 API 服务之间快速切换的用户。

> **适用范围：** 本文配置的是 Codex CLI，不是 Codex 桌面端。开始前请退出正在运行的 Codex，避免旧进程继续使用切换前的配置。

## 一、准备工作

开始前请确认以下条件：

- 已注册 Z-API，并能进入用户中心。
- 已安装 Node.js 18 或更高版本。
- 已安装 Codex CLI。
- CC-Switch 能正常启动。

在终端执行以下命令检查环境：

```bash\nnode --version
codex --version\n```

如果系统找不到 `codex` 命令，可以先安装官方 CLI：

```bash\nnpm install -g @openai/codex\n```

## 二、安装 CC-Switch

从 [CC-Switch GitHub Releases](https://github.com/farion1231/cc-switch/releases/latest) 下载最新稳定版。

| 系统 | 安装方式 |
| --- | --- |
| Windows | 下载普通的 `.msi` 安装包并按提示安装。 |
| macOS | 推荐使用 Homebrew，执行下方命令。 |
| Debian / Ubuntu | 下载与处理器架构匹配的 `.deb` 文件，再使用 `apt` 安装。 |

macOS 安装命令：

```bash\nbrew tap farion1231/ccswitch
brew install --cask cc-switch\n```

Debian / Ubuntu 安装示例：

```bash\nsudo apt install ./cc-switch_x.x.x_amd64.deb\n```

请将示例文件名替换为实际下载的版本。其他 Linux 发行版请在 Releases 页面选择对应格式。

## 三、创建 Z-API 密钥

1. 打开 [Z-API 控制台](https://api.zicc.cc)，进入 **API 密钥** 页面。
2. 创建或选择 **Codex** 分组的密钥。
3. 复制 API Key，并记录页面显示的 Base URL。

本文使用以下配置作为示例：

| 配置项 | 示例值 |
| --- | --- |
| 分组 | `Codex` |
| Base URL | `https://api.zicc.cc/v1` |
| API Key | `sk-你的Z-API密钥` |

> **注意：** Base URL 和可用模型可能随账号配置变化，请以 Z-API 用户中心显示的信息为准。不要把真实 API Key 发送给他人或写入公开文档。

## 四、在 CC-Switch 中添加 Codex 配置

1. 打开 CC-Switch，在顶部工具分组中选择 **Codex**。
2. 点击添加供应商，选择自定义供应商或可编辑的兼容模板。
3. 将供应商名称填写为 `Z-API`。
4. 按下表填写连接信息；不同版本的字段名称可能略有差异。

| CC-Switch 字段 | 填写内容 |
| --- | --- |
| 供应商名称 | `Z-API` |
| Base URL / API URL | Z-API 用户中心显示的 Codex Base URL |
| API Key | Codex 分组的 API Key |
| 默认模型 | 选择账号实际可用的模型，例如 `gpt-5.5` |

5. 检查 Base URL 末尾是否包含 `/v1`，确认后点击 **添加**。
6. 返回供应商列表，在 Z-API 配置右侧点击 **启用**。
7. 状态显示为 **使用中** 后，关闭原有终端并重新打开一个终端窗口。

如果 CC-Switch 提示导入或备份已有配置，建议先备份。Codex 的常见配置目录如下：

| 系统 | 配置目录 |
| --- | --- |
| Windows | `C:\Users\你的用户名\.codex` |
| macOS / Linux | `~/.codex` |

## 五、验证配置

在新终端中运行：

```bash\ncodex\n```

进入对话界面后发送一个简单任务。如果能够正常返回结果，说明配置已经生效。

建议同时检查以下项目：

- CC-Switch 中 Z-API 的状态仍为 **使用中**。
- 使用的是 Codex 分组密钥，不是 CC 或其他分组密钥。
- 模型名称存在于 Z-API 用户中心的可用模型列表中。
- Z-API 控制台中能够看到对应的调用记录和消耗。

## 六、常见问题

### 提示 401 或 API Key 无效

重新复制密钥，确认没有多余空格，并检查密钥是否属于 `Codex` 分组。

### 提示 404 或找不到模型

检查 Base URL 是否完整，Codex 通常需要 `/v1`；同时将模型名称改为账号实际可用的模型。

### 切换后仍在使用旧配置

完全退出 Codex 和原终端，再在 CC-Switch 中重新点击 **启用**。如果仍未生效，检查 `.codex` 配置目录是否存在，以及 CC-Switch 是否有写入权限。

### 需要切换回其他供应商

在 CC-Switch 的 Codex 分组中选择目标供应商并点击 **启用**。切换后重新启动 Codex，避免旧会话继续使用缓存配置。
