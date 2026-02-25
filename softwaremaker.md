# softwaremaker

## 用途

当用户说「帮我新建一个前后端项目并推到 GitHub」「建一个 React + Go 项目并上传到仓库」或触发本 Command 时，**不询问用户，直接按下列步骤执行**。

目标流程：

1. 在指定目录下创建一个**前后端分离**的项目（例如 `stock-analysis-agent`）：
   - 后端：Go（简单 REST API），如端口 `8081`
   - 前端：React + Vite（TypeScript），如端口 `5174`
2. 为前后端生成 Dockerfile 和 `docker-compose.yml`，支持本地一键起服务。
3. 使用 git 初始化仓库，并通过 **gh CLI** 在 GitHub 上创建远程仓库并推送代码。

---

## 交互步骤（Agent 执行时遵循）

**不询问用户、不等待确认，直接按下列步骤依次执行。**

1. **默认方案**
   - 项目根目录：当前工作目录（或用户 @ 的目录）
   - 后端：Go，前端：React + Vite（TypeScript）
   - 启用 Docker / docker-compose
   - 创建 GitHub 仓库并推送

2. **生成项目结构**
   - 在目标目录中：
     - 创建 `backend/`，包含：
       - 简单的 Go HTTP 服务（REST API 示例 + CORS）
       - `cmd/server/main.go`、`internal/` 等基础结构
     - 创建 `frontend/`，使用 Vite + React + TS：
       - 基础页面，能通过 `/api/...` 向后端发请求
       - 简单的状态处理与错误展示
     - 创建 `README.md` 和 `start.sh`：
       - 说明如何直接本地运行前后端
       - 说明如何使用 Docker / docker-compose 运行

3. **Docker 化**
   - 在项目根目录生成：
     - `Dockerfile.backend`：多阶段构建 Go 服务镜像
     - `frontend/Dockerfile`：构建静态资源并用 Nginx 或 Node 服务静态文件
     - `docker-compose.yml`：编排前后端服务，映射合适的宿主机端口
   - 保证端口不要与用户已有服务冲突；若冲突，自动向上调整（如 8081→9081，5174→9075），并同步更新 README 说明。

4. **Git 初始化与 GitHub 推送**
   - 在项目根目录执行：
     - `git init`
     - 添加 `.gitignore`（Node、Go、Docker 等常见忽略规则）
     - `git add . && git commit -m "Initialize fullstack project"`
   - 使用 `gh repo create`：
     - 仓库名默认与项目目录名一致
     - 默认 public
     - 设置 `origin` 远程，并执行首次 `git push -u origin <branch>`
   - 若出现 `Permission denied (publickey)`，在结果反馈中说明需配置 SSH key 或改用 HTTPS。

5. **结果反馈**
   - 告诉用户：
     - 本地项目路径
     - 本地启动命令（直接运行 / Docker 两种）
     - GitHub 仓库地址
   - 提醒用户后续可以继续在该项目上开发、提交和推送。

---

## 使用约束

- 遵守用户关于端口、技术栈、权限等的显式约束（如禁止使用 `kill`/`pkill`）。
- 若操作涉及破坏性修改（删除文件、覆盖已有仓库），必须先与用户确认。
- 如果用户只想做项目初始化而**不推送到 GitHub**，则只执行本地生成和 git 初始化，跳过 `gh repo create` 和 `git push`。

---

## 调用工具 / 技能

在执行本 Command 时，Agent 需按需调用以下技能来完成子任务：

- `/docker-expert`：设计和优化 Dockerfile、多阶段构建、`docker-compose.yml` 结构，以及容器安全加固。
- `/golang-pro`：设计和实现 Go 后端服务（路由、并发、性能优化、测试等），保证后端符合现代 Go 最佳实践。
- `/frontend-dev-guidelines`：按照严格的前端架构规范（React + TypeScript）实现或重构前端代码，关注可维护性与性能。
- `/ui-ux-pro-max`：为前端生成或优化 UI/UX 设计方案（配色、布局、动效、可访问性等），用于落地成页面样式。
- `/gh-cli`：通过 GitHub CLI 完成登录、创建仓库、配置远程、创建 PR 等 GitHub 相关操作。
- `/database`：当项目涉及持久化存储时，用于数据库选型、建模、迁移和性能优化的整体工作流设计。
- `/coding-agent`：在需要由 Claude Code / 其他终端智能体自动完成较大子任务（如批量重构、生成样板代码）时，使用 bash + PTY 启动对应 coding agent，在目标项目目录中运行子任务，再由当前 Agent 负责结果验证与收尾。


