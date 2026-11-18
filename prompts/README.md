# AI 编程交接提示词（拷贝使用）

## 共用项目背景（先发给 AI）

你正在协助开发 PMO Portal 1.0，一个企业内部工具导航门户。第一版目标：统一七个页面的视觉和路由、重点实现首页与工具导航、通过受控配置/API 管理工具目录、至少接入一个经授权且可真实使用的业务工具。技术方案：React/TypeScript/Vite/Ant Design + Python 3.11/FastAPI，最终由 FastAPI 提供 React 构建后的静态资源与 /api/*，部署在一个 Linux Azure App Service。开发仓库划分 frontend/ 和 backend/，接口契约在 docs/api-contract.md。不要引入未经批准的 Agent、MCP、外部数据传输和真实客户资料；不要修改权限策略、分支保护或提交任何密钥。先阅读仓库，再列计划、改动范围、测试步骤，任务完成后提交测试证据与未解决问题。

## 前端 AI 任务 1：平台框架

仅修改 frontend/。初始化 React/TypeScript/Vite/Ant Design，按原型实现深蓝侧栏、顶部搜索/用户区域及七页路由：/、/tools、/workflows、/dashboard、/files、/ai、/settings。可复用统一 Shell、ToolCard、空态/错误态组件。首页和工具导航作为真实交互重点，其余页面如尚无数据应清楚标记规划中。先使用与 docs/api-contract.md 一致的 Mock 数据，后续替换为 GET /api/tools；不伪造成功操作与统计。提供运行和构建命令、截图（仅示例数据）和手工验证方式。

## 后端 AI 任务 1：工具目录 API

仅修改 backend/ 与获准更新的契约文档。用 FastAPI 建立 /api/health、/api/tools、/api/tools/{id}；目录来源为版本化 JSON 配置，使用 Pydantic 校验。为工具状态、launchType 与 URL 安全校验定义规则；严格限制工具跳转目标。支持最小单元测试，明确接口错误码，保留日志但不泄露数据和凭据。不要连接真实内部系统，不要引入数据库或 Agent 框架。

## 集成 AI 任务 1：一个 Web App 构建与发布

在 scripts/ 内创建明确白名单的发布打包脚本：执行前端 npm ci/npm run build、Python 测试，复制 backend/main.py、backend/app、backend/requirements.txt 和 frontend/dist 到 staging 根目录，确保 ZIP 根层包含 main.py、app/、static/、requirements.txt。实现 FastAPI 静态资源服务与 SPA 路由回退，未知 /api 路径应返回 404。不得触碰 Azure 账户、生产环境或权限；发布命令仅生成文档供获授权操作员审查。

## 代码审查 AI 任务

审查本次 PR 是否遵循接口契约、只修改必要范围、无敏感文件进入提交、无未经批准依赖、前后端构建测试通过、路由刷新正常、真实/模拟状态区分明确。输出可执行的缺陷清单，不无依据声称已通过生产验收。
