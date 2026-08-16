---
title: Codex客户端切换中转站同步会话教程
category: kehuduan
order: 12
description: 通过统一 model_provider 标识，恢复切换中转站后不显示的 Codex 历史会话
updated: 2026-08-16T15:13
---

从官方服务或其他中转站切换到 Z-API 后，如果历史会话突然不显示，通常不是会话被删除了，而是新旧配置使用了不同的 `model_provider` 标识。

> **最简单的处理方法：** 找到原来能显示历史会话时使用的提供商标识，然后让 Z-API 配置中的 `model_provider` 和 `[model_providers.标识]` 使用同一个旧标识。

## 一、为什么切换后看不到会话

Codex 会在会话中记录创建时使用的 `model_provider` 标识。切换中转站后，如果原来的标识是 `custom`，而一键导入的新配置变成了 `OpenAI`，旧会话可能不会出现在当前列表中。

这里说的“标识”就是引号中的文字。例如：

- `model_provider = "OpenAI"` 的标识是 `OpenAI`
- `model_provider = "custom"` 的标识是 `custom`

标识区分大小写，必须完全一致。

## 二、找到原来会话使用的标识

1. 在 CC-Switch 中找到切换前使用的配置，或者打开之前备份的 `config.toml`。
2. 找到 `model_provider = "xxx"` 这一行。
3. 记住引号中的内容，这就是原来会话使用的标识。

例如原来的配置是 `model_provider = "OpenAI"`，需要记住的标识就是 `OpenAI`。

## 三、修改 Z-API 配置

在 CC-Switch 中打开当前 Z-API 的 Codex 配置。一键导入后的配置通常使用：

- `model_provider = "OpenAI"`
- `[model_providers.OpenAI]`

假设原来的会话标识是 `custom`，只需要将这两处改为：

- `model_provider = "custom"`
- `[model_providers.custom]`

只修改这两个标识，不要修改 Z-API 的 Base URL、API Key 和模型。Base URL 继续使用用户中心显示的地址，不要额外添加 `/v1`。

> **注意：** `model_provider` 引号中的标识，必须和 `[model_providers.标识]` 后面的标识完全相同。改一处、不改另一处会导致供应商配置无法加载。

## 四、保存并重新打开 Codex

1. 保存 CC-Switch 中的配置并点击 **启用**。
2. 完全退出 Codex 客户端，包括后台进程。
3. 重新启动 Codex，历史会话通常就会重新显示。

如果原来的标识本来就是 `custom`，则不需要修改标识，只需确认两处都写成 `custom` 后重新启动客户端。

## 仍然不显示怎么办

如果按上面的步骤操作后仍然不显示，再检查切换前后是否使用了同一个 `.codex` 目录。只有在目录被更换、原会话文件被移动或删除时，才需要恢复备份；正常切换中转站不需要复制会话文件或修改数据库。
