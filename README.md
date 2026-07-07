# fastapi-react-multiagent

这是一个精简版 Codex 多 Agent 项目模板。

它复刻当前项目的协作方式：主 Agent 负责编排、配置和集成验证，Planner 先输出 API 契约，Frontend / Backend 子 Agent 并行开发，Reviewer 审查通过后再做集成验证。

技术栈调整为：

- Backend：FastAPI + Pydantic
- Frontend：React + Vite + TypeScript

## 文件结构

```text
fastapi-react-multiagent/
  AGENTS.md
  README.md
  .gitignore
  docs/
    agent-workflow.md
    subagent-specs.md
  .codex/
    agents/
      planner.toml
      frontend.toml
      backend.toml
      reviewer.toml
```

`backend/` 和 `frontend/` 不在模板中预置实现代码；它们由对应子 Agent 在具体需求开始后创建和维护。

## 使用方式

1. 先让 Planner 输出 API 契约和任务拆解。
2. Frontend 只修改 `frontend/`。
3. Backend 只修改 `backend/`。
 4. Reviewer 审查目录边界、契约一致性和自检结果。
 5. 主 Agent 执行前后端集成验证。

## 模板边界

这个目录只复刻多 Agent 协作模式和角色规范，不预置前后端业务系统。

后续真正开发时：

- Planner 先按需求输出数据库设计和 API 契约。
- Backend 子 Agent 再创建或修改 `backend/`，技术栈为 FastAPI。
- Frontend 子 Agent 再创建或修改 `frontend/`，技术栈为 React + Vite + TypeScript。
- Reviewer 审查通过后，主 Agent 才做集成验证。
