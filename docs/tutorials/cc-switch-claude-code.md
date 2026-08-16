---
title: CC-Switch接入Claude code教程
category: kehuduan
order: 11
description: 使用 CC-Switch 管理 Z-API Claude Code 配置并完成连接验证
updated: 2026-08-16T13:29
---

CC-Switch 可以集中管理 Claude Code 的供应商配置。完成设置后，可以通过图形界面快速切换 API 服务，无需反复手动修改配置文件或环境变量。

> **适用范围：** 本文配置的是 Claude Code CLI。开始前请退出正在运行的 Claude Code，并关闭仍在使用旧配置的终端窗口。

## 一、准备工作

开始前请确认以下条件：

- 已注册 Z-API，并能进入用户中心。
- 已安装 Node.js 18 或更高版本。
- 已安装 Claude Code。
- CC-Switch 能正常启动。

在终端依次执行 `node --version` 和 `claude --version` 检查环境。

如果系统找不到 `claude` 命令，可以先安装官方 CLI：

安装命令：`npm install -g @anthropic-ai/claude-code`

## 二、安装 CC-Switch

从 [CC-Switch GitHub Releases](https://github.com/farion1231/cc-switch/releases/latest) 下载最新稳定版。

| 系统 | 安装方式 |
| --- | --- |
| Windows | 下载普通的 `.msi` 安装包并按提示安装。 |
| macOS | 推荐使用 Homebrew，执行下方命令。 |
| Debian / Ubuntu | 下载与处理器架构匹配的 `.deb` 文件，再使用 `apt` 安装。 |

macOS 安装步骤：

1. 添加软件源：`brew tap farion1231/ccswitch`
2. 安装 CC-Switch：`brew install --cask cc-switch`

Debian / Ubuntu 安装示例：`sudo apt install ./cc-switch_x.x.x_amd64.deb`

请将示例文件名替换为实际下载的版本。其他 Linux 发行版请在 Releases 页面选择对应格式。

## 三、创建 Z-API 密钥

1. 打开 [Z-API 控制台](https://api.zicc.cc)，进入 **API 密钥** 页面。
2. 创建或选择 **CC** 分组的密钥。
3. 复制 API Key，并记录页面显示的 Claude Code Base URL。

本文使用以下配置作为示例：

| 配置项 | 示例值 |
| --- | --- |
| 分组 | `CC` |
| Base URL | `https://api.zicc.cc` |
| API Key | `sk-你的Z-API密钥` |

> **注意：** Claude Code 和 Codex 使用的密钥分组不同。Base URL 与可用模型请以 Z-API 用户中心显示的信息为准，不要公开真实 API Key。

## 四、在 CC-Switch 中添加 Claude 配置

1. 打开 CC-Switch，在顶部工具分组中选择 **Claude**。
2. 点击添加供应商，选择自定义供应商或可编辑的兼容模板。
3. 将供应商名称填写为 `Z-API`。
4. 按下表填写连接信息；不同版本的字段名称可能略有差异。

| CC-Switch 字段 | 填写内容 |
| --- | --- |
| 供应商名称 | `Z-API` |
| Base URL / API URL | Z-API 用户中心显示的 Claude Code Base URL |
| API Key / Auth Token | CC 分组的 API Key |
| 默认模型 | 选择账号实际可用的 Claude 兼容模型 |

5. 确认无误后点击 **添加**。
6. 返回供应商列表，在 Z-API 配置右侧点击 **启用**。
7. 状态显示为 **使用中** 后，进入 CC-Switch 左上角的 **设置**。
8. 在通用设置中找到 **跳过 Claude Code 初次安装确认** 并启用。
9. 关闭原有终端，再打开一个新的终端窗口。

如果 CC-Switch 提示导入或备份已有配置，建议先备份。Claude Code 的常见配置目录如下：

| 系统 | 配置目录 |
| --- | --- |
| Windows | `C:\Users\你的用户名\.claude` |
| macOS / Linux | `~/.claude` |

## 五、验证配置

在新终端中运行 `claude`。

进入对话界面后发送一个简单问题。如果能够正常返回结果，说明配置已经生效。

建议同时检查以下项目：

- CC-Switch 中 Z-API 的状态仍为 **使用中**。
- 使用的是 CC 分组密钥，不是 Codex 或其他分组密钥。
- Z-API 控制台中能够看到对应的调用记录和消耗。
- 新终端没有被旧的手动环境变量覆盖。

## 六、常见问题

### 启动后仍要求官方账号登录

确认 CC-Switch 已启用 **跳过 Claude Code 初次安装确认**，然后完全退出 Claude Code 并重新打开终端。

### 提示 401 或 API Key 无效

重新复制密钥，确认没有多余空格，并检查密钥是否属于 `CC` 分组。

### 切换后仍在使用旧地址

检查系统中是否保留了手动设置的 `ANTHROPIC_BASE_URL`、`ANTHROPIC_API_KEY` 或 `ANTHROPIC_AUTH_TOKEN`。旧环境变量可能覆盖 CC-Switch 写入的配置，修改后需要重新打开终端。

### CC-Switch 无法写入配置

检查 `.claude` 配置目录是否存在，并确认当前用户对该目录拥有写入权限。Windows 用户不建议用不同权限级别分别运行终端和 CC-Switch。

### 需要切换回其他供应商

在 CC-Switch 的 Claude 分组中选择目标供应商并点击 **启用**。切换后重新启动 Claude Code，避免旧会话继续使用缓存配置。
