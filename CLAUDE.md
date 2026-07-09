# 《山海经》神兽可视化图谱

## 项目性质

这是一个单文件静态站点，用 HTML/CSS/JavaScript 展示《山海经》神兽可视化图谱。

## 目录约定

- `index.html`：线上部署入口，Vercel 与静态服务器默认读取此文件。
- `山海经神兽可视化.html`：原始中文命名版本，保留作源文件备份。
- `README.md`：项目说明、预览与部署信息。
- `docs/`：项目计划、记录与后续文档。

## 开发约定

- 不引入构建工具，除非明确需要第二个页面或模块化维护。
- 不添加第三方依赖；当前页面应保持可直接在浏览器打开。
- 修改页面内容时，同步更新 `index.html`；如继续保留中文源文件，也应保持两者一致。
- 用户体验优先：页面必须能在桌面与移动端直接访问，加载失败时不得依赖外部资源。

## 验证命令

```powershell
# 本地静态预览
python -m http.server 4173

# Git 状态
git status --short --branch

# Vercel 生产部署
npx vercel --prod --yes
```

## 部署

- GitHub 仓库：`shanhaijing-beasts-visualization`
- Vercel：通过 GitHub 仓库或 Vercel CLI 部署。
- 部署后在 `README.md` 填入线上 URL。
