+++
date = '2026-05-29T09:38:15+08:00'
title = 'TeamSpeak3'
image = "Cover/cs.webp"
categories = ["CS"]
+++

> 参考自 [Vigorous Pro](https://teamspeak.app/)

## 比奇堡的各位好

CS2 开黑想要**稳定、低延迟、对硬件友好**的语音方案?TeamSpeak3 是经典之选 —— 服务器自托管、客户端体积小、十几人同频道也几乎不掉包。这篇是我给身边人整理的一站式指南:**下载 → 连接 → 调音 → 快捷键**。

## TeamSpeak 是什么

[TeamSpeak](https://www.teamspeak.com/zh-CN/) 是一款主打游戏开黑场景的 **VoIP(网络语音)** 解决方案,相对 Discord / QQ 群语音的优势:

- **自托管**:服务器在自己手里,不依赖第三方平台
- **低延迟**:UDP 直连,中转节点少
- **低占用**:CPU / 内存占用都比 Discord 客户端轻得多
- **频道权限灵活**:多频道嵌套、独立密码、独立人数上限

## 客户端下载

### 方法一:文档中心(推荐)

直接进 [TeamSpeak3 官方下载](https://www.teamspeak.com),里面整理了 3 个国内镜像源,挑就近的那个下。

### 方法二:直链快速下载

| 用途 | 文件 | 链接 |
|------|------|------|
| Windows 64 位客户端 | `TeamSpeak3-Client-win64-3.6.2.exe` | [下载](https://files-services.teamspeak.app/releases/client/3.6.2/TeamSpeak3-Client-win64-3.6.2.exe) |
| 中文界面汉化包 | `Chinese_Translation_zh-CN.ts3_translation` | [下载](https://dl.tmspk.wiki/https:/github.com/VigorousPro/TS3-Translation_zh-CN/releases/download/snapshot/Chinese_Translation_zh-CN.ts3_translation) |
| 中文提示音包(可选) | `Chinese_Soundpack_zh-CN.ts3_soundpack` | [下载](https://teamspeak.top/download.php?file=/ts3/addons/Chinese_Soundpack_zh-CN.ts3_soundpack) |

> ⚠️ **关于中文提示音**:安装后,系统每次"用户加入 / 离开 / 移动"等事件都会用中文女声朗读(比如"用户加入了您的频道")。觉得吵就**不要勾选这一项**。

![TS3 客户端安装界面 — 推荐取消勾选提示音包](https://pub-fb5f542427074d66a4742ed7d0fcb694.r2.dev/ts3/image-1.png)

## 连接服务器

打开客户端 → 顶栏 **连接** → 在地址栏填入我的服务器地址。下面是**汉字笔画码加密**版本(避免直接挂在公网被扫)：

```
旦赁 卟透 肌匈 邗饫 妁巡 吕乙 瓜址 卟艳 邪玄 伥苍 他闱 加梧 邪泓 穴被
```

默认端口 `9987`(TS3 通用,服务器没改就不用填)。

填好之后点 **连接**,自动落到大厅,再切到相关频道就能开聊。

![TS3 连接服务器对话框](https://pub-fb5f542427074d66a4742ed7d0fcb694.r2.dev/ts3/image-2.png)

## 语音质量优化

新装客户端默认配置一般,推荐按下面这三步调一遍。

### 麦克风模式

![TS3 麦克风模式选择 — Push-to-Talk / Voice Activation / Continuous](https://pub-fb5f542427074d66a4742ed7d0fcb694.r2.dev/ts3/image-3.png)

### 选择音频输入设备

下拉选自己实际接的麦克风(USB / 蓝牙耳机 / 板载耳麦 都在这选)。

![TS3 音频输入设备选择](https://pub-fb5f542427074d66a4742ed7d0fcb694.r2.dev/ts3/image-4.png)

### 调整他人音量

频道里每个人都可以单独调音量,声音忽大忽小的时候用得上。

![TS3 单人音量调节面板](https://pub-fb5f542427074d66a4742ed7d0fcb694.r2.dev/ts3/image6.png)

## 常用快捷键

> 💡 **管理员模式提示**:如果全局快捷键在游戏窗口里**不生效**(比如打 CS2 时按键没用),请用 **管理员身份**重新打开 TeamSpeak3 — 否则游戏窗口会"抢走"快捷键。

![TS3 快捷键列表截图](https://pub-fb5f542427074d66a4742ed7d0fcb694.r2.dev/ts3/image-5.png)
