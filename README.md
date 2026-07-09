# 《山海经》神兽可视化图谱

一个单文件静态网页，用分类、卷次分布、搜索与详情弹窗展示《山海经》神兽名录。

## 预览

- Vercel：https://shanhaijing-beasts-visualization.vercel.app
- GitHub：https://github.com/ZSLPZDZH/shanhaijing-beasts-visualization

## 本地查看

直接用浏览器打开 `index.html`，或运行：

```powershell
python -m http.server 4173
```

然后访问：

```text
http://localhost:4173/
```

## 项目结构

```text
.
├── index.html                # 线上部署入口
├── 山海经神兽可视化.html      # 原始中文命名版本
├── README.md
└── CLAUDE.md
```

## 部署

- GitHub 仓库：`shanhaijing-beasts-visualization`
- Vercel：静态站点，无构建命令，输出目录为项目根目录。
