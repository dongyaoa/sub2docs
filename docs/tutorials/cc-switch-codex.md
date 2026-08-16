---
title: CC-Switch接入Codex客户端教程
category: kehuduan
order: 10
description: 使用 sub2api 一键导入 CC-Switch，并在 Codex 官方客户端中生效
updated: 2026-08-16T14:23
---

本教程面向 Codex 官方客户端用户。sub2api 的一键导入会把供应商配置写入 CC-Switch，再由 CC-Switch 更新 Codex 的本地配置。完成切换后必须彻底退出并重新启动客户端。

> **重要：** 一键导入生成的 `model_provider` 默认通常是 `OpenAI` 或 `customI`，不是 `Z-API`。Base URL 按用户中心显示的地址填写，不需要额外添加 `/v1`。

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

1. 打开 sub2api 用户中心，进入 **API 密钥** 页面。
2. 创建或选择 **Codex** 分组的密钥。
3. 复制 API Key，并记录页面显示的 Base URL 和可用模型。
4. 如果 Codex 已登录个人账号，先在左下角退出登录。
5. 在登录界面选择 **API Key**，粘贴 sub2api 密钥并完成登录。

API Key 通常以 `sk-` 开头。复制时不要带入前后空格，也不要把真实密钥写入公开文档或发送给他人。

## 三、安装 CC-Switch

从 [CC-Switch GitHub Releases](https://github.com/farion1231/cc-switch/releases/latest) 下载最新稳定版。

| 系统 | 安装方式 |
| --- | --- |
| Windows | 下载普通的 `.msi` 安装包并按提示安装。 |
| macOS | 依次执行 `brew tap farion1231/ccswitch` 和 `brew install --cask cc-switch`。 |

安装后启动 CC-Switch。如果软件提示导入已有 Codex 配置，建议先选择备份，再继续操作。

## 四、使用 sub2api 一键导入

1. 完全退出 Codex 客户端，包括仍在后台运行的进程。
2. 在 sub2api 用户中心找到 CC-Switch 的 **一键导入** 按钮。
3. 浏览器询问是否打开 CC-Switch 时，选择允许。
4. 在 CC-Switch 中确认导入的供应商、API Key、Base URL 和默认模型。
5. 返回 Codex 供应商列表，在刚导入的配置右侧点击 **启用**。
6. 状态显示为 **使用中** 后再启动 Codex 客户端。

一键导入后的关键配置如下：

| 配置项 | 正确说明 |
| --- | --- |
| `model_provider` | 默认通常为 `OpenAI` 或 `customI`，以实际导入结果为准 |
| Base URL | 使用用户中心显示的完整地址，不额外添加 `/v1` |
| API Key | Codex 分组的 API Key |
| 默认模型 | 选择账号实际可用的模型 |
| `wire_api` | `responses` |

`model_provider` 必须与配置段名称完全一致。例如值为 `OpenAI` 时，应对应 `model_providers.OpenAI`；值为 `customI` 时，应对应 `model_providers.customI`。不要只修改其中一处。

## 五、检查客户端配置

如果客户端仍然使用旧地址，可以在 Codex 左下角进入 **设置**，也可以按 `Ctrl + ,` 打开设置，然后进入 **Configuration** 并打开 `config.toml`。

重点检查以下内容：

| 配置项 | 正确值 |
| --- | --- |
| `model_provider` | `OpenAI` 或 `customI`，与一键导入结果一致 |
| 供应商配置段 | `model_providers.OpenAI` 或 `model_providers.customI` |
| `base_url` | sub2api 用户中心显示的地址，不添加 `/v1` |
| `wire_api` | `responses` |
| `api_key` | 当前 Codex 分组密钥 |
| `model` | 当前账号可用模型 |

检查完成后关闭配置文件，再彻底退出并重新启动 Codex。不要同时在 CC-Switch 和 `config.toml` 中修改同一项，以免后保存的一方覆盖另一方。

## 六、验证客户端是否可用

1. 在 Codex 客户端中新建会话。
2. 发送一个简单任务，确认能够正常返回结果。
3. 在 sub2api 用户中心查看是否出现对应调用和消耗。
4. 需要切换模型时，优先选择用户中心明确显示为可用的模型。

如需切换中文，可在 Codex 左上角依次进入 **File**、**General**、**Language**，选择中文后重启客户端。部分版本切换失败属于客户端问题，不影响使用中文对话。

## 七、常见问题

### 提示 401 或 API Key 无效

重新复制密钥，确认没有多余空格，并检查密钥是否属于 `Codex` 分组。客户端登录密钥与 CC-Switch 中启用的密钥应保持一致。

### 提示 404 或找不到模型

确认 Base URL 与用户中心显示的一致，并移除手动添加的 `/v1`。同时将模型改为账号实际可用的模型。

### 修改后提示找不到供应商

检查 `model_provider` 与供应商配置段名称是否一致。`OpenAI` 和 `customI` 区分大小写，不能混用。

### CC-Switch 显示使用中，但客户端没有变化

彻底退出 Codex 客户端和后台进程，重新在 CC-Switch 中点击 **启用**，再启动客户端。仍未生效时，检查 `.codex` 目录是否存在以及 CC-Switch 是否有写入权限。

### 客户端更新后配置被重置

重新打开 CC-Switch 并启用 sub2api 配置。重要配置建议定期备份 `.codex` 目录。
