# Dark Admin UI — 深色管理后台模板骨架

从 api-gateway 项目抽出的可复用 UI 骨架（FastAPI + Jinja2 + Bootstrap 5.3 深色主题）。

## 特性

- 🎨 深色主题（CSS 变量驱动，改 `:root` 即可换色）
- 📱 响应式：桌面侧边栏 + 移动端顶栏/抽屉菜单
- 📊 组件齐全：统计卡片、数据表格、徽章、按钮、表单、弹窗、代码块、tip-box、空状态、认证页
- 🔌 纯 CDN 依赖（Bootstrap 5.3.3 + Bootstrap Icons 1.11.3），无构建步骤

## 文件结构

```
ui-skeleton/
├── templates/
│   ├── base.html       # 骨架（侧边栏/顶栏/抽屉/样式/脚本）
│   ├── dashboard.html  # 示例：控制台（统计卡+表格+tip+空状态+弹窗）
│   └── login.html      # 示例：认证页
└── README.md
```

## 接入你的 FastAPI 项目

### 1. 配置变量（在路由渲染时传入 context）

```python
from fastapi import Request
from fastapi.templating import Jinja2Templates

templates = Jinja2Templates(directory="templates")

@app.get("/dashboard")
def dashboard(request: Request, user=Depends(get_current_user)):
    return templates.TemplateResponse("dashboard.html", {
        "request": request,
        "user": user,               # 必须含 email；登录后渲染侧边栏，未登录走 auth 布局
        "BRAND": "My Admin",         # 可选，品牌名（默认 "Admin"）
        "BRAND_ICON": "bi-shield-lock",  # 可选，品牌图标（默认 bi-shield-lock）
        "NAV_ITEMS": [               # 可选，侧边栏导航
            {"url": "/dashboard", "label": "控制台", "icon": "bi-speedometer2"},
            {"url": "/users", "label": "用户管理", "icon": "bi-people"},
            {"url": "/keys", "label": "API Keys", "icon": "bi-key"},
        ],
    })
```

### 2. 侧边栏 active 高亮

自动根据 `request.url.path == item.url` 高亮当前页。

### 3. user 对象约定

- `user.email` — 显示在侧边栏底部 / 头像首字母
- `user.role_label` — 可选，角色文字（如"管理员"）
- 未登录时传 `user=None` → 自动切换为认证页布局（无侧边栏）

## 常用改法

| 需求 | 做法 |
|------|------|
| 换主题色 | 改 `:root` 里 `--accent` / `--accent-2` |
| 统计卡上下堆叠 | 卡片 `col-md-3 col-6` 改成 `col-12` |
| 侧边栏变窄 | 改 `.sidebar { width: 250px }` 为 `210px` |
| 内容窄化居中 | 改 `.main-content { max-width: 1400px }` 为 `880px` |
| 只改单个页面 | 在页面模板内加 `<style>`（不会影响其他页） |

## 已包含的组件类

- `stat-card` + `stat-icon` + `bg-grad-*`（indigo/green/amber/sky/rose 渐变）
- `badge-key`（代码样式 key 显示）
- `tip-box`（图标+说明信息块）
- `code-block`（代码块）
- `empty-state`（空状态）
- `auth-wrap` / `auth-card`（认证页渐变背景）
- `.table` 深色表格样式
