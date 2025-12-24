# 🚀 Koyeb Account Keep-Alive (Koyeb 账号保活脚本)

利用 GitHub Actions 自动周期性运行 Koyeb CLI，防止 Koyeb 免费账号因长期不活跃而被停用/删除。

![GitHub Actions Workflow Status](https://img.shields.io/github/actions/workflow/status/你的用户名/你的仓库名/main.yml?label=Keep-Alive)

## ✨ 功能特性

*   **自动运行**: 基于 Cron 表达式，每 **6天** 自动运行一次，符合 Koyeb 的活跃度要求。
*   **多账号支持**: 支持同时配置多个 Koyeb 账号 Token，批量保活。
*   **智能更新**: 脚本会自动从 GitHub 获取并安装最新版的 `koyeb-cli`，无需手动维护。
*   **Telegram 通知**: 执行结果（成功或失败）会通过 Telegram Bot 推送给您。
*   **隐私安全**: 使用 GitHub Secrets 存储敏感信息，日志中自动隐藏关键 Token。

## ⚙️ 配置方法

### 1. 获取 Koyeb API Token
1. 登录 [Koyeb 控制台](https://app.koyeb.com/)。
2. 进入 **User Settings** -> **API**。
3. 点击 **Create API Access Token**，创建一个新的 Token 并复制保存。

### 2. 获取 Telegram 配置 (可选)
如果不配置 Telegram，脚本将只会在 GitHub Actions 日志中输出结果。
1. **Token**: 在 Telegram 中找 [@BotFather](https://t.me/BotFather) 创建机器人，获取 API Token。
2. **Chat ID**: 在 Telegram 中找 [@userinfobot](https://t.me/userinfobot) 发送任意消息，获取您的数字 ID。

### 3. 设置 GitHub Secrets
在您的 GitHub 仓库中，点击 **Settings** -> **Secrets and variables** -> **Actions** -> **New repository secret**，添加以下变量：

| Secret 名称 | 必填 | 说明 |
| :--- | :---: | :--- |
| `KOYEB_TOKENS` | ✅ | **Koyeb 的 API Token**。<br>如果有多个账号，请**一行一个** Token (回车换行)。 |
| `TG_BOT_TOKEN` | ❌ | Telegram 机器人的 Token (如果不填则不发送通知)。 |
| `TG_CHAT_ID` | ❌ | 接收通知的 Telegram 用户 ID (如果不填则不发送通知)。 |

## 🕒 运行机制
定时触发: 默认为 UTC 时间每隔 6 天的 00:30 (北京时间 08:30) 运行一次。
Cron 表达式: 30 0 */6 * *
手动触发: 您可以在 GitHub 仓库的 Actions 页面，选择 "Koyeb Keep Alive" -> Run workflow 手动触发测试。

## 📝 脚本逻辑说明
环境准备: 自动下载最新版 Linux AMD64 架构的 Koyeb CLI 工具。
读取配置: 逐行读取 KOYEB_TOKENS 中的 Token。
执行命令: 使用 CLI 执行 koyeb organizations list 命令。
该命令会触发 API 调用，证明账号活跃。
脚本包含错误捕获机制，单个 Token 失效不会中断整个流程。
结果解析: 解析 JSON 返回值，提取账号名称。
发送通知: 将运行状态推送到 Telegram。

## ⚠️ 免责声明
本脚本仅用于辅助保持账号活跃，不保证 100% 防止账号被封禁。
请勿滥用 GitHub Actions 资源。
Koyeb 的服务条款可能会随时更新，请自行关注官方政策。
License: MIT
code
Code

## 💡 小提示
如果是你自己使用，记得将文档开头的 `你的用户名/你的仓库名` 替换成你真实的 GitHub 路径，这样徽章（Badge）才能正常显示构建状态。
