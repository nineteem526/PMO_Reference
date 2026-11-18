# 08. UI 设计系统与七张原型图

## 已确定的设计方向

- 桌面优先的企业级蓝白灰风格；深蓝侧栏、浅色背景、白色卡片与适量蓝色强调。
- 左侧固定主导航，顶部全局搜索、通知和用户入口；统一卡片圆角、间距、字体层级和图标。
- 首页着重工具快捷入口；工具导航使用搜索、分类筛选、卡片、入口按钮。
- 对于未完成的功能，使用“规划中/建设中/测试中”，不伪造交互成功。
- 中文文案在代码中统一校对；图片生成的示例数据、人物、日期仅作为视觉参考。

## 原型索引

| 页面 | 文件 |
|---|---|
| 1. 首页总览 | `assets/prototypes/01-home.png` |
| 2. 工具导航 | `assets/prototypes/02-tools.png` |
| 3. 流程中心 | `assets/prototypes/03-workflows.png` |
| 4. 数据看板 | `assets/prototypes/04-dashboard.png` |
| 5. 文件中心 | `assets/prototypes/05-files.png` |
| 6. AI 助手 | `assets/prototypes/06-ai.png` |
| 7. 系统设置 | `assets/prototypes/07-settings.png` |

## 首版组件建议

- LayoutShell：导航 / 顶栏 / 主区域。
- ToolCard：图标、标题、描述、类别、状态、进入工具。
- ToolFilter：搜索框、类别标签、状态筛选。
- EmptyState / ErrorState / LoadingState：统一用户反馈。
- Breadcrumb / Notification / UserBadge：基础显示，真实身份从受信任的认证上下文获得。
- MockDataBanner：凡是示例指标均显示“演示数据”。

## 视觉验收

1. 七个页面统一侧栏和顶栏。
2. 首页/工具导航高保真且可交互，其他页面至少能导航。
3. 分辨率变化时不严重溢出，表格可合理滚动。
4. 键盘 Tab 能访问主要操作，按钮有焦点样式。
5. 真实工具入口能用；尚未实现的控件有明确禁用/说明状态。

**交付约定：** 将图片给 AI 作风格和布局参照，同时把本页当作结构化的验收准则。不要让 AI 从图片猜测真实业务数据、登录权限或后端能力。
