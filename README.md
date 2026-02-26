# softwaremaker

Cursor Command 合集：从零建仓、迭代开发、测试文档与截图。

## Commands

| 文件 | 用途 |
|------|------|
| `softwaremaker.md` | 一键创建 Go + React(Vite) 前后端项目、Docker 化并推送到 GitHub |
| `softwaremaker-iterate.md` | 已有项目上按需求迭代：需求澄清 → 方案确认 → 实现 → 自测与测试文档 |
| `softwaremaker-test-doc-agent.md` | 子 agent 用：编写前后端测试文档并标注/执行截图 |

## 使用

将本仓库中的 `*.md`（除本 README）复制到 `~/.cursor/commands/`，同名即可用 `/softwaremaker`、`/softwaremaker-iterate` 等触发。

## 内容

- **Commands**：上述 3 个 `.md` 为 Cursor Command 定义。
- **skills/**：上述 Command 依赖的 skill，可直接复制到 `~/.cursor/skills/` 使用。包含：
  - softwaremaker：`docker-expert`、`golang-pro`、`frontend-dev-guidelines`、`ui-ux-pro-max`、`gh-cli`、`database`、`coding-agent`。
  - softwaremaker-iterate 额外：`brainstorming`、`plan-writing`、`architecture-patterns`、`backend-patterns`、`api-design`、`database-migrations`、`react-patterns`、`frontend-patterns`、`clean-code`、`code-review-checklist`、`lint-and-validate`、`playwright-skill`、`screenshots`、`github-actions-templates`。
