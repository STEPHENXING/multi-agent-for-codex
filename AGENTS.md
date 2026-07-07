# AGENTS.md

## 核心规则

1. 涉及 `frontend/` 的代码改动，主 Agent 必须交给 Frontend 子 Agent。
2. 涉及 `backend/` 的代码改动，主 Agent 必须交给 Backend 子 Agent。
3. 主 Agent 只负责规划、协调、配置、集成验证，不直接修改前后端业务代码。
4. Planner 必须先输出 API 契约，Frontend / Backend 才能开始开发。
5. Reviewer 必须审查通过后，才能进入主 Agent 集成验证。
6. 所有中文必须保存为真实 UTF-8 中文字符，禁止写成 `\uXXXX`。
7. 子 Agent 完成任务后，必须按“子 Agent 返回格式”汇报。
8. 涉及代码改动时，必须执行“自检命令”，不能只口头说明。

## 技术栈

- Backend：FastAPI、Pydantic，按需要使用 SQLAlchemy / Alembic。
- Frontend：React、Vite、TypeScript。
- 前后端联调：前端通过 Vite proxy 转发 `/api/*` 到 FastAPI。

## 子 Agent 返回格式

Frontend / Backend 子 Agent 完成任务后必须返回：

```md
## 修改文件
- path/to/file

## 执行检查
- `命令`: 通过 / 失败 / 未执行

## 检查结果
- 构建结果：
- Unicode 转义检查：
- 其他测试：

## 未解决风险
- 无 / 具体风险说明

## 影响面
- 是否影响接口：是 / 否
- 是否影响配置：是 / 否
- 是否影响数据库：是 / 否
```

## 自检命令

Frontend Agent 修改 `frontend/` 后必须执行：

```powershell
Push-Location frontend
npm install
npx eslint .
npx tsc --noEmit
npm run build
Pop-Location
```

Backend Agent 修改 `backend/` 后必须执行：

```powershell
Push-Location backend
python -m pip install -r requirements.txt
python -m compileall app tests
python -m pytest
ruff check --fix .
ruff format .
Pop-Location
```

所有 Agent 在提交或交付前必须检查中文 Unicode 转义：

```powershell
rg -n '\\u[0-9a-fA-F]{4}' frontend backend docs .codex AGENTS.md README.md
```

检查要求：该命令应无输出；如果出现匹配结果，必须修复为真实 UTF-8 中文字符。

如果环境没有 `rg`，使用：

```bash
grep -RInE '\\u[0-9a-fA-F]{4}' frontend backend docs .codex AGENTS.md README.md
```

## 完成标准

一个任务"完成"意味着通过 `docs/agent-workflow.md` 中"集成验证清单"的全部 6 项检查，而不仅仅是单个 Agent 自检通过。

## 详细流程

详细的循环流程、问题分级、Git 流程和回退策略放在：

```text
docs/agent-workflow.md
```
