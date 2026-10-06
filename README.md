# DevLessonSkill

一个 Claude Code 技能：**开发经验的总结与复用**。  
做程序性任务（写脚本、开发小工具、做游戏 mod、开发 GUI 工具等）时，任务开始前先查历史踩坑经验，任务完成后把可复用经验沉淀进经验库，减少重复试错。

**定位**：不是通用记忆库，也不是团队 wiki。它是中低频开发任务的“踩坑备忘录”——记的准、找得到、不啰嗦。  
宽入严出：总结时不设门槛，入库前独立质检，只留模型靠已有知识猜不到、或会猜错的坑。

## 它不做什么

- 不对聊天/问答触发，只服务程序性开发任务。
- 不做自动检索、知识图谱、分层存储、自演化；低频场景下这些是纯开销。
- 不把一次性工程细节写进经验库，只进 `project_summary/` 写完结报告。
- 不强行总结通用常识、工具基本用法、官方文档已明确警告的内容。

## 和通用 memory / skill 项目的区别

通用方案多面向高频、通用、长期知识库，强调自动检索、分层存储和自演化。  
DevLessonSkill 面向中低频、一次性/工具开发任务，只做分类路由 + 短经验文件 + 独立质检，优先降低上下文成本和人工维护成本。  
目标不是建大而全的 wiki，而是“下次别再踩同一个坑”。

## 一键安装

需已安装 [Claude Code](https://claude.com/claude-code)。

**方式一（新版 Claude Code，一条命令）**：

```
/plugin install dev-lesson --marketplace wwc7654321/DevLessonSkill
```

**方式二（两步，兼容旧版）**：

```bash
claude plugin marketplace add wwc7654321/DevLessonSkill
claude plugin install dev-lesson@dev-lesson-marketplace
```

装完重启 Claude Code（或在会话里 `/reload-plugins`）即可生效。

## 使用方式

1. **自动调用（推荐）**：写进 `CLAUDE.md`（用户级或项目级均可），开发任务开始前读经验、结束后总结，无需手动说话：

```markdown
仅对程序性任务（写脚本、开发工具、做 mod 等）生效，聊天/问答等其它对话不触发。触发时按 dev-lesson 技能执行：任务开始前先按任务类别读经验，任务完成后总结经验。
```

2. **手动调用**（同一个命令 `/dev-lesson`，分两个场景）：

| 场景 | 命令 | 作用 |
|------|------|------|
| 开发任务开始前 | `/dev-lesson` | 读对应分类经验，防重复踩坑 |
| 开发任务结束后 | `/dev-lesson` | 提炼可复用经验、质检后写入经验库 |

## 结构

```
skills/dev-lesson/
  SKILL.md              # 入口：经验分类、复用、总结、质检流程
  lessons/
    class.md            # 任务分类路由词典
    *.md                # 各类可复用经验（本地模型翻译 / Godot mod / WinForms…）
  project_summary/      # 一次性工程的完结报告（不进经验库）
  总结经验.md / 经验质检.md / 工作目录.md
```

- **可复用经验** → 进 `lessons/`，由 `class.md` 路由。
- **一次性工程**（做完即用、不会再写二代）→ 只在 `project_summary/` 写完结报告，不污染经验库。

## 许可

[MIT](LICENSE)
