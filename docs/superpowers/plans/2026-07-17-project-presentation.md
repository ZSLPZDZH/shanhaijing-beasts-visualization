# GitHub 项目介绍优化实施计划

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** 把仓库介绍调整为面向普通读者和作品集访客的展示页，并补齐预览图、MIT License 与待发布的 GitHub About 信息。

**Architecture:** 不修改单页应用功能，仅调整仓库外围信息。README 负责访客叙事，`docs/images/project-preview.png` 提供仓库内托管的生产页截图，`LICENSE` 明确原创代码授权；GitHub About、Homepage 和 Topics 通过远端 API 在用户授权发布后更新。

**Tech Stack:** Markdown、MIT License、PNG、PowerShell、原生 HTML/CSS/JavaScript、Git、GitHub CLI

---

## 文件结构

- Modify: `README.md` — 项目定位、核心能力、数据口径、访问方式、维护信息与授权边界。
- Create: `LICENSE` — 原创代码的 MIT License。
- Create: `docs/images/project-preview.png` — Vercel 生产页面桌面端预览图。
- Preserve: `CLAUDE.md` — 保留当前未提交修改，不纳入本计划的实现提交。
- Preserve: `docs/manual-github-vercel-update.xml` — 保留当前未跟踪文件及其内容。

### Task 1: 保护现有工作并确认基线

**Files:**
- Verify: `CLAUDE.md`
- Verify: `README.md`
- Verify: `docs/manual-github-vercel-update.xml`

- [ ] **Step 1: 确认工作分支与工作区状态**

Run:

```powershell
git branch --show-current
git status --short --branch
```

Expected:

```text
codex/improve-project-presentation
```

状态中允许存在实现前已有的 `CLAUDE.md`、`README.md` 修改和 `docs/manual-github-vercel-update.xml` 未跟踪文件。

- [ ] **Step 2: 记录受保护文件的实现前哈希**

Run:

```powershell
Get-FileHash -Algorithm SHA256 -LiteralPath 'CLAUDE.md','docs\manual-github-vercel-update.xml' |
  ForEach-Object { "{0} {1}" -f $_.Hash, $_.Path }
```

Expected hashes:

```text
025D15B898B11380FC4CAD11747325C2F0A597E4BF050D3B415493563081C752 CLAUDE.md
8A05499CD2779015EB09EB81772E523FCD18C065DB9CB8ABECCDB22329FEF112 docs/manual-github-vercel-update.xml
```

### Task 2: 添加生产页面预览图

**Files:**
- Create: `docs/images/project-preview.png`

- [ ] **Step 1: 创建图片目录**

Run:

```powershell
New-Item -ItemType Directory -Path 'docs\images' -Force | Out-Null
```

Expected: `docs/images/` 存在，其他目录不变。

- [ ] **Step 2: 从生产页面采集桌面端截图**

Run the `web-access` dependency check first:

```powershell
node 'C:\Users\LPZ\.agents\skills\web-access\scripts\check-deps.mjs'
```

Expected: output includes `node: ok`, `chrome: ok`, and `proxy: ready`.

Then capture the page:

```powershell
$previewPath = (Resolve-Path 'docs\images').Path + '\project-preview.png'
$newTab = Invoke-RestMethod -Uri 'http://localhost:3456/new?url=https%3A%2F%2Fshanhaijing-beasts-visualization.vercel.app%2F'
$targetId = if ($newTab.targetId) { $newTab.targetId } elseif ($newTab.id) { $newTab.id } else { $newTab.target }
Start-Sleep -Seconds 2
try {
  Invoke-RestMethod -Uri ("http://localhost:3456/screenshot?target={0}&file={1}" -f $targetId, [uri]::EscapeDataString($previewPath))
} finally {
  Invoke-RestMethod -Uri ("http://localhost:3456/close?target={0}" -f $targetId)
}
```

Expected: 页面截图保存为 `docs/images/project-preview.png`，截图中不包含浏览器工具栏或调试界面。

- [ ] **Step 3: 验证 PNG 可读取且尺寸适合作为 README 首图**

Run:

