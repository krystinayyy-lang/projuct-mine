# 行政智能化需求 · 项目看板

一个零依赖的项目管理看板，支持「项目大类 → 项目 → 阶段 → 子任务」四层结构，
单文件前端 + 可选 Python 共享服务。

## 在线预览

https://krystinayyy-lang.github.io/projuct-mine/

## 功能

- **14 个项目大类**，可增删改，支持拖拽排序
- **项目分层**：大类下建项目，项目实施类有 6 个阶段（调研 → 需求确认 → 采购/立项 → 实施开发 → 验收 → 上线）
- **运维类项目**：不走阶段，下面是持续要解决的问题清单，带解决进度条
- **按周 / 月 / 半年 / 年** 查看，所有子项跟随周期筛选
- **一句话录入**：说人话自动识别大类、优先级、提需人、截止时间
- **需求变更留痕**：登记变更自动生成差异说明，永久记录
- **项目构成环形图**：实施类项目 ≥2 个时展示阶段分布
- **深浅色主题**切换

## 两种使用方式

### 1. 在线只读（当前 GitHub Pages）

直接打开上面的链接。数据从 `data.json` 读取，是**只读快照**——
你在页面上做的修改只存在自己浏览器里，刷新会回到快照状态。

### 2. 本地完整版（多人共享、可编辑）

```bash
python3 server.py          # 默认 0.0.0.0:8765
```

数据存在 SQLite（`data.db`），支持多人同时编辑、10 秒自动同步、每日备份。
同一局域网的人访问 `http://<你的IP>:8765/` 即可协作。

## 文件说明

| 文件 | 说明 |
| --- | --- |
| `index.html` | 前端全部（HTML+CSS+JS 单文件） |
| `data.json` | 看板数据（在线版读取此文件） |
| `server.py` | 共享服务（Python 标准库 + SQLite，零依赖） |
| `import_excel.py` | 把 Excel 需求池导入看板 |
| `migrate_projects.py` | 按「大类+状态」把需求归并成项目 |
| `gen_snapshot.py` | 生成自带数据的单文件快照 |

## 更新在线数据

```bash
curl -s http://127.0.0.1:8765/api/data | python3 -c "
import sys,json
d=json.load(sys.stdin); d.pop('rev',None)
open('data.json','w',encoding='utf-8').write(json.dumps(d,ensure_ascii=False,indent=1))"
```

然后提交 `data.json`，GitHub Pages 会自动重新部署。

## 说明

本仓库展示的是行政 IT 侧需求管理看板的通用实现，数据为示例用途。
