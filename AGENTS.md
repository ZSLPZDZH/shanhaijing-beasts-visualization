# Codex 项目级说明

开始任何工作前，先读取并遵守 [CLAUDE.md](CLAUDE.md) 中的平台共享项目规范。本文件只补充 Codex 专属的工作流约束，不复制项目事实。

- 功能和文档改动使用独立分支，通过 Pull Request 合并到 `main`；不要直接在 `main` 上提交或推送。
- `git push` 和生产部署必须获得用户明确授权。
- `.env.local`、`.vercel/` 和 `docs/manual-github-vercel-update.xml` 只留在本机，不提交到仓库。
- `docs/` 只保留对使用者或维护者仍有价值的资料；已完成的临时实施计划及时删除。
