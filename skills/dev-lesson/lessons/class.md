# 任务分类词典

## 本地小模型翻译（LocalModelTranslation）
- 边界：调用本地大模型（LM Studio / Ollama 等 OpenAI 兼容接口）批量翻译文本，需工程化保障（校验、断点续传、批量）。游戏文本、小说、字幕皆可。
- 经验文件：`LocalModelTranslation.md`

## Godot 游戏解包/改翻译/打包（GodotGameMod）
- 边界：GDRE 解包 Godot PCK/exe，改翻译（CSV→`.translation`），重新打包。
- 跨类复用：翻译文本复用「本地小模型翻译」经验。
- 经验文件：`GodotGameMod.md`

## .NET WinForms 桌面工程（DotnetWinForms）
- 边界：用 C#/.NET 写 WinForms 桌面/托盘/窗体工具，含后台循环 + 界面刷新。
- 经验文件：`DotnetWinForms.md`
