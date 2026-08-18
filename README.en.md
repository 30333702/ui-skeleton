# Dark Admin UI

A reusable dark-themed admin dashboard UI skeleton built with **Bootstrap 5.3 + Jinja2**. Extracted from the api-gateway project.

## Features

- 🎨 Dark theme driven by CSS variables (change `:root` to re-color)
- 📱 Responsive: desktop sidebar + mobile topbar/drawer menu
- 📊 Components: stat cards, data tables, badges, buttons, forms, modals, code blocks, tip boxes, empty states, auth pages
- 🔌 Pure CDN dependencies (Bootstrap 5.3.3 + Bootstrap Icons 1.11.3), no build step

## File Structure

```
ui-skeleton/
├── templates/
│   ├── base.html       # Skeleton (sidebar/topbar/drawer/styles/scripts)
│   ├── dashboard.html  # Example: dashboard (stat cards + table + tips + empty state + modal)
│   └── login.html      # Example: auth page
└── README.md
```

## Integration (FastAPI)

### 1. Pass context variables when rendering

```python
from fastapi import Request
from fastapi.templating import Jinja2Templates

templates = Jinja2Templates(directory="templates")

@app.get("/dashboard")
def dashboard(request: Request, user=Depends(get_current_user)):
    return templates.TemplateResponse("dashboard.html", {
        "request": request,
        "user": user,               # must have email; logged-in renders sidebar, anonymous uses auth layout
        "BRAND": "My Admin",         # optional, brand name (default "Admin")
        "BRAND_ICON": "bi-shield-lock",  # optional, brand icon (default bi-shield-lock)
        "NAV_ITEMS": [               # optional, sidebar navigation
            {"url": "/dashboard", "label": "Dashboard", "icon": "bi-speedometer2"},
            {"url": "/users", "label": "Users", "icon": "bi-people"},
            {"url": "/keys", "label": "API Keys", "icon": "bi-key"},
        ],
    })
```

### 2. Active nav highlighting

Automatic based on `request.url.path == item.url`.

### 3. User object contract

- `user.email` — shown in sidebar footer / avatar initial
- `user.role_label` — optional, role text (e.g. "Admin")
- `user=None` when anonymous → auto-switches to auth layout (no sidebar)

## Common Customizations

| Need | How |
|------|-----|
| Change theme color | Edit `--accent` / `--accent-2` in `:root` |
| Stack stat cards vertically | Change `col-md-3 col-6` to `col-12` on cards |
| Narrow sidebar | Change `.sidebar { width: 250px }` to `210px` |
| Narrow & center content | Change `.main-content { max-width: 1400px }` to `880px` |
| Per-page overrides only | Add `<style>` inside the page template (won't affect other pages) |

## Included Component Classes

- `stat-card` + `stat-icon` + `bg-grad-*` (indigo/green/amber/sky/rose gradients)
- `badge-key` (monospace key display)
- `tip-box` (icon + description info block)
- `code-block` (code snippet)
- `empty-state` (empty state)
- `auth-wrap` / `auth-card` (auth page gradient background)
- `.table` dark table styles
