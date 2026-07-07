# 响应智能体（Response Agent）说明

本文档说明 HelloWorld 项目中“响应智能体”的角色、触发条件、输入输出格式及使用方式。

## 职责

响应智能体是 Multica 工作流中负责处理分配到的 issue 并产出最终结果的 Agent，主要职责包括：

- 读取 issue 的标题、描述、元数据（metadata）和评论历史，理解任务背景。
- 根据需求在仓库中完成开发、文档、配置或其他必要改动。
- 当涉及代码/文件变更时，创建分支、提交并推送；必要时打开 Pull Request。
- 通过 issue 评论向团队汇报结果，并按工作流更新 issue 状态。

## 触发条件

响应智能体在以下情况下被触发：

- issue 被分配给该智能体，且状态从 `backlog` 变为 `todo` 或 `in_progress`。
- 在 issue 评论中通过 `[@响应智能体](mention://agent/<agent-id>)` 被 `@mention`。
- 由 Multica autopilot 或主控 Agent 按工作流自动分派。

## 输入格式

响应智能体接收的输入包括：

- **issue 详情**：通过 `multica issue get <issue-id> --output json` 获取，包含 `title`、`description`、`metadata`、`status`、`comments` 等字段。
- **项目资源**：HelloWorld 的 GitHub 仓库，通过 `multica repo checkout https://github.com/2019-02-18/HelloWorld.git` 检出。
- **工作流上下文**：`AGENTS.md` 中定义的 Agent 身份、路由表、Stage 规则与评论规范。
- **可用技能（skills）**：如 `multica-working-on-issues`、`subagent-driven-development`、`writing-plans` 等。

## 输出格式

响应智能体的输出通常包括：

- **代码/文档变更**：以 commit 形式提交到 GitHub 分支。
- **Pull Request**：PR 标题建议包含 routable issue key（如 `MUH-34: 增加响应智能体说明文档`）；如需在合并时关闭 issue，正文应包含 `Closes MUH-34`。
- **issue 评论**：说明已完成的工作、相关 PR 链接、部署/验证状态及注意事项。

## 使用方式

1. 在 Multica 中创建 issue，明确需求、验收标准和相关文件。
2. 将 issue 分配给响应智能体，或将状态设置为 `todo`。
3. 响应智能体自动检出仓库、完成任务、推送变更并评论汇报。
4. 若打开 PR，合并后 issue 会根据 `Closes` 关键字自动进入 `done` 状态。

## 相关文件

- `index.html`：项目主页面。
- `README.md`：项目总览与入口文档。
- `.github/workflows/deploy.yml`：自动部署到 GitHub Pages 的工作流。
