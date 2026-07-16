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
