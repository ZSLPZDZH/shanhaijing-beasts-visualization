# 《山海经》神兽可视化图谱

> 以卷次、吉凶分类与关键词搜索，探索 151 条《山海经》神兽名录。

[在线体验](https://shanhaijing-beasts-visualization.vercel.app) · [数据口径](#数据口径与来源) · [MIT License](LICENSE)

![《山海经》神兽可视化图谱页面预览](docs/images/project-preview.png)

## 一眼看懂

| 名录条目 | 神兽数量 | 原典范围 | 分类方式 |
| ---: | ---: | ---: | :--- |
| 151 条 | 152 只 | 18 卷 | 瑞兽 / 凶兽 / 异兽 |

这是一个零依赖的单页可视化项目，用更直观的方式浏览《山海经》中的神兽记录。访问者既可以观察不同卷次的收录分布，也可以按属性筛选，或直接搜索某只神兽。

## 可以如何探索

- **按卷观察**：用柱状图比较十八卷对应的九组卷次分布。
- **按属性理解**：查看瑞兽、凶兽、异兽的数量与占比。
- **组合筛选**：同时使用属性和卷次缩小名录范围。
- **关键词搜索**：按神兽名、拼音或内容要点检索。
- **查看详情**：在弹窗中阅读出处、特征和简要解释。
- **跨设备访问**：桌面端与移动端均可直接使用。

## 数据口径与来源

页面数据整理自项目的《山海经》神兽三套名录（精简、分类、全本），展示前已对三表条目数进行核对。

- **151 条与 152 只的差异**：`青鴍、黄鷔` 共用一条编号，但代表两只神兽，因此名录为 151 条，实体计数为 152 只。
- **分类口径**：瑞兽、凶兽、异兽是本项目为了浏览和比较而采用的整理方式，不代表唯一或权威的古籍分类体系。
- **使用边界**：项目用于文化内容探索与可视化展示，不作为学术校勘版本或正式引文依据。

更多整理背景见[飞书交付报告](https://ecnurcfxtmqo.feishu.cn/wiki/VwQuwMaCii6d2fkbqvWcwbTDnle)。

## 在线访问与本地运行

直接打开[在线版本](https://shanhaijing-beasts-visualization.vercel.app)，无需安装任何软件。

需要本地查看时，可以直接用浏览器打开 `index.html`，或在项目目录运行：

```powershell
python -m http.server 4173
```

然后访问 `http://localhost:4173/`。

## 技术特点

- 原生 HTML、CSS、JavaScript，无第三方依赖。
- 数据与交互集中在单文件中，无构建步骤。
- 页面不依赖外部字体、图片或接口，离线打开仍可使用。
- 响应式布局覆盖桌面端与移动端。

## 项目结构

```text
.
├── index.html                                  # 线上部署入口
├── 山海经神兽可视化.html                        # 原始中文命名版本
├── README.md                                   # 项目介绍
├── LICENSE                                     # 原创代码的 MIT License
├── CLAUDE.md                                   # 项目开发与部署约定
└── docs/
    ├── images/project-preview.png              # README 页面预览图
    └── superpowers/                            # 设计与实施记录
```

## 部署与维护

项目是无构建步骤的静态站点，Vercel 输出目录为项目根目录。合并到 `main` 后，推送会触发 Vercel 自动部署：

```powershell
git status --short --branch
git push origin main
```

需要手动部署时，必须显式指定项目目录：

```powershell
npx vercel --cwd "E:\Project\《山海经》神兽可视化图谱" --prod --yes
```

维护资料：

- [飞书交付报告](https://ecnurcfxtmqo.feishu.cn/wiki/VwQuwMaCii6d2fkbqvWcwbTDnle)
- [飞书更新手册](https://ecnurcfxtmqo.feishu.cn/wiki/IMaowmEE0iuzUykSUaMc94pEncb)

## 许可证与内容说明

除非文件中另有说明，本仓库的原创代码采用 [MIT License](LICENSE)，版权所有 © 2026 ZSLPZDZH。

《山海经》原典为古代典籍。本项目中的现代整理数据、解释文字与视觉呈现可能包含独立来源或整理成果，不因代码采用 MIT License 而自动获得相同授权；引用或再利用时请分别核对来源与权利边界。
