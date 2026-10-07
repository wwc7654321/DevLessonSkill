# 项目发布/成果物经验

适用：把开发好的项目整理成干净、可发布的仓库（源码 + 净化后的 config + 构建产物 zip），准备推到 GitHub / 上传 GitHub Releases。与具体语言、业务无关，是纯发布流程的通用点。

## 一、发布产物要按「目录」git-ignore，不能只靠按扩展名忽略

`*.exe` / `*.dll` / `*.pdb` 只忽略二进制扩展名，但发布文件夹里通常还有 `config.json`、`README.md`、`*.zip` 这类非二进制文件，它们不被这些规则挡掉；此时 `git add -A` 会把整个发布文件夹和 zip 以未跟踪形式一并提交进 git 库（二进制入库、又大又脏）。

把发布文件夹的目录名（如 `GameScreenTranslator/`）和 `*.zip` 也写进 `.gitignore`，按目录整体忽略，而不是指望扩展名规则兜住。

## 二、发布用的 config 要单独净化，别复制开发目录的私密 config

开发/工作目录的 `config.json` 是真实可跑的（真实 `EndpointUrl`、`Model`、`ApiKey`，甚至含敏感的模型名），发布仓库里跟踪的 `config.json` 应把这些字段置空当「模板」，让用户自己填。准备发布时复制**净化版**，不是整份拷贝工作 config——否则密钥和私密模型名会随发布物一起泄漏。

## 三、Windows 的 Git Bash 没 zip/7z，打 zip 用 PowerShell Compress-Archive

Git Bash 默认不带 `zip` / `7z`，按 Linux 习惯 `zip -r x.zip dir` 会 `command not found`。改用：

```powershell
Compress-Archive -Path <目录> -DestinationPath <name.zip> -Force
```

`-Force` 覆盖旧 zip；传目录时目录名会成为 zip 顶层条目。
