# 05. 本地开发、跨平台兼容与测试

## Windows 开发不代表生产也必须用 Windows

建议继续在获授权的 Windows 工作站中使用 VS Code + Git。最终目标为 Linux App Service，因此从第一天起遵循：

- Python 路径用 `pathlib`，不要写死 `C:\\Users\\...` 或 `LOCALAPPDATA`。
- 服务端配置通过受控环境变量注入；密钥不入库。
- 避免 Windows 专属 PowerShell 脚本成为 Linux 启动入口。
- 文件名大小写严格一致。
- 可行时在 Linux CI Runner 测试依赖；若 Runner 不可用，在获批环境中补充手工测试。

## 本地启动（示意）

```powershell
# 终端 A：后端，在真实工程仓库根目录
cd backend
py -3.11 -m venv .venv
.\.venv\Scripts\python.exe -m pip install -r requirements.txt
.\.venv\Scripts\python.exe -m uvicorn main:app --reload --port 8000
```

```powershell
# 终端 B：前端
cd frontend
npm ci
npm run dev
```

Vite 配置 `/api` 代理指向本地 FastAPI。首次初始化若尚无 package-lock.json，则先运行 npm install 并提交锁文件，之后用 npm ci。

## 最小测试

- 后端：GET /api/health 返回 200；GET /api/tools 返回格式正确的工具数组；错误 ID 返回 404。
- 前端：七个页面能路由；工具筛选与搜索有效；加载、空态、错误态可见。
- 静态构建：npm run build 完成；`static/index.html` 与静态 assets 可加载。
- 部署路由：浏览器直接刷新 `/tools` 不返回 404；未知 `/api/abc` 不被误返回 HTML。
- 权限：非授权身份不可绕过前端直接访问业务 API。

## 发布构建原则

在干净临时目录构建：先运行前端构建，再只复制需要部署的后端文件、requirements.txt 和 dist 静态产物。不能把 `.venv`、`node_modules`、`.env`、真实资料或用户目录打进 ZIP。
