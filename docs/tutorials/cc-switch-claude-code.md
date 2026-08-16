---
title: CC-Switch接入Claude客户端教程
category: kehuduan
order: 11
description: 配置 Z-API Claude 客户端，并区分 Claude Code 与 Claude Desktop 的接入方式
updated: 2026-08-16T13:50
---

Claude 客户端分为 Claude Code 和 Claude Desktop，两者的配置方式不同。CC-Switch 管理的是 **Claude Code** 配置；Claude Desktop 必须使用客户端内置的第三方推理设置，不能直接套用 CC-Switch 的 Claude Code 配置。

| 使用的客户端 | 正确接入方式 |
| --- | --- |
| Claude Code | 使用 CC-Switch 的 **Claude** 分组，按本文第三至第五节操作。 |
| Claude Desktop | 使用客户端的 **Configure third-party inference**，按本文第六节操作。 |

## 一、准备客户端环境

### Claude Code

Claude Code 需要 Node.js 18 或更高版本。在终端依次执行 `node --version` 和 `claude --version` 检查环境。

如果系统找不到 `claude` 命令，执行 `npm install -g @anthropic-ai/claude-code` 安装官方客户端。

### Claude Desktop

从 [Claude 官方下载页面](https://claude.com/download) 下载与系统对应的安装包。安装完成后先启动一次客户端。

Windows 安装过程需要访问 Anthropic 服务。如果安装程序无法联网，请使用系统代理的全局模式，或在安装程序所在目录设置 `HTTP_PROXY` 和 `HTTPS_PROXY` 后重新运行安装程序。

## 二、创建 Z-API 密钥

1. 打开 [Z-API 控制台](https://api.zicc.cc)，进入 **API 密钥** 页面。
2. 创建或选择 **CC** 分组的密钥。
3. 复制 API Key，并记录页面显示的 Claude Base URL。

| 配置项 | 示例值 |
| --- | --- |
| 分组 | `CC` |
| Base URL | `https://api.zicc.cc` |
| API Key | `sk-你的Z-API密钥` |

> **注意：** Claude 客户端和 Codex 使用的密钥分组不同。Base URL 与可用模型请以 Z-API 用户中心显示的信息为准，不要公开真实 API Key。

## 三、安装 CC-Switch

从 [CC-Switch GitHub Releases](https://github.com/farion1231/cc-switch/releases/latest) 下载最新稳定版。

| 系统 | 安装方式 |
| --- | --- |
| Windows | 下载普通的 `.msi` 安装包并按提示安装。 |
| macOS | 依次执行 `brew tap farion1231/ccswitch` 和 `brew install --cask cc-switch`。 |
| Debian / Ubuntu | 下载 `.deb` 安装包，再执行 `sudo apt install ./实际文件名.deb`。 |

安装后启动 CC-Switch。如果软件提示导入已有 Claude Code 配置，建议先备份再继续。

## 四、用 CC-Switch 配置 Claude Code

1. 完全退出正在运行的 Claude Code，并关闭旧终端窗口。
2. 打开 CC-Switch，在顶部工具分组中选择 **Claude**。
3. 点击添加供应商，选择自定义供应商或可编辑的兼容模板。
4. 将供应商名称填写为 `Z-API`。
5. 按下表填写连接信息；不同版本的字段名称可能略有差异。

| CC-Switch 字段 | 填写内容 |
| --- | --- |
| 供应商名称 | `Z-API` |
| Base URL / API URL | Z-API 用户中心显示的 Claude Base URL |
| API Key / Auth Token | CC 分组的 API Key |
| 默认模型 | 选择账号实际可用的 Claude 兼容模型 |

6. 点击 **添加**，返回供应商列表后在 Z-API 右侧点击 **启用**。
7. 状态显示为 **使用中** 后，进入 CC-Switch 左上角的 **设置**。
8. 在通用设置中启用 **跳过 Claude Code 初次安装确认**。
9. 打开一个新的终端窗口并运行 `claude`。

## 五、验证 Claude Code

进入对话界面后发送一个简单问题。如果能够正常返回结果，并且 Z-API 控制台出现调用记录，说明配置已经生效。

Claude Code 的常见配置目录如下：

| 系统 | 配置目录 |
| --- | --- |
| Windows | `C:\Users\你的用户名\.claude` |
| macOS / Linux | `~/.claude` |

如果仍使用旧地址，检查系统中是否保留了手动设置的 `ANTHROPIC_BASE_URL`、`ANTHROPIC_API_KEY` 或 `ANTHROPIC_AUTH_TOKEN`。旧环境变量可能覆盖 CC-Switch 配置，修改后需要重新打开终端。

## 六、配置 Claude Desktop

Claude Desktop 不读取 CC-Switch 的 Claude Code 配置，请在客户端中单独设置：

1. 启动 Claude Desktop，进入左上角菜单中的 **Settings**。
2. 打开 **Help**、**Troubleshooting**，启用 **Enable Developer Mode**。
3. 按提示彻底退出并重新启动 Claude Desktop。
4. 重启后打开新增的 **Developer** 菜单。
5. 进入 **Configure third-party inference**。
6. 在 **Gateway base URL** 中填写 Z-API 用户中心显示的 Claude 地址，示例为 `https://api.zicc.cc`。
7. 在 **Gateway API key** 中填写 CC 分组的 API Key。
8. 启用 **Skip login-mode chooser**。
9. 点击右下角 **Apply locally**，然后重新启动客户端。

Gateway 地址必须使用 HTTPS 域名，不接受 IP、端口或 HTTP 地址。配置完成后新建对话，并在 Z-API 控制台确认调用记录。

## 七、常见问题

### Claude Code 仍要求官方账号登录

确认 CC-Switch 已启用 **跳过 Claude Code 初次安装确认**，然后关闭所有旧终端和 Claude Code 进程，再重新启动。

### Claude Desktop 找不到 Developer 菜单

确认已在 **Help**、**Troubleshooting** 中启用 Developer Mode，并彻底退出客户端后重新启动。

### 提示 401 或 API Key 无效

重新复制密钥，确认没有多余空格，并检查密钥是否属于 `CC` 分组。

### 切换后仍在使用旧配置

Claude Code 需要检查旧环境变量；Claude Desktop 需要重新点击 **Apply locally**。两种客户端修改配置后都应彻底退出并重新启动。
