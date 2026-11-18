# hita-workspace-memory

> 让 AI agent 记住你：一个项目，一份索引；开工前，先读索引；新的记忆，自动写进对应文件。

A file-based project memory skill for **Claude Code** and **Codex**. The skill holds no facts; it routes the agent: read the right project files before working, write each new memory to the right file when you say 「记一下」.

## 它是怎么来的

| 版本 | 做法 | 问题 |
|---|---|---|
| v0 | 直接用 OpenClaw，靠对话上下文 | 换个对话就忘，写在记忆里的教训也不去查 |
| v1（`legacy/`） | 两个 skill：**hita-mind-module** 管记什么（8 类抽屉），**hita-knowledge-manager** 管记多少（热 / 温 / 冷三层） | 项目一多，所有东西挤在同一个地方，写项目 A 的周报会混进项目 B 的数字 |
| **v2（现在）** | **hita-workspace-memory**：合成一个 skill，按项目路由 | — |

## 它像一栋楼

```
工作区/
├── 工作区索引.md          ← 大厅：有哪些项目、触发词、去哪找
├── memory/                ← 公共区：跨项目都成立的东西
│   ├── 通用工作流程规则.md
│   ├── 用户长期偏好.md
│   ├── 长期决策.md
│   └── 纠错反馈.md        ← 被纠正过的错误，下次开工先读
└── projects/
    └── 项目A/             ← 一个项目，一个房间
        ├── 项目卡.md      ← 这个项目是什么、开工按什么顺序读
        ├── 项目记忆.md    ← 稳定事实、业务背景、已确认的误报
        ├── 周期摘要.md    ← 每期的数字和结论
        ├── 报告规则.md    ← 交付格式、被改过的措辞
        └── 文件索引.md    ← 原始资料在哪（只记路径，不复制原件）
```

- **读**：先问是哪个项目 → 读 `项目卡.md` → 按需要读项目记忆、周期摘要、报告规则；用记忆之前先核对它是不是过期了。
- **写**：说一句「记一下」，agent 自己判断放哪：项目的事放进项目房间，通用经验放公共区，被纠正过的写进纠错反馈；写之前先查重，有就合并。
- **安全**：密码、密钥、Token 只记位置，不记内容。

## 安装

1. 把 `hita-workspace-memory/` 复制到 skill 目录：
   - Claude Code：`~/.claude/skills/hita-workspace-memory/`
   - Codex：`~/.codex/skills/hita-workspace-memory/`
2. 把 `hita-workspace-memory/templates/workspace/` 复制成你的工作区，默认位置是 `~/.codex/hita_workspace`（两个 agent 可以共用同一个工作区）。放在别处的话，改 `SKILL.md` 里的那一行路径。
3. 在 `工作区索引.md` 里登记你的项目，照 `projects/示例项目/` 建项目文件夹。

## 怎么用

```text
帮我写项目 A 这周的周报              → 先读索引和项目 A 的项目卡，再按报告规则写
记一下：项目 A 的报告不要用「高危」这个词  → 写进 projects/项目A/报告规则.md
记一下：以后交付都先给结论            → 写进 memory/用户长期偏好.md
你上次把 B 的数字写进 A 了，记一下    → 写进 memory/纠错反馈.md
```

## 和 v1 的区别

| | v1 两个 skill | v2 hita-workspace-memory |
|---|---|---|
| 存储 | 一个 JSON 文件 | 按项目分的 Markdown 文件，人也能直接读、改 |
| 分类 | 8 类 + 热温冷三层，平铺 | 按项目路由，项目内五个文件 |
| 读取 | 需要时再查 | 开工前按顺序读，读之前核对是否过期 |
| 写入 | 手动调用 `node index.js add` | 说「记一下」自动归位，先查重 |
| 运行 | Node.js 脚本 | 纯 skill，无依赖 |

v1 的代码保留在 [`legacy/`](legacy/)。

## License

MIT © hita
