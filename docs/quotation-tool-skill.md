---
name: quotation-tool
description: Use when the user asks about the quotation tool (見積もり作成ツール), CostPlanSheet2026.xlsm, QuoteOutput.xlsx, or any work on quotation-tool.html — provides full project context, architecture decisions, current state, and known issues.
---

# Quotation Tool — Project Context

## Overview

A browser-based quotation generation tool (`quotation-tool.html`) that runs entirely in the client
(no backend). The user fills in project details and the tool outputs a formatted quote.

The file lives in the Bob playground workspace:

```
/Users/kevinshim/.bob/playground/quotation-tool.html
```

It is managed across two GitHub repositories:

- **管理者作業リポジトリ（個人）:** https://github.com/ibmkevin/playground (Private — ibmkevin のみ)
- **チーム共有リポジトリ（正式）:** https://github.com/ibm-el-japan/quotation-tool (Public — チームメンバー向け)
- **Branch:** `main`
- **GitHub Organization:** `ibm-el-japan`

---

## GitHub Pages（ワンクリックアクセス）

| 項目 | URL |
|---|---|
| **チームポータル** | https://ibm-el-japan.github.io/quotation-tool/ |
| **ツール直接リンク** | https://ibm-el-japan.github.io/quotation-tool/quotation-tool.html |
| Pages ブランチ | `gh-pages` |
| ステータス | built / 公開中 |

---

## Current Release

| Item | Value |
|---|---|
| Release Version | **v1.1.0** |
| Release Date | 2025-07-11 |
| File | `quotation-tool.html` (1,899 lines) |
| Save format version | 3 (includes `kosuTrips`, `kosuNights`) |

### Key features in v1.1.0
- 6 tabs: 基本情報 / チェックシート / 工数 / 交通費等 / 作成・出力 / バージョン情報
- 工数シートに移動回数・宿泊数列 → 交通費等タブへ自動連動
- 管理者モード（🔑ボタン）: Labor単価・Contingency・GP は管理者のみ編集可
- バージョン情報タブ: リリース情報・変更履歴・GitHub権限ガイドをツール内に組み込み
- 保存/読み込み機能（案件データを .html ファイルとして保存・復元）
- お客様提出用出力・印刷機能

---

## Repository Structure

### 管理者作業リポジトリ: ibmkevin/playground

```
playground/
├── quotation-tool.html          ← 最新版（管理者の作業コピー）
├── releases/
│   └── quotation-tool_v1.1.0.html
├── docs/
│   └── quotation-tool-skill.md  ← このスキルのGitHubバックアップ
├── CHANGELOG.md
└── README.md
```

### チーム共有リポジトリ: ibm-el-japan/quotation-tool

```
quotation-tool/                  ← https://github.com/ibm-el-japan/quotation-tool
├── quotation-tool.html          ← チーム向け最新版テンプレート
├── releases/
│   └── quotation-tool_v1.1.0.html
├── work/                        ← チームの案件作業ファイル置き場
│   └── README.md
├── docs/
│   └── quotation-tool-skill.md  ← スキルバックアップ
├── WORK_LOG.md                  ← チーム変更履歴ログ
├── CHANGELOG.md
└── README.md
[gh-pages branch]
├── index.html                   ← チームポータル
└── quotation-tool.html          ← Pages配信用
```

---

## GitHub Access & Permissions

### Organization: ibm-el-japan

| ロール | 対象 | 操作可能な範囲 |
|---|---|---|
| Owner / Admin | ibmkevin（管理者） | 全設定・メンバー管理・全ファイル変更 |
| Write | チームメンバー（担当SE等） | ファイル閲覧・ダウンロード・work/へのアップロード |
| Read | 参照専用メンバー | ファイル閲覧・ダウンロードのみ |

### メンバー招待
https://github.com/orgs/ibm-el-japan/people または
https://github.com/ibm-el-japan/quotation-tool/settings/access

### ツール内の権限モード

| モード | アクセス方法 | 追加で操作可能な項目 |
|---|---|---|
| 一般ユーザー（デフォルト） | 起動時 | お客様名・案件名・工数・チェックシート等 |
| 管理者モード | ヘッダー右上 🔑 ボタン → パスワード入力 | Labor単価・Contingency・GP |

管理者パスワード: `quotation-tool.html` 内 `ADMIN_PASSWORD` 定数（約537行目）で管理。

---

## Team Collaboration Workflow (work/ フォルダ)

### ファイル命名規則
```
[案件略称]_[YYYYMMDD]_v[番号]_[担当者イニシャル].html
例: ABC社_20250711_v1_KS.html
```

