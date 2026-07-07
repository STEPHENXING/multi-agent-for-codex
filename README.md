# fastapi-react-multiagent

Codex 多 Agent 协作模板，开箱即用。clone 到项目目录后，在 Codex 里跟主 Agent 说一句需求，四个子 Agent 自动按 Planner → Frontend/Backend 并行 → Reviewer → 集成验证的流水线协作。

## 技术栈

- 后端：FastAPI + Pydantic
- 前端：React + Vite + TypeScript

## 快速开始

### 1. Clone 模板

```bash
git clone https://github.com/STEPHENXING/multi-agent-for-codex.git my-project
cd my-project
rm -rf .git
git init
```

### 2. 在 Codex 中打开项目

用 Claude Code / Codex 打开 `my-project` 目录。模板自带的 `.codex/agents/` 目录会被自动识别，四个子 Agent 即刻就位。

### 3. 提第一个需求

直接对主 Agent 说，例如：

> 帮我做一个任务管理工具，支持创建、编辑、删除任务，前端用表格展示。

### 4. 观察流水线运转

主 Agent 自动调度：

```text
用户需求
  → Planner  输出 API 契约 + 数据库设计 + 任务拆解
  → Frontend / Backend  并行开发
  → Reviewer  审查目录边界、契约一致性、自检结果
  → 主 Agent  集成验证
  → ✅ 交付
```

你只需要在 Planner 产出契约时确认一声，其余全自动。

## 文件结构

```text
my-project/
  AGENTS.md                  # 8 条核心规则 + 自检命令 + 返回格式
  README.md
  .gitignore
  docs/
    agent-workflow.md        # 角色分工、循环流程、问题分级、集成验证清单
    subagent-specs.md        # 各子 Agent 的输入/输出/限制/必跑命令
  .codex/agents/
    planner.toml              # 需求规划师（只读）
    frontend.toml             # React 前端子 Agent
    backend.toml              # FastAPI 后端子 Agent
    reviewer.toml             # 质量审查员（只读）
  frontend/                   # ← 子 Agent 自动创建
  backend/                    # ← 子 Agent 自动创建
```

`frontend/` 和 `backend/` 由子 Agent 第一次开发时自动生成，模板本身不预置业务代码。

## 角色与边界

| 角色 | 职责 | 边界 |
| --- | --- | --- |
| 主 Agent | 协调、编排、集成验证 | 不直接修改 `frontend/` 和 `backend/` |
| Planner | 需求拆解、数据库设计、API 契约 | 只输出规划，不写代码 |
| Frontend | React 页面、状态管理、接口调用 | 只修改 `frontend/` |
| Backend | FastAPI 接口、业务逻辑、数据模型 | 只修改 `backend/` |
| Reviewer | 审查契约一致性、质量风险、自检结果 | 不修改业务代码 |

## 常见需求示例

直接复制下面任意一句话到 Codex：

- "帮我做一个用户管理系统，有注册/登录/列表/删除功能"
- "给我做一个博客后台，支持写文章、编辑、发布、草稿状态"
- "做一个简单的数据看板，前端从后端 API 拉数据后用图表展示"

Planner 拆出契约后，Frontend 和 Backend 会各司其职、并行干活。

## 自定义

调整子 Agent 行为：编辑 `.codex/agents/*.toml` 中的 `developer_instructions`。

调整工作流规则：编辑 `AGENTS.md` 和 `docs/agent-workflow.md`。