```powershell
Add-Type -AssemblyName System.Drawing
$image = [System.Drawing.Image]::FromFile((Resolve-Path 'docs\images\project-preview.png'))
try {
  [pscustomobject]@{ Width = $image.Width; Height = $image.Height; Format = $image.RawFormat }
  if ($image.Width -lt 1200 -or $image.Height -lt 600) { throw 'Preview image is too small.' }
} finally {
  $image.Dispose()
}
```

Expected: PNG 能读取，宽度至少 1200 像素，高度至少 600 像素。

- [ ] **Step 4: 提交预览图**

```powershell
git add -- 'docs/images/project-preview.png'
git commit -m "docs(readme): add project preview image"
```

Expected: 只提交 `docs/images/project-preview.png`。

### Task 3: 添加 MIT License

**Files:**
- Create: `LICENSE`

- [ ] **Step 1: 创建完整许可证文件**

Create `LICENSE` with exactly:

```text
MIT License

Copyright (c) 2026 ZSLPZDZH

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
```

- [ ] **Step 2: 验证许可证关键字段**

Run:

```powershell
$license = Get-Content -LiteralPath 'LICENSE' -Raw
if (-not $license.StartsWith('MIT License')) { throw 'Missing MIT License title.' }
if ($license -notmatch 'Copyright \(c\) 2026 ZSLPZDZH') { throw 'Incorrect copyright line.' }
if ($license -notmatch 'THE SOFTWARE IS PROVIDED "AS IS"') { throw 'Incomplete MIT License text.' }
```

Expected: 命令无错误退出。

- [ ] **Step 3: 提交许可证**

```powershell
git add -- 'LICENSE'
git commit -m "docs(license): add MIT license"
```

Expected: 只提交 `LICENSE`。

### Task 4: 重写 README 的访客叙事

**Files:**
- Modify: `README.md`

- [ ] **Step 1: 用完整内容替换 README**

Replace `README.md` with exactly:

````markdown
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
    ├── manual-github-vercel-update.xml         # 飞书更新手册源 XML
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
- 本地更新手册源文件：`docs/manual-github-vercel-update.xml`

## 许可证与内容说明

除非文件中另有说明，本仓库的原创代码采用 [MIT License](LICENSE)，版权所有 © 2026 ZSLPZDZH。

《山海经》原典为古代典籍。本项目中的现代整理数据、解释文字与视觉呈现可能包含独立来源或整理成果，不因代码采用 MIT License 而自动获得相同授权；引用或再利用时请分别核对来源与权利边界。
````

- [ ] **Step 2: 验证 README 关键区块与内部资源引用**

Run:

```powershell
$readme = Get-Content -LiteralPath 'README.md' -Encoding utf8 -Raw
$required = @(
  '# 《山海经》神兽可视化图谱',
  '## 一眼看懂',
  '## 可以如何探索',
  '## 数据口径与来源',
  '## 在线访问与本地运行',
  '## 许可证与内容说明',
  'docs/images/project-preview.png',
  '[MIT License](LICENSE)',
  '151 条',
  '152 只',
  '飞书更新手册'
)
$missing = $required | Where-Object { -not $readme.Contains($_) }
if ($missing) { throw "README missing: $($missing -join ', ')" }
if (-not (Test-Path -LiteralPath 'docs\images\project-preview.png')) { throw 'Preview image is missing.' }
if (-not (Test-Path -LiteralPath 'LICENSE')) { throw 'LICENSE is missing.' }
```

Expected: 命令无错误退出。

- [ ] **Step 3: 检查 Markdown 空白与 Git 差异**

Run:

```powershell
git diff --check -- README.md
git diff -- README.md
```

Expected: `git diff --check` 无输出；差异包含新的访客叙事，并保留飞书更新手册链接和部署信息。

- [ ] **Step 4: 提交 README**

```powershell
git add -- 'README.md'
git commit -m "docs(readme): improve project presentation"
```

Expected: 本提交只包含 `README.md`。实现前已有的 README 修改会作为已批准内容整合到新结构中。

### Task 5: 完整验证本地结果

**Files:**
- Verify: `README.md`
- Verify: `LICENSE`
- Verify: `docs/images/project-preview.png`
- Verify: `index.html`

- [ ] **Step 1: 验证受保护文件未被改写**

Run:

