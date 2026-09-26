# 🗂 全栈演示项目包：浏览器 → Python → SQL

一个可直接运行的完整全栈链路演示（任务管理器），把这条经典架构完整跑通：

```
浏览器 (HTML/CSS/JS)
        |
        | HTTP / fetch (JSON)
        v
Python 后端 (Flask)
        |
        | SQL 驱动 (sqlite3)
        v
SQL 数据库 (SQLite, 单文件 app.db)
```

## 目录结构

```
fullstack-demo/
├── frontend/               # 浏览器层
│   ├── index.html          # 页面结构
│   ├── style.css           # 页面样式
│   └── app.js              # 用 fetch 调用后端 API
├── backend/
│   └── app.py              # Python 后端：HTTP 路由 + SQL 操作
├── data/
│   └── app.db              # SQLite 数据库文件（首次启动自动创建）
├── requirements.txt        # Python 依赖
├── start.bat               # Windows 一键启动
└── README.md
```

## 数据流转（一个"添加任务"的完整旅程）

1. **浏览器**：用户在表单里输入标题，点「添加」→ `app.js` 用 `fetch("/api/tasks", { method: "POST" })` 发出 HTTP 请求
2. **HTTP / fetch**：请求体是 JSON，经网络到达 Flask 后端
3. **Python 后端**：Flask 路由 `create_task()` 校验参数
4. **SQL 驱动**：`app.py` 通过 `sqlite3` 执行 `INSERT INTO tasks ...`
5. **SQL 数据库**：数据持久化到 `data/app.db` 文件，后端把新记录以 JSON 返回
6. **浏览器**：`app.js` 收到响应后重新拉取列表和统计，页面刷新

## REST API 一览

| 方法 | 路径 | 作用 | 对应 SQL |
|------|------|------|----------|
| GET | `/api/tasks?q=关键词` | 列表/搜索任务 | `SELECT ... LIKE` |
| POST | `/api/tasks` | 新建任务 | `INSERT INTO` |
| PUT | `/api/tasks/<id>` | 切换完成状态 | `UPDATE` |
| DELETE | `/api/tasks/<id>` | 删除任务 | `DELETE FROM` |
| GET | `/api/stats` | 统计（含分类柱状图数据） | `COUNT` / `GROUP BY` |

## 如何运行

```bash
# 1. 安装依赖
pip install -r requirements.txt

# 2. 启动
python backend/app.py

# 3. 浏览器打开 http://127.0.0.1:5000
```

Windows 下直接双击 `start.bat` 即可。

## 学习建议

- 打开浏览器 **F12 → Network** 面板，观察每次点击触发的 fetch 请求与响应
- 用 **DB Browser for SQLite** 打开 `data/app.db`，实时看到页面操作造成的数据变化
- 尝试在 `app.py` 中新增一个接口（例如"按分类筛选"），再到 `app.js` 中调用它
