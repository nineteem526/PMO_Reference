# 02. 技术架构与代码结构

## 架构选择：模块化单体（MVP）

- **前端：** React + TypeScript + Vite + Ant Design；确有可用数据时使用 ECharts。
- **后端：** Python 3.11 + FastAPI + Uvicorn；按 API / Service / Schema / Config 分层。
- **身份：** Microsoft Entra ID + App Service Authentication；后端按用户与角色校验，目标工具继续独立鉴权。
- **数据：** 首版目录配置文件（JSON）；收藏可暂存浏览器本地；未来按真实需求选 SharePoint Lists / Azure SQL。
- **运行：** React 在构建时编译为静态资源；FastAPI 提供页面与 /api 路由；**一个 Linux App Service Web App** 承载首版。
- **发布：** GitHub 负责代码版本；GitHub Actions 只在批准且 Runner 可用时使用；否则按批准路径用 Azure CLI 发布 ZIP。

```text
浏览器 / 企业认证
       │
       ▼
 Azure App Service（Linux / Python）
  ├─ React dist 静态页面（/、/tools 等）
  ├─ FastAPI 接口（/api/*）
  ├─ 工具目录配置（只读）
  └─ 链接获授权的独立业务工具
       ├─ Web RFQ 应用
       ├─ 文档与报表服务
       └─ 经批准的其他入口
```

## 源码仓库建议结构（真实代码放企业私有仓库）

```text
pmo-portal/
├── frontend/
│   ├── src/
│   │   ├── layouts/          # 页面 Shell、侧栏、顶栏
│   │   ├── pages/            # 七个页面
│   │   ├── components/       # 卡片、状态、搜索等
│   │   ├── services/         # API 客户端
│   │   ├── types/            # TS 类型
│   │   └── routes/
│   ├── package.json
│   └── package-lock.json
├── backend/
│   ├── main.py              # ASGI 入口
│   ├── app/
│   │   ├── api/             # 路由
│   │   ├── services/        # 业务逻辑
│   │   ├── schemas/         # Pydantic
│   │   └── config/tools.json
│   ├── tests/
│   └── requirements.txt
├── scripts/build-release.ps1
├── docs/api-contract.md
└── README.md
```

## 开发与发布区别

- 本地：Windows 下分别运行 Vite（前端）和 Uvicorn（后端）；Vite 代理 /api 到后端。
- 发布：npm run build 得到 dist；将其复制到发布包内 static；FastAPI 同时服务 static 和 /api。
- ZIP 根目录应直接包含 main.py、app/、static/ 和 requirements.txt，不能再多套一级 release 文件夹。
- FastAPI 的 SPA fallback 只匹配前端页面路由；未知 /api 路径必须返回 404，而不是 index.html。

## 为什么不使用微服务 / Docker / MCP 网关开局

首版用户需求是工具发现、链接、交互和部署。多服务会带来配置、认证、日志、成本、故障定位和跨人协作负担。等出现独立资源需求、耗时后台任务、大规模 AI 编排时再拆分。

## 扩展边界

新增业务工具优先作为独立服务或获批链接接入，不强迫现有应用重写。统一门户不自动拥有被接入工具的用户权限。最终可以由多个 Web App 共享符合条件的 App Service Plan，但共享 CPU/内存，无法无限扩容。
