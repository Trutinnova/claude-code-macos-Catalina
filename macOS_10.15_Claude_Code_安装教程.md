# 老 Mac（macOS 10.15）安装 Claude Code 教程

## 开始之前先搞清楚

这篇教程写给谁看的：
- 你的 Mac 是 2015~2019 年的老款，系统停在 10.15（Catalina），升级不了
- 你去网上搜安装教程，跟着敲命令结果报错 `dyld: Symbol not found`
- 你只是想装个 Claude Code 用 DeepSeek 写代码，不想折腾技术细节

这篇教程不废话，每一步都告诉你**贴什么、回车、看什么结果**。

---

## 准备三样东西

打开终端（启动台 → 其他 → 终端）。**一条一条贴**，检查你电脑上有没有这 3 样东西：

```
node --version
```

```
git --version
```

```
npm --version
```

---

**如果哪个显示 `command not found`**，说明没装。下面是对应的安装命令，缺哪个装哪个：

没 Node.js？终端贴这条：

```
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.7/install.sh | bash && source ~/.zshrc && nvm install 18 && nvm use 18 && nvm alias default 18
```

没 Git？终端贴这条：

```
xcode-select --install
```

没 npm？装 Node.js 就自带 npm，按上面第一条装就行。

---

装完后**关掉终端，重新打开**，再检查一次：

```
node --version
```

```
git --version
```

```
npm --version
```

三条都显示版本号，搞定。

---

#### 最后，搞一个 DeepSeek 账号

打开 `platform.deepseek.com`，注册，登录。进去后找"API Keys"，创建一个新 Key，复制保存好。是一串以 `sk-` 开头的字符。**这串东西就是你的钥匙，不要告诉别人。**

---

## 安装 Claude Code

以下操作全部在终端里完成。**注意：每一步都只贴一行，回车，看到结果没问题再贴下一行。** 不要一口气全贴进去。

#### 第 1 步：关掉自动更新

自动更新会把你的 Claude Code 偷偷升到新版，然后老 Mac 就打不开了。必须先把它锁死：

```
export DISABLE_AUTOUPDATER=1
```

回车后不会有任何输出，这是正常的。

#### 第 2 步：配置 npm

npm 是装 Node 时自带的，用来下载 Claude Code。默认需要管理员权限，很麻烦。下面 4 条命令让它以后不用权限也能装东西。**一条一条贴，每条回车后再贴下一条：**

```
mkdir -p ~/.npm-global
```

回车后没输出，正常。继续：

```
npm config set prefix '~/.npm-global'
```

回车后没输出，正常。继续：

```
echo 'export PATH="$HOME/.npm-global/bin:$PATH"' >> ~/.zshrc
```

回车后没输出，正常。最后：

```
source ~/.zshrc
```

#### 第 3 步：装 Claude Code

敲下面这行，回车，等十几秒：

```
npm install -g @anthropic-ai/claude-code@2.1.112
```

看到 `added 3 packages` 就说明装好了。

#### 第 4 步：验证

```
claude --version
```

屏幕上显示 `2.1.112 (Claude Code)` —— 成了。

---

## 让 Claude Code 能连上 DeepSeek

#### 第 1 步：设置钥匙

下面每条命令**一行一行贴**，把里面的 `sk-你的Key` 换成你第 3 步拿到的真实 Key：

> ⚠️ 第一条最重要，锁死自动更新。漏了它 Claude Code 会偷偷升到新版，老 Mac 就打不开了。

```
export DISABLE_AUTOUPDATER=1
```

```
export ANTHROPIC_BASE_URL=https://api.deepseek.com/anthropic
```

```
export ANTHROPIC_AUTH_TOKEN=sk-你的Key
```

```
export ANTHROPIC_MODEL=deepseek-v4-pro
```

```
export ANTHROPIC_DEFAULT_HAIKU_MODEL=deepseek-v4-flash
```

```
export ANTHROPIC_DEFAULT_SONNET_MODEL=deepseek-v4-pro
```

```
export ANTHROPIC_DEFAULT_OPUS_MODEL=deepseek-v4-pro
```

```
export CLAUDE_CODE_SUBAGENT_MODEL=deepseek-v4-flash
```

