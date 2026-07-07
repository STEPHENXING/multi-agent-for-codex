# 子 Agent 执行规范

这个文件是给人看的说明版；真正给 Codex 读取的是 `.codex/agents/*.toml`。

## Planner

输入：

1. 用户需求。
2. `AGENTS.md` 核心规则。
3. 已有项目结构和技术栈。

输出：

1. 数据库表结构；没有数据库时必须明确说明。
2. API 契约，包括方法、路径、请求、响应。
3. Frontend / Backend 可并行执行的任务清单。
4. 风险和待确认问题。

限制：

1. 不写业务代码。
2. 不直接修改 `frontend/` 或 `backend/`。

## Frontend

输入：

1. Planner 输出的 API 契约。
2. UI 规范和已有前端代码风格。
3. `AGENTS.md` 的目录边界和自检命令。

输出：

1. 修改文件列表。
2. 执行命令和结果。
3. 未解决风险。
4. 是否影响接口、配置或数据库。

必跑命令：

```powershell
Push-Location frontend
npm install
npx eslint .
npx tsc --noEmit
npm run build
Pop-Location
rg -n '\\u[0-9a-fA-F]{4}' frontend backend docs .codex AGENTS.md README.md
```

## Backend

输入：

1. Planner 输出的 API 契约。
2. 数据库表结构。
3. `AGENTS.md` 的目录边界和自检命令。

输出：

1. 修改文件列表。
2. 执行命令和结果。
3. 未解决风险。
4. 是否影响接口、配置或数据库。

必跑命令：

```powershell
Push-Location backend
python -m pip install -r requirements.txt
python -m compileall app tests
python -m pytest
ruff check --fix .
ruff format .
Pop-Location
rg -n '\\u[0-9a-fA-F]{4}' frontend backend docs .codex AGENTS.md README.md
```

## Reviewer

输入：

1. Planner 的 API 契约。
2. Frontend / Backend 的返回结果。
3. 代码 diff 或修改文件列表。
4. 自检命令结果。

输出：

1. P1 / P2 / P3 问题清单。
2. 是否允许进入集成验证。
3. 必须回派给哪个子 Agent。
4. 缺失的检查命令或证据。

审查重点：

1. 是否遵守目录边界。
2. 是否按契约实现。
3. 是否执行了具体自检命令。
4. 是否存在 `\uXXXX` 中文转义。
5. 是否有接口、配置、数据库影响未声明。
