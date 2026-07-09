# 《山海经》神兽可视化图谱 Deployment Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Publish the existing single-file static visualization site to a public GitHub repository and Vercel production URL.

**Architecture:** Keep the site as a no-build static HTML project. Add a deployable `index.html` entrypoint copied from the existing Chinese-named HTML source, initialize Git on `main`, push to GitHub repo `shanhaijing-beasts-visualization`, then deploy the repository to Vercel.

**Tech Stack:** Static HTML/CSS/JavaScript, Git, GitHub CLI (`gh`), Vercel CLI via `npx vercel`.

---

## Context

The project at `E:\Project\《山海经》神兽可视化图谱` currently contains only `山海经神兽可视化.html` and is not a Git repository. The user chose a public GitHub repository and approved the recommended repository name `shanhaijing-beasts-visualization`. The intended outcome is a shareable Vercel production URL backed by a GitHub repo so future updates can be committed and pushed normally.

Because this publishes content externally, execution should stop for user action only if GitHub or Vercel authentication is missing or an interactive browser/login step is required.

## Files to create or modify

- Create `E:\Project\《山海经》神兽可视化图谱\CLAUDE.md` — project-level rules, deployment commands, and structure constraints.
- Create `E:\Project\《山海经》神兽可视化图谱\index.html` — Vercel/GitHub static entrypoint, copied exactly from `山海经神兽可视化.html`.
- Create `E:\Project\《山海经》神兽可视化图谱\.gitignore` — small static-site ignore rules.
- Create `E:\Project\《山海经》神兽可视化图谱\README.md` — project summary, local preview, deployment links placeholder to update after deployment.
- Create `E:\Project\《山海经》神兽可视化图谱\docs\superpowers\plans\2026-07-09-deploy-github-vercel.md` — this implementation plan for future handoff.
- Git metadata `.git` will be created by `git init`.

## Task 1: Add project rules and deployment-ready static entrypoint

**Files:**
- Create: `CLAUDE.md`
- Create: `.gitignore`
- Create: `README.md`
- Create: `index.html`
- Preserve: `山海经神兽可视化.html`

- [ ] **Step 1: Create project-level `CLAUDE.md`**

Write this content to `E:\Project\《山海经》神兽可视化图谱\CLAUDE.md`:

```markdown
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
```

- [ ] **Step 2: Create `.gitignore`**

Write this content to `E:\Project\《山海经》神兽可视化图谱\.gitignore`:

```gitignore
# OS/editor noise
.DS_Store
Thumbs.db
.vscode/
.idea/

# Local environment files
.env
.env.*

# Vercel local metadata
.vercel/

# Logs
*.log
```

- [ ] **Step 3: Create `README.md` before deployment**

Write this content to `E:\Project\《山海经》神兽可视化图谱\README.md`:

```markdown
# 《山海经》神兽可视化图谱

一个单文件静态网页，用分类、卷次分布、搜索与详情弹窗展示《山海经》神兽名录。

## 预览

部署完成后在此补充 Vercel 链接。

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
```

- [ ] **Step 4: Create deploy entrypoint**

Copy `E:\Project\《山海经》神兽可视化图谱\山海经神兽可视化.html` to `E:\Project\《山海经》神兽可视化图谱\index.html` without changing content.

PowerShell command:

```powershell
Copy-Item -LiteralPath "E:\Project\《山海经》神兽可视化图谱\山海经神兽可视化.html" -Destination "E:\Project\《山海经》神兽可视化图谱\index.html"
```

Expected: `index.html` exists and has the same title `《山海经》神兽图谱 · 可视化`.

- [ ] **Step 5: Commit Task 1**

Run from `E:\Project\《山海经》神兽可视化图谱`:

```powershell
git init
git branch -M main
git add CLAUDE.md .gitignore README.md index.html "山海经神兽可视化.html"
git commit -m "chore(project): initialize static site"
```

Expected: Git creates the first commit on `main`.

## Task 2: Save the implementation plan in the project

**Files:**
- Create: `docs/superpowers/plans/2026-07-09-deploy-github-vercel.md`

- [ ] **Step 1: Create plan directory and write this plan**

Create `E:\Project\《山海经》神兽可视化图谱\docs\superpowers\plans\2026-07-09-deploy-github-vercel.md` containing this plan.

PowerShell command shape:

```powershell
New-Item -ItemType Directory -Force "E:\Project\《山海经》神兽可视化图谱\docs\superpowers\plans"
```

