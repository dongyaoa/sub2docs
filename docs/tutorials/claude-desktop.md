---
title: Claude Desktop桌面端配置
category: kehuduan
order: 100
description: ''
updated: 2026-08-12T23:50
---

## 软件下载

点击 [Claude Desktop下载链接](https://claude.com/download) ，进入下载页面

![](/images/uploads/QQ20260812-235130.jpg)

在如上图 `Desktop` 一块，根据自己的系统，下载对应的安装包

软件安装

Windows系统下软件安装需要请求Anthropic官方，需要你用梯子挂 **全局服务（TUN模式）** ，或是用命令行来运行安装程序，使其强制走代理，否则会出现以下报错

![](/images/uploads/QQ20260813-004212.jpg)

如果出现以上报错无法安装，请在 Claude Desktop安装程序所在目录运行 `cmd` 命令行

确认你当前使用梯子的端口号，比如我使用的是 `Clash Verge` ，则端口号为 `7897`

![](/images/uploads/QQ20260813-004252.jpg)

在命令行中分别输入以下命令，运行安装程序，此时能够正常安装

```plain\nset HTTP_PROXY=http://127.0.0.1:7897
set HTTPS_PROXY=http://127.0.0.1:7897
"Claude Setup.exe"\n```

![](/images/uploads/QQ20260813-004336.jpg)

正常安装

![](/images/uploads/QQ20260812-235723.jpg)

## 绕过登录并配置第三方接口

1.打开软件进入登录界面

2. 启用 Developer Mode
3. 启动客户端并完成登录
4. 点击左上角菜单 → **Settings**
5. 进入 **Help** → **Troubleshooting**
6. 勾选 **Enable Developer Mode**
7. 按提示重启软件

重启后，左上角菜单会新增 **Developer** 项。点击 **Developer** → **Configure third-party inference** 进入配置页面：

![](/images/uploads/QQ20260813-000958.jpg)

在Gateway base URL填入 `https://api.zicc.cc`

Gateway API key请填入生成的 CC分组 的APIKEY

打开最下方 `Skip login-mode chooser` 选项

地址必须是 HTTPS 域名，不接受 IP、端口或 HTTP 协议。

![](/images/uploads/QQ20260813-003145.jpg)

点击右下角 `Apply locally` 按钮使配置生效

进行愉快的对话吧\~

![](/images/uploads/QQ20260813-004004.jpg)
