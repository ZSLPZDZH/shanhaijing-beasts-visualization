# GitHub 项目介绍优化设计

## 目标

把仓库从“部署交接页”调整为面向普通读者与作品集访客的项目展示页，让访问者在首屏快速理解：项目展示什么、数据规模多大、可以如何探索，以及在哪里立即体验。

仓库仍保留面向维护者的本地运行与部署信息，但这些内容移到 README 后部，不再占据主要叙事位置。

## 项目定位

项目定位为：以原生 Web 技术呈现《山海经》神兽名录的交互式可视化图谱。

核心事实：

- 151 条名录记录，对应 152 只神兽。
- 覆盖《山海经》十八卷，并按页面现有九组卷次展示分布。
- 按瑞兽、凶兽、异兽三类组织与筛选。
- 支持名称、拼音和内容要点搜索，以及神兽详情查看。

README 不宣称数据具有学术权威性；数据口径与来源必须独立说明。

## GitHub About 信息

仓库描述使用：

> 以卷次、吉凶分类与搜索交互探索 151 条《山海经》神兽名录的单页可视化图谱。

Homepage 使用：

`https://shanhaijing-beasts-visualization.vercel.app`

Topics 使用：

- `shanhaijing`
- `chinese-mythology`
- `data-visualization`
- `interactive-visualization`
- `cultural-heritage`
- `html`
- `css`
- `javascript`

## README 结构

README 按以下顺序组织：

1. 项目标题与一句话定位。
2. 在线体验、许可证等关键入口。
3. 生产页面预览图。
4. 数据规模与主要交互能力。
5. 数据来源、151/152 计数差异及使用边界。
6. 在线访问与本地运行方式。
7. 技术特点与项目结构。
8. 部署和维护资料。
9. 许可证与内容权利说明。

首屏避免堆放维护命令。徽章只保留能够帮助访客判断项目状态的少量项目，不使用无意义的装饰性徽章。

## 预览图

从当前 Vercel 生产页面采集桌面端首屏，保存为 `docs/images/project-preview.png`。截图应完整呈现标题、数据卡片和两组主要图表的上半部分，不包含浏览器界面或调试信息。

README 通过仓库相对路径引用该图片，确保 GitHub 页面无需依赖外部图片服务。

## 许可证与内容边界

新增 MIT License，版权信息使用 `Copyright (c) 2026 ZSLPZDZH`。MIT 许可证只明确覆盖仓库中的原创代码。

README 同时声明：

- 《山海经》原典属于公共文化文本。
- 整理数据、解释文字和视觉内容可能包含独立来源或整理成果，不因代码采用 MIT License 而自动获得相同授权。
- 项目用于文化内容探索与可视化展示，不作为学术校勘版本。

## 文件变更

- 重写 `README.md` 的信息层级，同时保留当前未提交修改中新增的飞书更新手册链接和部署说明。
- 新增 `LICENSE`。
- 新增 `docs/images/project-preview.png`。
- 不修改 `index.html` 或 `山海经神兽可视化.html` 的产品功能。
- 不修改现有 `CLAUDE.md` 和 `docs/manual-github-vercel-update.xml` 的未提交内容。

## 验证

- 检查 README 中的仓库相对链接和外部链接。
- 确认预览图能在本地读取，格式为 PNG。
- 确认 `LICENSE` 内容完整且版权年份、名称正确。
- 启动本地静态服务器并验证首页返回 HTTP 200。
- 检查 `git diff`，确保没有覆盖现有未提交内容。

## 发布边界

所有修改在 `codex/improve-project-presentation` 分支完成并本地提交。未经用户明确要求，不执行 `git push`，也不修改 GitHub About、Homepage 或 Topics 等远端设置。