Expected: directory exists.

- [ ] **Step 2: Commit Task 2**

Run:

```powershell
git add docs/superpowers/plans/2026-07-09-deploy-github-vercel.md
git commit -m "docs(deploy): add github vercel deployment plan"
```

Expected: second commit records the plan.

## Task 3: Create public GitHub repository and push

**Files:**
- No file content changes expected.
- Git remote `origin` will be added.

- [ ] **Step 1: Check GitHub CLI authentication**

Run:

```powershell
gh auth status
```

Expected: authenticated to GitHub with permission to create repositories. If not authenticated, stop and ask the user to run:

```text
! gh auth login
```

- [ ] **Step 2: Create public repo and push**

Run from project root:

```powershell
gh repo create shanhaijing-beasts-visualization --public --source . --remote origin --push
```

Expected: GitHub repo is created, `origin` is set, and `main` is pushed.

- [ ] **Step 3: Verify GitHub remote**

Run:

```powershell
git remote -v
git status --short --branch
```

Expected: `origin` points to the new GitHub repository and branch is clean/up to date.

## Task 4: Deploy to Vercel production

**Files:**
- May create `.vercel/` locally; it is ignored by Git.
- Modify: `README.md` after final Vercel URL is known.

- [ ] **Step 1: Check Vercel CLI access**

Run:

```powershell
npx vercel --version
```

Expected: Vercel CLI prints a version. If npm asks to install `vercel`, allow the npx prompt only if user approval mode permits; otherwise ask the user.

- [ ] **Step 2: Deploy production site**

Run from project root:

```powershell
npx vercel --prod --yes
```

Expected: Vercel deploys the static site and returns a production URL. If login is required, stop and ask the user to run:

```text
! npx vercel login
```

- [ ] **Step 3: Connect Vercel project to GitHub if CLI did not bind automatically**

If deployment succeeds but GitHub auto-deploy is not connected, run:

```powershell
npx vercel git connect "https://github.com/$(gh api user --jq .login)/shanhaijing-beasts-visualization"
```

Expected: Vercel reports Git connected. If it reports `You need to add a Login Connection to your GitHub account first`, use the known workaround: open the Vercel project's Git Settings page in the browser, click `Continue with GitHub`, complete GitHub App authorization, then rerun the command.

## Task 5: Verify deployment end-to-end and update README

**Files:**
- Modify: `README.md`

- [ ] **Step 1: Verify the production URL returns HTTP 200**

Run:

```powershell
curl.exe -L -s -o NUL -w "%{http_code}" "<VERCEL_PRODUCTION_URL>"
```

Expected: `200`.

- [ ] **Step 2: Verify page title through fetched HTML**

Run:

```powershell
$html = (Invoke-WebRequest -Uri "<VERCEL_PRODUCTION_URL>" -UseBasicParsing -TimeoutSec 20).Content
if ($html -match '<title>《山海经》神兽图谱 · 可视化</title>') { 'title-ok' } else { 'title-missing' }
```

Expected: `title-ok`.

- [ ] **Step 3: Update README with links**

Replace the preview section in `README.md` with:

```markdown
## 预览

- Vercel：<VERCEL_PRODUCTION_URL>
- GitHub：https://github.com/<GITHUB_USERNAME>/shanhaijing-beasts-visualization
```

- [ ] **Step 4: Commit README link update**

Run:

```powershell
git add README.md
git commit -m "docs(readme): add deployment links"
git push
```

Expected: GitHub receives the README update and Vercel may trigger another deployment.

- [ ] **Step 5: Final status check**

Run:

```powershell
git status --short --branch
```

Expected: clean working tree on `main`, tracking `origin/main`.

## Verification

End-to-end verification is complete only when all of these are true:

1. `git status --short --branch` shows a clean working tree.
2. GitHub repository `shanhaijing-beasts-visualization` is public and contains `index.html`, `README.md`, `CLAUDE.md`, `.gitignore`, original Chinese HTML file, and this plan.
3. Vercel production URL returns HTTP `200`.
4. Fetched production HTML contains `<title>《山海经》神兽图谱 · 可视化</title>`.
5. `README.md` contains both GitHub and Vercel links.

## Self-review notes

- No build system is introduced because the site is a single static HTML file.
- No environment variables or secrets are required.
- The original Chinese-named HTML file is preserved to avoid destructive renaming.
- `index.html` is added because Vercel/static hosting expects a conventional entrypoint.