### フロー
1. https://ibm-el-japan.github.io/quotation-tool/ を開く（ポータル）
2. 「ツールを開く」ボタンをクリック → ブラウザ上でそのまま動作
3. 入力 → 💾 保存 → 命名規則でリネーム
4. `work/` フォルダにアップロード（GitHub上: Add file → Upload files）
5. `WORK_LOG.md` に変更内容を追記（✏️ Edit → Commit）
6. 次の担当者が `work/` の最新ファイルを取得して継続

### 変更履歴の確認
- **WORK_LOG.md**: https://github.com/ibm-el-japan/quotation-tool/blob/main/WORK_LOG.md
- **Git commit履歴**: https://github.com/ibm-el-japan/quotation-tool/commits/main

---

## Environment Setup

### MCP Servers (global — `~/.bob/settings/mcp.json`)

| Server | Purpose |
|---|---|
| `pdf-reader` | Reads PDF files via a local Node.js binary |
| `askibm` | IBM internal Q&A via uvx |
| `xlsm-reader` | Reads `.xlsm` / `.xlsx` workbooks via a local Node.js binary |
| `github` | GitHub MCP server (`@modelcontextprotocol/server-github`) |

### PAT 管理

| PAT | 認証ユーザー | 用途 |
|---|---|---|
| `ghp_****`（`~/.bob/settings/mcp.json` 参照） | kevinshim-space | `ibm-el-japan/quotation-tool` への書き込み |
| Classic PAT（旧）| ibmkevin | 失効 — `ibmkevin/playground` への書き込みは現在不可 |

> ⚠️ `ibmkevin/playground` へのプッシュには別途 `repo` スコープを持つ PAT が必要。

---

## Version Management

### バージョンアップ手順（管理者）
```bash
# 1. quotation-tool.html を編集（APP_VERSION, ver-badge, CHANGELOGタブ内も更新）

# 2. スナップショット作成
cp quotation-tool.html releases/quotation-tool_vX.Y.Z.html

# 3. CHANGELOG.md に追記

# 4. ibm-el-japan/quotation-tool へプッシュ（temp-dir パターン）
PAT="<token>"
TMPDIR=$(mktemp -d) && cd "$TMPDIR"
git init && git checkout -b main
git remote add origin "https://ibm-el-japan:<PAT>@github.com/ibm-el-japan/quotation-tool.git"
git fetch origin main --quiet && git reset --hard origin/main
# ファイルをコピー後:
git add . && git commit -m "release: vX.Y.Z — [変更概要]" && git push origin main

# 5. gh-pages ブランチも更新（同じパターンで gh-pages ブランチへ）
```

### バージョニングポリシー
| 種別 | 変化 |
|---|---|
| フォーマット変更・機能追加 | マイナー +1 (例: v1.1.0 → v1.2.0) |
| バグ修正・軽微な修正 | パッチ +1 (例: v1.1.0 → v1.1.1) |
| 大規模再設計 | メジャー +1 (例: v1.x.x → v2.0.0) |

---

## Known Issues / Follow-up Items

- `ibmkevin/playground` リポジトリへの git push が現在不可（旧PATが失効）。
  新しい `repo` スコープ付き Classic PAT を生成し以下を実行:
  ```bash
  git remote set-url origin "https://ibmkevin:<NEW_PAT>@github.com/ibmkevin/playground.git"
  git push
  ```

- 現在の `mcp.json` の PAT は `kevinshim-space` ユーザーとして認証される。
  `ibm-el-japan/quotation-tool` への書き込みはこの PAT で可能。

---

## How to Resume Work

1. Open Bob in the playground workspace (`/Users/kevinshim/.bob/playground`).
2. Activate this skill by typing `/quotation-tool` or asking about the quotation tool.
3. Read `quotation-tool.html` (currently v1.1.0, 1899 lines).
4. Push changes to `ibm-el-japan/quotation-tool` using the current PAT (temp-dir pattern).

---

## How to Update This Skill

Live skill location (Bob reads this):
```
~/.bob/skills/quotation-tool/SKILL.md
```

GitHub backup (review/edit here):
```
https://github.com/ibm-el-japan/quotation-tool/blob/main/docs/quotation-tool-skill.md
```

Sync after local edit:
```bash
cp ~/.bob/skills/quotation-tool/SKILL.md \
   /Users/kevinshim/.bob/playground/docs/quotation-tool-skill.md
# Push to ibm-el-japan/quotation-tool via temp-dir pattern
```

Sync after GitHub edit:
```bash
# Download updated file from GitHub, then:
cp quotation-tool-skill.md ~/.bob/skills/quotation-tool/SKILL.md
```
