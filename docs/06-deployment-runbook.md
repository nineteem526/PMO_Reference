# 06. Azure App Service（Linux）发布 Runbook

> 本文为部署流程样例，不代表具备企业权限。真实资源位置、计费、访问控制、审批、身份来源必须由企业管理员确认。

## 方案

React + Vite 构建静态文件，FastAPI 提供 `/` 与 `/api/*`，部署为**一个 Python/Linux Web App**。

建议配置项：

| 配置 | 初始建议 |
|---|---|
| Publish | Code（非 Container） |
| Runtime | Python 3.11 或企业批准版本 |
| OS | Linux |
| Startup | `python -m uvicorn main:app --host 0.0.0.0 --port 8000` |
| Python 构建 | ZIP 源码部署时按实际部署流程启用 `SCM_DO_BUILD_DURING_DEPLOYMENT=true` |
| Authentication | Entra ID，限制为获授权的试用用户 |
| 数据 | 不在部署文件目录长期存放用户业务数据 |

## 发布 ZIP 内容（ZIP 根层）

```text
main.py
app/
  api/
  services/
  schemas/
  config/tools.json
static/
  index.html
  assets/...
requirements.txt
```

构建流程：

1. `cd frontend && npm ci && npm run build`
2. 执行后端单元测试与 JSON 校验。
3. 清空新建的 staging 文件夹，只复制必要的后端源文件和前端 dist 文件。
4. 检查文件清单，排除秘钥、数据库、本地日志、真实业务文件、虚拟环境。
5. 将 staging **内部文件**打 ZIP，并记录对应 Git commit。

## 首版手动发布（当组织 Runner 不可用时）

使用企业批准的 ADM 或管理身份进入获批的 Cloud Shell/Azure CLI，上传经审核的发布 ZIP：

```bash
RG="YOUR_APPROVED_RESOURCE_GROUP"
APP="YOUR_APPROVED_APP_NAME"
ZIP="pmo-portal-release.zip"

az account show --output table
az webapp config appsettings set \
  --resource-group "$RG" --name "$APP" \
  --settings SCM_DO_BUILD_DURING_DEPLOYMENT=true
az webapp config set \
  --resource-group "$RG" --name "$APP" \
  --startup-file "python -m uvicorn main:app --host 0.0.0.0 --port 8000"
az webapp deploy \
  --resource-group "$RG" --name "$APP" \
  --src-path "$ZIP" --type zip
```

以上资源名和 ZIP 都是占位值；运行命令会变更云资源，应在测试环境通过授权后执行。

## GitHub Actions 发展路线

Runner 可用并获批准后：Checkout → `npm ci` → `npm run build` → Python 测试 → 生成同样的 ZIP → 使用 OIDC 部署身份 → 先部署测试环境 → 验收后上线。**不将 ADM 密码、个人令牌或 YubiKey PIN 写进 Secrets。**

## 验收和回滚

- 实际员工账号能打开页面并完成一项功能；未授权账号访问受阻。
- `/api/health` 返回健康状态；浏览器能直接刷新子路径；关键静态资源可用。
- 工具链接符合预期，目标工具需要再次鉴权时能正常处理。
- 发布保留 Git SHA、ZIP 的经批准保存位置、操作者、测试结果。
- 故障时回退到上一已验收 ZIP；若发生了数据库写入/迁移，另做数据恢复方案。

## 费用提示

多个 Web App 可以在符合条件时共享一个 App Service Plan 的 CPU/内存，**不是每新增一个工具就买一台实例**；但数据库、网络、日志、AI API 可能额外收费。停止单个 Web App 通常不会使付费 Plan 自动停止计费。建预算告警且定期复核。