```powershell
$expected = @{
  'CLAUDE.md' = '025D15B898B11380FC4CAD11747325C2F0A597E4BF050D3B415493563081C752'
  'docs\manual-github-vercel-update.xml' = '8A05499CD2779015EB09EB81772E523FCD18C065DB9CB8ABECCDB22329FEF112'
}
foreach ($path in $expected.Keys) {
  $actual = (Get-FileHash -Algorithm SHA256 -LiteralPath $path).Hash
  if ($actual -ne $expected[$path]) { throw "Protected file changed: $path" }
}
```

Expected: 命令无错误退出。

- [ ] **Step 2: 验证本地静态站点返回 HTTP 200**

Run:

```powershell
$server = Start-Process -FilePath 'python' -ArgumentList '-m','http.server','4173','--directory',(Get-Location).Path -PassThru -WindowStyle Hidden
try {
  Start-Sleep -Seconds 2
  $status = curl.exe -L -s -o NUL -w "%{http_code}" 'http://localhost:4173/'
  if ($status -ne '200') { throw "Local preview returned HTTP $status" }
} finally {
  Stop-Process -Id $server.Id -ErrorAction SilentlyContinue
}
```

Expected: 首页返回 `200`，启动的服务器进程随后关闭。

- [ ] **Step 3: 验证关键公开链接**

Run:

```powershell
$urls = @(
  'https://shanhaijing-beasts-visualization.vercel.app',
  'https://github.com/ZSLPZDZH/shanhaijing-beasts-visualization',
  'https://ecnurcfxtmqo.feishu.cn/wiki/VwQuwMaCii6d2fkbqvWcwbTDnle',
  'https://ecnurcfxtmqo.feishu.cn/wiki/IMaowmEE0iuzUykSUaMc94pEncb'
)
foreach ($url in $urls) {
  $status = curl.exe -L -s -o NUL -w "%{http_code}" $url
  if ($status -eq '000') { throw "Unreachable URL: $url" }
  "{0} {1}" -f $status, $url
}
```

Expected: 每个地址返回非 `000` 状态；Vercel 与 GitHub 应返回 `200`。

- [ ] **Step 4: 验证分支历史与剩余工作区内容**

Run:

```powershell
git diff --check
git log --oneline --decorate -5
git status --short --branch
```

Expected: 本计划产生的 README、LICENSE 和预览图均已提交；工作区仍只保留实现前已有的 `CLAUDE.md` 修改和 `docs/manual-github-vercel-update.xml` 未跟踪文件。

### Task 6: 等待发布授权后更新 GitHub

**Remote targets:**
- Repository branch: `codex/improve-project-presentation`
- GitHub About description
- GitHub Homepage
- GitHub Topics

- [ ] **Step 1: 获得用户对 `git push` 的明确授权**

Do not run any remote write command until the user explicitly asks to publish or push the branch.

- [ ] **Step 2: 推送独立分支**

Run only after authorization:

```powershell
git push --set-upstream origin codex/improve-project-presentation
```

Expected: 远端创建同名分支，`main` 不被直接修改。

- [ ] **Step 3: 更新 GitHub About 描述与 Homepage**

Run only after authorization:

```powershell
gh api --method PATCH 'repos/ZSLPZDZH/shanhaijing-beasts-visualization' `
  -f 'description=以卷次、吉凶分类与搜索交互探索 151 条《山海经》神兽名录的单页可视化图谱。' `
  -f 'homepage=https://shanhaijing-beasts-visualization.vercel.app'
```

Expected: API 返回的 `description` 和 `homepage` 与设计稿一致。

- [ ] **Step 4: 更新 GitHub Topics**

Run only after authorization:

```powershell
@{
  names = @(
    'shanhaijing',
    'chinese-mythology',
    'data-visualization',
    'interactive-visualization',
    'cultural-heritage',
    'html',
    'css',
    'javascript'
  )
} | ConvertTo-Json -Compress |
  gh api --method PUT 'repos/ZSLPZDZH/shanhaijing-beasts-visualization/topics' --input -
```

Expected: API 返回八个 Topics，名称与设计稿一致。

- [ ] **Step 5: 只读验证远端元数据**

Run:

```powershell
gh api 'repos/ZSLPZDZH/shanhaijing-beasts-visualization' --jq '{description,homepage}'
gh api 'repos/ZSLPZDZH/shanhaijing-beasts-visualization/topics' --jq '.names'
```

Expected: About 描述、Homepage 与八个 Topics 全部可见。
