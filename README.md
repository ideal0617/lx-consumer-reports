# lx-consumer-reports

自动化投研报告发布仓库。仓库同时服务 **GitHub Pages（单站点）** 和 **Cloudflare Pages（多站点，日报/周报/回购日报分离）**，每次覆盖式更新。

## 目录结构

```
lx-consumer-reports/
├── daily.html            ← 根目录，供 GitHub Pages 使用
├── weekly.html
├── buyback.html
├── daily/index.html      ← 子目录，供 Cloudflare Pages 日报项目使用（Root dir = daily）
├── weekly/index.html     ← 子目录，供 Cloudflare Pages 周报项目使用（Root dir = weekly）
└── buyback/index.html    ← 子目录，供 Cloudflare Pages 回购日报项目使用（Root dir = buyback）
```

`publish.py` 每次会同步把三份最新的报告写入 6 个位置。

## 发布 URL

| 通路 | 日报 | 周报 | 回购增持日报 |
| --- | --- | --- | --- |
| GitHub Pages | https://ideal0617.github.io/lx-consumer-reports/daily.html | https://ideal0617.github.io/lx-consumer-reports/weekly.html | https://ideal0617.github.io/lx-consumer-reports/buyback.html |
| Cloudflare Pages | https://\<daily-project\>.pages.dev/ | https://\<weekly-project\>.pages.dev/ | https://\<buyback-project\>.pages.dev/ |

Cloudflare Pages 项目名称在 Dashboard 创建时确定，`<...>.pages.dev` 打开根路径就是报告本体。

## 更新流程

```powershell
cd d:\qoder\lx-consumer-reports
.\publish.ps1                # 同步 + commit + push
.\publish.ps1 --dry-run      # 仅显示会同步哪些文件
.\publish.ps1 --skip-push    # 同步 + commit 但不 push
```

命名约定（源目录 `D:\obsidian-lx\daily-watchlist\daily-watchlist-reports\` 内，前缀严格匹配）：

- 日报：`nanfangxiaofeiyanxuan-daily-YYYYMMDD-chainstyle.html`
- 周报：`YYYY-MM/YYYY-WNN-chainstyle.html`
- 回购增持日报：`nanfangxiaofeiyanxuan-buyback-YYYYMMDD.html`（由 `/dw-buyback` 生成，见 [dw-buyback skill](../.qoder/skills/dw-buyback/SKILL.md)）

脚本按日期/周序倒序取最新一份。三份报告任一份有变化即触发一次 commit；全部无变化时自动跳过。回购日报为可选文件——若源目录内暂无 `nanfangxiaofeiyanxuan-buyback-*.html`，只发布日报与周报，不阻断流程。

## Cloudflare Pages 项目配置

三个项目都指向本仓库，仅 Root directory 不同：

| 项目 | Production branch | Root directory | Build command | Output directory |
| --- | --- | --- | --- | --- |
| 日报 | `main` | `daily` | *（留空）* | *（留空，默认根）* |
| 周报 | `main` | `weekly` | *（留空）* | *（留空，默认根）* |
| 回购日报 | `main` | `buyback` | *（留空）* | *（留空，默认根）* |

纯静态无需构建，`git push` 触发 Cloudflare 自动部署。

## 关联 Skill

- `/dw-today` → 生成 daily 报告
- `/dw-week` → 生成 weekly 报告
- `/dw-buyback` → 生成 buyback 回购增持日报（[SKILL.md](../.qoder/skills/dw-buyback/SKILL.md)）
