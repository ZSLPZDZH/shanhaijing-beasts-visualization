# 《山海经》神兽可视化图谱

一个单文件静态网页，用分类、卷次分布、搜索与详情弹窗展示《山海经》神兽名录。

## 预览

- Vercel：https://shanhaijing-beasts-visualization.vercel.app
- GitHub：https://github.com/ZSLPZDZH/shanhaijing-beasts-visualization
- 飞书交付报告：https://ecnurcfxtmqo.feishu.cn/wiki/VwQuwMaCii6d2fkbqvWcwbTDnle

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
├── index.html                                  # 线上部署入口
├── 山海经神兽可视化.html                        # 原始中文命名版本
├── README.md
├── CLAUDE.md
└── docs/superpowers/plans/2026-07-09-deploy-github-vercel.md
```

## 部署

这是无构建步骤的静态站点，Vercel 输出目录为项目根目录。

```powershell
# 查看 Git 状态
git status --short --branch

# 推送到 GitHub，触发 Vercel 自动部署
git push

# 需要手动部署时，显式指定项目目录
npx vercel --cwd "E:\Project\《山海经》神兽可视化图谱" --prod --yes
```

## 验证

```powershell
curl.exe -L -s -o NUL -w "%{http_code}" "https://shanhaijing-beasts-visualization.vercel.app"
```

预期输出：`200`。

## 注意事项

- 不要在 `C:\Users\LPZ` 等非项目目录直接运行 `git init` 或 `npx vercel --prod`。
- Vercel CLI 若提示正在部署 home directory，立即停止，改用 `--cwd` 指向项目目录。
- `.env.local` 与 `.vercel/` 是本地部署元数据，必须保持 Git 忽略。
