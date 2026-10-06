# Godot 游戏解包/改翻译/打包 经验

适用：改 Godot 游戏（PCK/exe）的翻译文本，并重新打包。翻译文本本身（批量、校验、模型调用）见 [[LocalModelTranslation]]。

## 零、前提工具（都需要本地下载，缺一不可）

- **Godot Engine**（与游戏同版本，console/无头版即可，下载解压无需装 IDE）：重新生成 `.translation` 的唯一可靠途径（见第三节），这一步必须本地有 Godot 才能执行，普通用户若没装 Godot 就做不了。
- **先定游戏真实版本、再下 Godot**：exe 内嵌 `Godot Engine v4.x.y` 字符串、`project.godot` 的 `config/features=PackedStringArray("4.x",...)`、GDRE 输出的 `Detected Engine Version` 三处可查。github.com 超时时用 GitHub API 直链下载：`curl api.github.com/repos/godotengine/godot/releases/tags/<tag>` 拿资产 id，再 `curl -H "Accept: application/octet-stream" .../releases/assets/<id>`（重定向到可达的 objects.githubusercontent.com）；tuxfamily 旧镜像已空。
- **GDRE Tools**（Godot RE Tools）：解包/打包 PCK 的第三方工具，同样要下载，但比 Godot 简单（单 exe、无配置）。

如果当前目录没看到，用户也没提。则询问用户本地是否有，没有的话建议下载。

## 一、认识 Godot 4 翻译体系（先别急着动手）

- 源是 CSV（`key,ja,en,ko,zh_Hans,zh_Hant` 表头，UTF-8 带 BOM，LF），但**运行时游戏加载的不是 CSV，而是它导入生成的 `.translation` 二进制**（`OptimizedTranslation`/RSRC）。
- 每个 CSV 旁有 `.csv.import` 文件（`importer="csv_translation"`），`dest_files` 列出它生成的各语言 `.translation`；`project.godot` 里 `locale/translations` 是运行时加载清单。
- 结论：改翻译 = **先 `--recover` 拿到 CSV**（PCK 里通常根本没有 CSV，只有 `.csv.import` + `.translation` 二进制，见第二节）→ 改 CSV → Godot 导入重新生成 `.translation` → 只替换 `.translation` 打回包。
- **ui_text 与 dialogue 键体系不同、对版本错位敏感度不对称**：ui_text 的 key 列==ja 列（源键就是日文文本，PCK 里无源文件、恢复靠 `.translation` 哈希配对），版本一错就最先错位；dialogue 的 key 是 `[ID:xxx]`（如 `C29301_5AC3B9`）、有 `.dialogue` 源文件可恢复，版本不对也基本能对上。**排查版本问题先看 ui_text。**

### 对话里可能有 `[[a|b|c]]` 随机变体（Dialogue Manager 插件语法）

- 小模型翻译**长变体串容易截断丢变体**（该有 10 个只留 3 个，或丢闭合 `]]`）；光查假名残留查不出这种结构损坏，必须显式校验：变体数（`|` 个数）与原文一致 + 有闭合 `]]`。

## 二、解包（GDRE）

- **关键区分 `--extract`（原样抽取，不重建）和 `--recover`（完整恢复，重建 CSV）**：
  - `gdre_tools.exe --headless --extract=<game.pck> --output=<目录>` → raw 抽取，PCK 里有什么抽什么。PCK 里**没有 `.csv`**（只有 `.csv.import` + `.translation` 二进制），所以它**拿不到 CSV**。
  - `gdre_tools.exe --headless --recover=<game.pck> --output=<目录>` → 完整工程恢复，会从 `.translation` + `.dialogue` 的 `[ID:xxx]` 键**重建出 `.csv`**，并反编译 `.gdc→.gd`、资源转文本。**改翻译用这个。**
  - 注意：GDRE **GUI 的 "extract" 按钮实际就是 `--recover`**（GUI 里能直接看到 csv），别被 CLI `--extract` 的名字误导。
- `--list-files=<pck>` 查 PCK 内部路径（`res://...`），打补丁前用它确认目标路径；顺带确认 PCK 里到底有没有 `.csv`（通常只有 `.csv.import`）。

## 三、重新生成 .translation（关键：Godot 导入，别 hack 二进制）

- 把改好的 CSV 覆盖到解包项目对应位置，用**同版本** Godot 无头导入：
  `Godot_*.exe --headless --import --path <项目目录>`
- **版本必须精确一致，不是任意 4.x 都行**：`.translation` 偏移 `0x10` 是格式版本字节（4.4=`04`、4.5=`05`），它决定哈希桶布局；用错版本导入，游戏按错规则解析 → 对白错位（实测症状：ui_text `突風`→`异物混入`）。
- 只重导有变化的资源（CSV→translation）；其余资源报错（缺 Spine 插件、SVG、脚本解析错误等）**不影响翻译导入**，看 exit code + `.translation` 时间戳/内容即可。
- **别 hack 二进制（读和写都算）**：`.translation`（OptimizedTranslation）里只存 hash + 译文、**不存源键**（源键是单向哈希，反推不出），字符串数组按 hash 桶序排而非 CSV 行序（跨语言 `ja[i]↔zh[i]` 对不上）。所以**读原文/旧译用 `--recover` 拿 CSV**，别用 Python/`--bin-to-txt` 去解二进制。
- **`--recover` 反推遇重复源键会伪错配**（ui_text 常有同一 ja 出现多次）：会把「与重复键相邻的键」指到别人译文（实测 `品質改善`↔`ふれあい`、`集荷レール`↔`朝`）。这是 GDRE 反推伪错、不是游戏 bug，**别据此判断版本不符或去「修」**。权威验证对齐：写最小 Godot 工程 `load("x.zh_Hans.translation")` + `TranslationServer.add_translation` + `set_locale` + `TranslationServer.translate(key)` 直接查，与游戏运行时同一套哈希，结果才对。
- **实测 `--patch-translations` 对已有翻译不可靠**：它是「追加新条目」而非「按 key 替换」——给 1 行 CSV 只追加 1 条、旧串仍在；给全量 CSV 也只追加不替换（推测是 GDRE 所用 Godot 版本与游戏版本的 key 哈希不一致，对不上就当新键追加）。**改已有翻译别用它**，走上面 Godot 导入。`--bin-to-txt`/`--txt-to-bin` 往返也有损（`bucket_table` 里的字符串长度不随改动重算），同样别用来改。

## 四、重新打包（--pck-patch 优于整体重打包）

- 用 `--pck-patch` **只替换**改动文件进原 PCK，比 `--pck-create` 整体重打包安全（其余几百文件原样保留）：
  `gdre_tools.exe --headless --pck-patch=<原pck> --output=<新pck> --patch-file=<本地文件>=res://<内部路径> ...`（`--patch-file` 可重复）
- 内部路径用 `--list-files` 查到的 `res://` 路径。
- 验证：`--extract` 抽一个新 pck 文件，md5 与本地重新生成的文件比对一致；`--list-files` 确认文件数不变、`Verified N files, no errors`。
- 改前先 `cp 原pck 原pck.bak` 备份。
