# lx-consumer-reports

自动化投研报告发布仓库。仓库同时服务 **GitHub Pages（单站点）** 和 **Cloudflare Pages（双站点，日报/周报分离）**，每次覆盖式更新。

## 目录结构

```
lx-consumer-reports/
├── daily.html          ← 根目录，供 GitHub Pages 使用
├── weekly.html
├── daily/index.html    ← 子目录，供 Cloudflare Pages 日报项目使用（Root dir = daily）
└── weekly/index.html   ← 子目录，供 Cloudflare Pages 周报项目使用（Root dir = weekly）
```

`publish.py` 每次会同步把两份最新的 chainstyle 报告写入 4 个位置。

## 发布 URL

| 通路 | 日报 | 周报 |
| --- | --- | --- |
| GitHub Pages | https://ideal0617.github.io/lx-consumer-reports/daily.html | https://ideal0617.github.io/lx-consumer-reports/weekly.html |
| Cloudflare Pages | https://<daily-project>.pages.dev/ | https://<weekly-project>.pages.dev/ |

Cloudflare Pages 项目名称在 Dashboard 创建时确定，`<...>.pages.dev` 打开根路径就是报告本体。

## 更新流程

```powershell
cd d:\qoder\lx-consumer-reports
.\publish.ps1                # 同步 + commit + push
.\publish.ps1 --dry-run      # 仅显示会同步哪些文件
.\publish.ps1 --skip-push    # 同步 + commit 但不 push
```

命名约定（源目录内）：

- 日报：`nanfangxiaofeiyanxuan-daily-YYYYMMDD-chainstyle.html`
- 周报：`YYYY-MM/YYYY-WNN-chainstyle.html`

脚本按日期/周序倒序取最新一份。无变化时自动跳过 commit。

## Cloudflare Pages 项目配置

两个项目都指向本仓库，仅 Root directory 不同：

| 项目 | Production branch | Root directory | Build command | Output directory |
| --- | --- | --- | --- | --- |
| 日报 | `main` | `daily` | *（留空）* | *（留空，默认根）* |
| 周报 | `main` | `weekly` | *（留空）* | *（留空，默认根）* |

纯静态无需构建，`git push` 触发 Cloudflare 自动部署。
# lx-consumer-reports

自动化投研报告发布仓库。仓库根目录只维护两个稳定文件，**每次覆盖式更新**：

| 类型 | 文件 | 固定 URL |
| --- | --- | --- |
| 日报 | [`daily.html`](./daily.html) | https://ideal0617.github.io/lx-consumer-reports/daily.html |
| 周报 | [`weekly.html`](./weekly.html) | https://ideal0617.github.io/lx-consumer-reports/weekly.html |

## 更新流程

本地脚本从 `D:\obsidian-lx\daily-watchlist\daily-watchlist-reports\` 扫描最新的 chainstyle 报告，覆盖到本仓库并 `git push`。

```powershell
cd d:\qoder\lx-consumer-reports
python publish.py                 # 同步 + commit + push
python publish.py --dry-run       # 仅显示会同步哪些文件
python publish.py --skip-push     # 同步 + commit 但不 push
```

命名约定（源目录内）：

- 日报：`nanfangxiaofeiyanxuan-daily-YYYYMMDD-chainstyle.html`
- 周报：`YYYY-MM/YYYY-WNN-chainstyle.html`

脚本按日期/周序倒序取最新一份。无变化时自动跳过 commit。
