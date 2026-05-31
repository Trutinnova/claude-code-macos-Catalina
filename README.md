# claude-code-macos-10.15.8（Catalina）
# 让 macOS 10.15 老 Mac 也能装上 Claude Code，支持 DeepSeek 和 MiMo
教程只适用 macOS 10.15（Catalina

##为什么锁定 v2.1.112 版本？
* 官方限制：Claude Code 从 v2.1.113 开始进行了重大架构重构，其 npm 包内部换成了**原生二进制文件（Native Binaries），直接要求系统必须是 macOS 13.0+ (Ventura) 及以上。
* 终极选择：v2.1.112是官方最后一个纯 JavaScript（Pure JS）编写的版本。因此，对于 macOS 10.15 而言，通过 npm 安装 `v2.1.112` 是最完美的绝唱版本。

# 为什么选择“本地安装”，而不是“Docker 安装”？
Docker 的局限： 虽然在 macOS 10.15 上可以通过安装旧版 Docker (如 Docker 4.15) 来运行一些容器，但 Claude Code 在 Docker 容器内会受到极大限制。它无法读取、控制和修改容器外部（你 Mac 本地）的文件和系统环境。
本地安装的优势：只有将 Claude Code (v2.1.112) 直接安装在 Mac 本地系统上，它才能真正发挥出完全体实力，自由地帮你操作本地代码库、执行本地终端命令。

## 选一篇开始
  
  | 你用的是 | 点这里 |
  |---------|------|
  | DeepSeek | [DeepSeek版教程](https://github.com/Trutinnova/claude-code-macos-10.15/blob/main/macOS_10.15_Claude_Code_安装教程.md) |
  | 小米 MiMo | [小米 MiMo 版教程](https://github.com/Trutinnova/claude-code-macos-10.15/blob/main/macOS_10.15_Claude_Code_小米MiMo版_安装教程.md) |

  看不懂选哪个？你从哪个平台拿的 Key，就选哪篇
  
  
  
