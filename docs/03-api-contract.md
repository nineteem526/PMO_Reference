# 03. 前后端 API 契约（草案 v0.1）

> 前端可以基于本页 Mock 数据独立开发。修改字段之前，双方先更新契约并在 PR 中说明。

## 路由

| 方法 | 路径 | 含义 |
|---|---|---|
| GET | /api/health | 健康和构建版本 |
| GET | /api/tools | 可访问的工具列表（按权限过滤） |
| GET | /api/tools/{id} | 单个工具，未知 ID 返回 404 |
| GET | /api/me | 当前已授权用户的基本资料（不暴露令牌） |

## GET /api/tools（示例）

```json
{
  "tools": [
    {
      "id": "rfq-search",
      "name": "RFQ 智能检索",
      "description": "检索并对照获授权的示例资料",
      "category": "AI工具",
      "icon": "search",
      "status": "testing",
      "launchType": "web",
      "url": "https://example.invalid/rfq",
      "tags": ["检索", "文档"],
      "ownerDisplayName": "工具维护团队"
    }
  ]
}
```

其中 example.invalid **纯属占位，绝不是可用的企业网站**。开发时可将测试工具置为 testing，确定真实地址并获审批后再改成 available。

### 枚举

- status: available / testing / coming_soon / disabled
- launchType: web / desktop / document
- category：由工具目录配置统一管理，避免前端硬编码大量中文分类。

### 约定

- 前端应渲染「加载」「失败」「空列表」「建设中」等真实状态。
- 工具 URL 必须受控，服务端校验协议和允许的域名/地址；禁止开放跳转。
- 打开外部链接时使用 rel="noopener noreferrer"。
- 前端卡片可见不等于用户获得访问目标系统的权限。
- `/api/me` 不能从请求中自行信任伪造的用户名；须验证应用认证上下文。
- 不新增虚构“成功部署”“AI 调用次数”“审批已完成”等数据。
- 所有有副作用的 API 将来需增加身份、授权、审计和防重复提交设计。

## 初期不用实现的 API

文件上传、审批动作、多 Agent 调用、跨系统数据写入、在线工具管理员接口，均不是首版必须项。
