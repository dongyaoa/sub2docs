---
title: codex-desktop
category: kehuduan
order: 20
description: 使用 Z-API API Key 登录并配置 Codex 桌面端
updated: 2026-08-16T12:11
---

## 下载链接

- [Windows 官方下载](https://get.microsoft.com/installer/download/9PLM9XGG6VKS?cid=website_cta_psi)
- [OpenAI.Codex_26.422.3464.0_x64__2p2nqsd0c76g0.Msix](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/preview/KjsJbxXqBoPVj1xu6aYcCedUnPg?mount_point=docx_file&preview_type=16)
- [Codex.dmg](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/preview/H2rWbFPiGosnwwxhumDch8m8nLb?mount_point=docx_file&preview_type=16)

![Codex 桌面端下载示例](https://img.imgdd.com/5a27141c-ef8e-44f4-8129-e41e6806a604.png)

## 一、登录

1. 如果已经登录个人账号，先点击左下角的 **退出登录**。

![退出个人账号](https://img.imgdd.com/e852cf34-54c9-44df-ad88-e80280bf08b1.png)

2. 点击下方 **API Key** 登录方式，输入你的 Z-API API Key。API Key 通常以 `sk-` 开头，复制时注意不要带入空格。

| API Key 登录入口 | 输入 API Key |
| --- | --- |
| ![API Key 登录入口](https://img.imgdd.com/2e3cf5b1-3400-451b-9577-a0c322d006c6.png) | ![输入 API Key](https://img.imgdd.com/27bfc757-c241-47cf-8a18-69e2cda5673b.png) |

## 二、设置 API 站点

正常使用前需要设置 `config.toml` 文件。下面两种方式任选一种打开配置文件即可。

### 方法一：从设置入口打开

点击左下角 **设置**，也可以使用 `Ctrl + ,` 直接打开设置。

![打开 Codex 设置](https://img.imgdd.com/8328dd84-2b0f-4cfc-8845-3c5d0b9fedd4.png)

![打开配置入口](https://img.imgdd.com/9f8d4708-fb9a-4a89-afa9-9862eb8f71e5.png)

### 方法二：从菜单打开

依次点击 **File** -> **Settings...** -> **Configuration** -> **Open config.toml**。

![Codex 菜单设置入口](https://img.imgdd.com/4e3eb461-45d7-4c9a-be86-e886157170dd.png)

![打开 config.toml](https://img.imgdd.com/2cc1a341-0c6f-46d3-966d-69b41755064e.png)

### 配置 config.toml

把下面内容复制到 `config.toml` 中。重点是 `base_url` 要填写 Z-API 用户中心「API 密钥」页面显示的 Base URL。配置完成后退出 Codex，再重新启动应用。

```toml\nmodel_provider = "Z-API"
model = "gpt-5.5" # 换成你在 Z-API 上实际可用的模型
model_reasoning_effort = "medium"

# Windows 沙盒配置
[windows]
sandbox = "elevated"

# 项目信任配置，请根据实际项目路径修改
[projects.'C:\Users\你的Windows用户名\Documents\codex\你的项目']
trust_level = "trusted"

# Z-API 中转提供商配置
[model_providers.Z-API]
name = "codex"
base_url = "https://api.zicc.cc/v1"
wire_api = "responses"
api_key = "sk-你的Z-API密钥"\n```

如果用户中心显示的 Base URL 与示例不同，请以用户中心为准。

## 三、开始使用

直接输入你的任务即可。为了避免浪费额度，建议根据任务需求选择合适的模型和思考程度。

![Codex 桌面端使用示例](https://img.imgdd.com/09a86d8f-814e-4717-83ab-11d1d55bc386.png)

## 切换中文

点击左上角 **File** -> **General** -> **Language**，选择中文后重启桌面端即可。

如果切换失败，通常是当前官方版本问题，不影响模型用中文回复。

![Codex 切换中文](https://img.imgdd.com/8b6be2c4-a544-4854-9d94-d85e043dbd5f.png)

## 查询消耗

可在 [Z-API 控制台](https://api.zicc.cc) 查询消耗。
