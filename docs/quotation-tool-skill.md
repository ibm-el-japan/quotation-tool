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

It has been committed to **two** GitHub repositories:

- **管理者リポジトリ（個人作業用）:** https://github.com/ibmkevin/playground (Private — ibmkevin のみ)
- **チーム共有リポジトリ:** https://github.com/ibmkevin/quotation-tool (Private — チームメンバー向け)
- **Branch:** `main`
- **GitHub user:** `ibmkevin`

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

### 管理者リポジトリ: ibmkevin/playground

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

### チーム共有リポジトリ: ibmkevin/quotation-tool

```
quotation-tool/
├── quotation-tool.html              ← チーム向け最新版テンプレート
├── releases/
│   └── quotation-tool_v1.1.0.html  ← バージョン付きスナップショット
├── work/                            ← チームの案件作業ファイル置き場
│   └── README.md                    ← 命名規則・フロー説明
├── WORK_LOG.md                      ← チーム変更履歴ログ
├── CHANGELOG.md
└── README.md
```

---

## GitHub Access & Permissions

### チーム共有リポジトリの権限ロール

| ロール | 対象 | 操作可能な範囲 |
|---|---|---|
| Admin | ibmkevin（管理者） | 全設定・コラボレーター管理・全ファイル変更 |
| Write | チームメンバー（担当SE等） | ファイル閲覧・ダウンロード・work/へのアップロード |
| Read | 参照専用メンバー | ファイル閲覧・ダウンロードのみ |

### コラボレーター招待
Settings → Collaborators and teams → Add people:
https://github.com/ibmkevin/quotation-tool/settings/access

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
1. `quotation-tool.html` を Raw → 保存 → ブラウザで開く
2. 入力 → 💾 保存 → 命名規則でリネーム
3. `work/` フォルダにアップロード（GitHub上: Add file → Upload files）
4. `WORK_LOG.md` に変更内容を追記（✏️ Edit → Commit）
5. 次の担当者が `work/` の最新ファイルを取得して継続

### 変更履歴の確認
- **WORK_LOG.md**: https://github.com/ibmkevin/quotation-tool/blob/main/WORK_LOG.md
- **Git commit履歴**: リポジトリの Commits タブ

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

| PAT | 用途 | リポジトリ |
|---|---|---|
| Fine-grained PAT（`~/.bob/settings/mcp.json` 参照） | 現在の `mcp.json` に設定済み | `quotation-tool` リポジトリへの書き込み |
| Classic PAT（旧）| 失効 / 権限なし | `playground` への書き込みは現在不可 |

> ⚠️ `playground` リポジトリへのプッシュには別途 `repo` スコープを持つ PAT が必要。
> GitHub Settings → Developer settings → Personal access tokens で再生成すること。

---

## Version Management

### バージョンアップ手順（管理者）
```bash
# 1. quotation-tool.html を編集（APP_VERSION, ver-badge, CHANGELOGタブ内も更新）

# 2. スナップショット作成
cp quotation-tool.html releases/quotation-tool_vX.Y.Z.html

# 3. CHANGELOG.md に追記

# 4. playground リポジトリへコミット（PAT更新後）
git add quotation-tool.html releases/ CHANGELOG.md README.md docs/quotation-tool-skill.md
git commit -m "release: vX.Y.Z — [変更概要]"
git push

# 5. quotation-tool リポジトリへも反映（新PAT使用）
```

### バージョニングポリシー
| 種別 | 変化 |
|---|---|
| フォーマット変更・機能追加 | マイナー +1 (例: v1.1.0 → v1.2.0) |
| バグ修正・軽微な修正 | パッチ +1 (例: v1.1.0 → v1.1.1) |
| 大規模再設計 | メジャー +1 (例: v1.x.x → v2.0.0) |

---

## Known Issues / Follow-up Items

- `playground` リポジトリへの git push が現在不可（旧PATが失効）。
  新しい `repo` スコープ付き Classic PAT を生成し、以下を実行:
  ```bash
  git remote set-url origin "https://ibmkevin:<NEW_PAT>@github.com/ibmkevin/playground.git"
  git push
  ```

- `mcp.json` の `GITHUB_PERSONAL_ACCESS_TOKEN` は現在 `quotation-tool` 専用PAT。
  `playground` 用の PAT は別途管理が必要。

- Git `user.name` / `user.email` の初期設定:
  ```bash
  git config --global user.name "Kevin Shim"
  git config --global user.email "kevinshim@jp.ibm.com"
  ```

---

## How to Resume Work

1. Open Bob in the playground workspace (`/Users/kevinshim/.bob/playground`).
2. Activate this skill by typing `/quotation-tool` or asking about the quotation tool.
3. Read `quotation-tool.html` to check the current state (currently v1.1.0, 1899 lines).
4. Make changes, then push to `quotation-tool` repo using the current PAT:
   ```bash
   # Push to quotation-tool repo (current PAT works)
   # Use the temp-dir push pattern (see version management above)
   ```

---

## How to Update This Skill

The live skill Bob reads is at:
```
~/.bob/skills/quotation-tool/SKILL.md
```

The GitHub backup is at:
```
https://github.com/ibmkevin/playground/blob/main/docs/quotation-tool-skill.md
```

To sync after editing locally:
```bash
cp ~/.bob/skills/quotation-tool/SKILL.md \
   /Users/kevinshim/.bob/playground/docs/quotation-tool-skill.md
# then git add / commit / push (requires playground PAT)
```

To sync after editing on GitHub:
```bash
git pull  # in playground directory
cp docs/quotation-tool-skill.md ~/.bob/skills/quotation-tool/SKILL.md
```
