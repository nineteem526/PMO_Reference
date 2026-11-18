# PMO Portal Reference

> 2026-11-17｜PMO Portal 的可公开技术参考、研发规范与 AI 协作交接资料。
>
> **不是生产代码仓库、公司资料备份库或官方制度文档。** 本仓库是公开参考材料；实际企业工程代码应留在获批的企业私有仓库。

## 项目愿景

先交付一个**有统一视觉、真正能导航并打开现有业务工具**的内部 PMO 门户（MVP），再根据真实业务需求扩展文件、流程、报表和 AI 能力。首版以工具导航为核心，不同时重建七套后台。

**首版必须完成：** 企业授权的登录与试用范围；首页；工具目录（搜索、分类、状态、入口）；至少一个真实可访问的工具；可重现部署与回退。

**首版可先展示原型/建设中：** 流程引擎、全员业务统计、文件上传管理、多系统 MCP 编排、自主 Agent、复杂数据库。

## 文档导航

| 文件 | 用途 |
|---|---|
| [docs/01-product-scope.md](docs/01-product-scope.md) | 页面范围、优先级、演示脚本、里程碑 |
| [docs/02-architecture.md](docs/02-architecture.md) | React/FastAPI/Azure 架构、仓库结构、模块边界 |
| [docs/03-api-contract.md](docs/03-api-contract.md) | 工具目录 API 与前后端契约 |
| [docs/04-two-person-workflow.md](docs/04-two-person-workflow.md) | 双人分工、Git 分支、PR、AI 编码规范 |
| [docs/05-local-development.md](docs/05-local-development.md) | Windows 本地开发、Linux 兼容、测试流程 |
| [docs/06-deployment-runbook.md](docs/06-deployment-runbook.md) | App Service 构建、配置、发布、回滚 |
| [docs/07-integrations-security.md](docs/07-integrations-security.md) | Entra、企业 API、MCP、数据安全边界 |
| [docs/08-design-system.md](docs/08-design-system.md) | 七页原型与 UI 规范 |
| [docs/09-status-and-decisions.md](docs/09-status-and-decisions.md) | 已决定、待确认、风险清单 |
| [templates/HANDOFF_TEMPLATE.md](templates/HANDOFF_TEMPLATE.md) | AI 跨环境交接模板 |
| [prompts/README.md](prompts/README.md) | 前端、后端、部署交接提示词 |
| [AGENTS.md](AGENTS.md) | 供代码助手先读的总指令 |

## 快速开发模式

```text
开发者 A（后端/集成）  -> backend/  -> API / 数据 / 发布
开发者 B（前端/体验）  -> frontend/ -> 首页 / 工具导航 / 组件
       ↓ 共同维护 docs/api-contract / PR 评审
                 企业私有研发仓库
       ↓ 测试、构建、经授权发布
        Azure App Service（Linux）
```

本 Reference 仓库只传递**脱敏的技术约定、决定、进度和问题摘要**，不承载源码自动同步，也不能取代企业内部 CI/CD。

## 公开仓库边界（重要）

禁止提交：任何真实客户/供应商文件、内部 URL/IP/租户与订阅标识、账号、身份认证截屏、密钥、证书、Token、内部日志、未公开代码、经营数据、公司安全策略细节。即使经过打码，也需要先确认公司允许发布。敏感问题请直接在企业批准的环境里处理。

**如何交换信息：** 将公司环境中的工作结果归纳成不包含受限信息的事实摘要，按 [交接模板](templates/HANDOFF_TEMPLATE.md) 放到此仓库。需要检查真实代码时，使用获批准的企业 GitHub 环境，而不是把真实文件搬到此处。

## 仓库的资料性质

所有架构和部署步骤均为建议/样例；实际可用权限、许可证、计费、网络与组织策略必须由企业 IT/管理员验证。文中人物、日期、URL、项目号、业务数据均为虚构或占位。页面原型中的数字是设计演示，不代表真实统计。
