---
name: git-message
description: >-
  Drafts Git commit messages in Echok API `Sign: Message` convention from staged
  diffs only, optionally formats touched files, and waits for explicit user
  confirmation before `git commit`. Use when the user asks for a commit message,
  help committing, or reviewing staged changes in this repository (aligned with
  `.opencode/commands/git-message.md`).
---

# Git 提交消息（Echok API）

## 何时使用

用户要求：写提交说明、根据暂存区提交、生成 commit message、或遵循仓库内 OpenCode 的 `git-message` 流程时，按本技能执行。

## 前置检查（必做）

1. 用 `git status` 与 `git diff --cached` 确认 **暂存区**。
2. **若暂存区为空**：不要撰写消息、不要提交；直接告知用户先 `git add` 再重试，并结束流程。

## 消息格式

- 单行首选：`Sign: Message`（中文摘要即可，与仓库示例一致）。
- 变更多、需要说明时：**第一行**仍为 `Sign: Message`，**空一行**后写详细说明（可列表、分段）。

### Sign 含义（必选其一）

| Sign | 含义 |
|------|------|
| `New` | 新功能 |
| `Chg` | 改动已有功能 |
| `Enh` | 优化或重构 |
| `Bug` | 修复缺陷 |
| `Doc` | 文档或 PHPDoc / 注释 |

**示例**：`New: 支持 Excel 读取`

## 撰写内容规则

- **只描述已暂存的变更**；未暂存修改不在提交范围内。若工作区与暂存不一致，以 **用户最终暂存结果** 为准。
- **禁止**空洞摘要，例如「修改了 10 个文件」；应概括 **行为或问题**，必要时点出关键文件/模块。
- **New**：若涉及新公共 API 或重要方法，阅读相关 **方法注释**，在正文里体现职责含义。
- **Chg**：若改了方法签名或调用约定，阅读 **参数注释**，说明参数含义或兼容性。
- 基于 **`git diff --cached`** 分析后再写消息，不要凭猜测编造未出现的改动。

## 格式化（提交前）

- **仅**对「本次提交会包含的、且确有改动的文件」做格式化；不要全仓库、不要动无关未改文件。
- 使用本仓库已有约定（如 StyleCI/PhpStorm/项目脚本）；若无统一命令，则跳过格式化并告知用户自行格式化，**不要臆造**不存在的工具名。

## 确认与提交

1. 输出拟定的完整提交消息（含多段正文若需要）。
2. **明确请用户确认** 文案无误后，再执行 `git commit`。
3. **未经用户确认，不得自动 `git commit`**。

## 反例

```text
❌ Chg: 修改了多个文件
❌ fix stuff
✅ Bug: 修复订单列表在空仓库下的分页越界
```
