# AGENTS.md — resume-wiki 工作手册

## 这是什么项目
面试**八股 / 面经**的学习库。核心是一个"给 LLM 读"的知识库 `wiki/`（llm-wiki），围绕它做 ingest / question / query 的学习闭环。

## 路由：先读 skill
当用户发来八股题、面经、知识点，或要求"消化 / ingest"、"考我 / question / 出题"、"查 wiki / query"时，**先读并严格遵循 `skill/resume/SKILL.md`**。具体流程都在那里，本文件不重复。

> 注意：本仓库的 skill 放在仓库内、不走全局 `~/.qoderwork/skills/`，**不会被自动触发**。需要时由本手册引导 agent 主动去读 `skill/resume/SKILL.md`。

## wiki 红线
- 每个知识点是一个**自包含**的 md 文件，路径 `wiki/<topic>/<slug>.md`，slug 用小写英文+连字符。
- 正文**中文**为主，术语 / API / 代码保留英文。
- 面向 LLM：信息密度与可检索性优先，不追求给人看的排版。
- **任何** wiki 内容改动后，必须同步更新 `wiki/index.md`（条目 + 一句话摘要 + updated 日期）。

## 自动 push 规则
**每次更新 `wiki/`（含 `wiki/index.md`）后，必须自动提交并推送到远端**，无需用户额外要求：

    git add -A && git commit -m "wiki: <本次改动的知识点/摘要>" && git push

- commit message 用中文简述本次消化/修改了哪些知识点。
- 若 push 失败（如网络/认证），如实告知用户，不要谎报成功。

## 别碰
- `.obsidian/` —— Obsidian 配置，不要修改或删除。
- `.codex/skills/resume`、`.qoder/skills/resume` —— 指向 `skill/resume` 的软链，勿改。

## 通用
- 用中文与用户交流。
- 删除文件走废纸篓，不做永久删除；改动前对非版本控制文件先备份（本仓库已在 git 下，改动可回溯）。
