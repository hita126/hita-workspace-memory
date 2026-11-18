# legacy：v1（已停用）

hita-workspace-memory 之前的版本，两个 OpenClaw skill（Node.js，JSON 存储）：

- `hita-mind-module/`：管记什么，8 类记忆（决策、偏好、模式、因果、联系、反馈、项目、日常）。
- `hita-knowledge-manager/`：管记多少，热 / 温 / 冷三层（30 天不用降温层，90 天不用进冷层，只留索引）。
- 原来的说明见 `README-v1.md`，原来的总 skill 见 `SKILL.md`。

停用原因：项目一多，所有记忆挤在同一个存储里，不同项目的内容会串。v2 改成按项目路由，见仓库根目录的 README。