```
export API_TIMEOUT_MS=600000
```

每条回车后都没有输出，正常。

#### 第 2 步：启动

```
claude
```

等几秒，进入一个带颜色的大界面，能打字跟它对话 —— 成了。

#### 第 3 步：让设置永久生效

上面的设置关掉终端就没了。把下面 9 条命令**一条一条贴**，写入配置文件。跟前面一样，贴一行回车一下：

```
echo 'export DISABLE_AUTOUPDATER=1' >> ~/.zshrc
```

```
echo 'export ANTHROPIC_BASE_URL=https://api.deepseek.com/anthropic' >> ~/.zshrc
```

```
echo "export ANTHROPIC_AUTH_TOKEN=sk-你的Key" >> ~/.zshrc
```

```
echo 'export ANTHROPIC_MODEL=deepseek-v4-pro' >> ~/.zshrc
```

```
echo 'export ANTHROPIC_DEFAULT_HAIKU_MODEL=deepseek-v4-flash' >> ~/.zshrc
```

```
echo 'export ANTHROPIC_DEFAULT_SONNET_MODEL=deepseek-v4-pro' >> ~/.zshrc
```

```
echo 'export ANTHROPIC_DEFAULT_OPUS_MODEL=deepseek-v4-pro' >> ~/.zshrc
```

```
echo 'export CLAUDE_CODE_SUBAGENT_MODEL=deepseek-v4-flash' >> ~/.zshrc
```

```
echo 'export API_TIMEOUT_MS=600000' >> ~/.zshrc
```

然后让设置生效：

```
source ~/.zshrc
```

检查有没有写入成功：

```
cat ~/.zshrc | tail -10
```

应该看到刚刚写入的 9 行 `export` 配置。

---

## 出问题了怎么办

| 你看到的 | 什么原因 | 怎么办 |
|---------|---------|-------|
| `command not found: claude` | 终端不认识 claude | 敲 `source ~/.zshrc` 再试 |
| `EACCES` 开头的错误 | 用了 sudo | 回到"安装"部分第 2 步重新配 npm，别用 sudo |
| `dyld: Symbol not found: _ubrk_clone` | 自动更新把 2.1.112 替换成了新版 | 回到"安装"部分从头来，一定先敲 `export DISABLE_AUTOUPDATER=1` |
| 打开网页要你登录 | 走错验证流程了 | 检查环境变量设了没，关掉终端重来 |
| `API Error: 400` | 网络错误 | 检查 Key 和 Base URL 打错了没 |
| 每次都要点确认很烦 | 这是 Claude Code 的安全机制 | 可以加白名单自动放行，见下面"可选设置" |

---

## 可选：减少权限弹窗

如果觉得每次读文件都要你点确认太烦，可以加个白名单。终端敲：

```
cat > ~/.claude/settings.json << 'EOF'
{
  "permissions": {
    "allow": [
      "Read",
      "Edit",
      "Write",
      "Glob",
      "Grep"
    ]
  }
}
EOF
```

这表示读文件、改文件、写文件、搜索文件不需要每次问你。命令级别的操作还是会问，安全。不想用了就删掉这个文件。

---

## 不想要了怎么卸载

**一条一条贴**，三步删干净：

```
npm uninstall -g @anthropic-ai/claude-code
```

回车，等它跑完。然后贴下一条：

```
rm -rf ~/.claude
```

回车，没输出。最后：

```
sed -i '' '/^export ANTHROPIC_/d; /^export DISABLE_AUTOUPDATER=/d; /^export API_TIMEOUT_MS=/d; /^export CLAUDE_CODE_SUBAGENT_MODEL=/d' ~/.zshrc
```

回车，没输出。验证删干净了没：

```
claude --version
```

显示 `command not found` 就干净了。

---

## 补充信息

- 这篇教程适用的系统：macOS 10.15.0 ~ 10.15.8（Catalina 所有版本都行）
- 为什么必须装 2.1.112 这个老版本：从 2.1.113 开始 Claude Code 换了新的打包方式，老系统跑不了。2.1.112 是最后一个能跑在老 Mac 上的版本
- API Key 在 https://platform.deepseek.com 获取，按用量扣费，不写代码的时候不花钱
