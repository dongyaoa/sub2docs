---
title: CC-Switch接入Codex客户端教程
category: kehuduan
order: 10
description: 使用 CC-Switch 配置 Z-API 并在 Codex 官方客户端中生效
updated: 2026-08-16T13:50
---

本教程面向 Codex 官方客户端用户。CC-Switch 负责管理 Codex 的本地供应商配置，Codex 客户端负责 API Key 登录和日常使用；完成切换后必须彻底退出并重新启动客户端。

> **与手动配置的区别：** 手动方式需要编辑 `config.toml`，CC-Switch 会代为写入和切换供应商配置。已经使用手动配置的用户，建议先备份 `.codex` 目录。

## 一、下载并安装 Codex 客户端

| 系统 | 官方下载地址 |
| --- | --- |
| Windows | [Microsoft 安装包](https://get.microsoft.com/installer/download/9PLM9XGG6VKS?cid=website_cta_psi) |
| macOS | [OpenAI 官方下载页面](https://chatgpt.com/zh-Hans-CN/download/) |

安装完成后先启动一次 Codex，让客户端创建本地配置目录。

| 系统 | Codex 配置目录 |
| --- | --- |
| Windows | `C:\Users\你的用户名\.codex` |
| macOS | `~/.codex` |

## 二、创建密钥并登录客户端

1. 打开 [Z-API 控制台](https://api.zicc.cc)，进入 **API 密钥** 页面。
2. 创建或选择 **Codex** 分组的密钥。
3. 复制 API Key，并记录页面显示的 Base URL 和可用模型。
4. 如果 Codex 已登录个人账号，先在左下角退出登录。
5. 在登录界面选择 **API Key**，粘贴 Z-API 密钥并完成登录。

API Key 通常以 `sk-` 开头。复制时不要带入前后空格，也不要把真实密钥写入公开文档或发送给他人。

## 三、安装 CC-Switch

从 [CC-Switch GitHub Releases](https://github.com/farion1231/cc-switch/releases/latest) 下载最新稳定版。

| 系统 | 安装方式 |
| --- | --- |
| Windows | 下载普通的 `.msi` 安装包并按提示安装。 |
| macOS | 依次执行 `brew tap farion1231/ccswitch` 和 `brew install --cask cc-switch`。 |

安装后启动 CC-Switch。如果软件提示导入已有 Codex 配置，建议先选择备份，再继续操作。

## 四、在 CC-Switch 中添加 Z-API

1. 完全退出 Codex 客户端，包括仍在后台运行的进程。
2. 打开 CC-Switch，在顶部工具分组中选择 **Codex**。
3. 点击添加供应商，选择自定义供应商或可编辑的兼容模板。
4. 将供应商名称填写为 `Z-API`。
5. 按下表填写配置；不同 CC-Switch 版本的字段名称可能略有差异。

| CC-Switch 字段 | 填写内容 |
| --- | --- |
| 供应商名称 | `Z-API` |
| Base URL / API URL | Z-API 用户中心显示的地址，示例为 `https://api.zicc.cc/v1` |
| API Key | Codex 分组的 API Key |
| 默认模型 | 选择账号实际可用的模型，例如 `gpt-5.5` |
| Wire API | `responses` |

6. 确认 Base URL 末尾包含 `/v1`，然后点击 **添加**。
7. 返回供应商列表，在 Z-API 配置右侧点击 **启用**。
8. 状态显示为 **使用中** 后再启动 Codex 客户端。

## 五、检查客户端配置

如果客户端仍然使用旧地址，可以在 Codex 左下角进入 **设置**，也可以按 `Ctrl + ,` 打开设置，然后进入 **Configuration** 并打开 `config.toml`。

重点检查以下内容：

| 配置项 | 正确值 |
| --- | --- |
| `model_provider` | `Z-API` |
| `base_url` | Z-API 用户中心显示的 Codex Base URL |
| `wire_api` | `responses` |
| `api_key` | 当前 Codex 分组密钥 |
| `model` | 当前账号可用模型 |

检查完成后关闭配置文件，再彻底退出并重新启动 Codex。不要同时在 CC-Switch 和 `config.toml` 中修改同一项，以免后保存的一方覆盖另一方。

## 六、验证客户端是否可用

1. 在 Codex 客户端中新建会话。
2. 发送一个简单任务，确认能够正常返回结果。
3. 在 Z-API 控制台查看是否出现对应调用和消耗。
4. 需要切换模型时，优先选择 Z-API 用户中心明确显示为可用的模型。

如需切换中文，可在 Codex 左上角依次进入 **File**、**General**、**Language**，选择中文后重启客户端。部分版本切换失败属于客户端问题，不影响使用中文对话。

## 七、常见问题

### 提示 401 或 API Key 无效

重新复制密钥，确认没有多余空格，并检查密钥是否属于 `Codex` 分组。客户端登录密钥与 CC-Switch 中启用的密钥应保持一致。

### 提示 404 或找不到模型

检查 Base URL 是否完整并包含 `/v1`，同时将模型改为账号实际可用的模型。

### CC-Switch 显示使用中，但客户端没有变化

彻底退出 Codex 客户端和后台进程，重新在 CC-Switch 中点击 **启用**，再启动客户端。仍未生效时，检查 `.codex` 目录是否存在以及 CC-Switch 是否有写入权限。

### 客户端更新后配置被重置

重新打开 CC-Switch 并启用 Z-API 配置。重要配置建议定期备份 `.codex` 目录。
