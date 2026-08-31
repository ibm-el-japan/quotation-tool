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
| Release Version | **v1.3.0** |
| Release Date | 2025-09-01 |
| File | `quotation-tool.html` (~2400 lines) |
| Save format version | 4 (includes `prevState` for diff tracking; `kosuTrips`/`kosuNights` removed) |

### Key features in v1.3.0
- 6 tabs: 基本情報 / チェックシート / 工数 / 交通費等 / 作成・出力 / バージョン情報
- **🔍 変更履歴ビュー（差分表示）**: ファイルを開いた際に前回保存時との差分をモーダルで自動表示
  - 基本情報・工数・交通費・チェックシートの各シートごとに追加/削除/変更を色分け表示
  - `prevState` フィールドを保存データに埋め込み、次回開封時の比較基準として使用
- **GitHub連携機能**: ブラウザから直接 `work/` フォルダにコミット
  - 「☁️ GitHubへ保存」: 担当者名・PAT・変更コメントを入力 → GitHub APIで保存
  - 「📂 GitHubから開く」: `work/` 一覧をAPIで取得し、クリックで読み込み
  - PAT は `sessionStorage` 保持（ページを閉じると自動削除）
- 管理者モード（🔑ボタン）: Labor単価・Contingency・GP は管理者のみ編集可
  - ※ admin-lock は HTML 要素から削除済み — 起動時コードで旧保存ファイルのクラスも除去
- 保存/読み込み機能（ローカル保存も引き続き利用可）
- お客様提出用出力・印刷機能

---

## Key Constants in quotation-tool.html

| Constant/ID | Value | Line (approx) |
|---|---|---|
| `APP_VERSION` | `'v1.3.0'` | ~618 |
| `APP_RELEASE_DATE` | `'2025-09-01'` | ~619 |
| `ADMIN_PASSWORD` | `'P@ssw0rd!'` | ~621 |
| `ver-badge` | `v1.3.0` | ~207 |
| `GH_REPO_OWNER` | `'ibm-el-japan'` | ~713 |
| `GH_REPO_NAME` | `'quotation-tool'` | ~714 |
| `GH_WORK_PATH` | `'work'` | ~715 |

---

## Repository Structure

### チーム共有リポジトリ: ibm-el-japan/quotation-tool

```
quotation-tool/                  ← https://github.com/ibm-el-japan/quotation-tool
├── quotation-tool.html          ← チーム向け最新版テンプレート (v1.3.0)
├── releases/
│   ├── quotation-tool_v1.3.0.html
│   ├── quotation-tool_v1.2.0.html
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
└── quotation-tool.html          ← Pages配信用 (v1.3.0)
```

---

## GitHub Access & Permissions

### Two GitHub accounts involved

| Account | Role | Repos |
|---|---|---|
| `ibmkevin` | IBM user / org owner | `ibmkevin/playground`, `ibm-el-japan` org owner |
| `kevinshim-space` | Current PAT auth user | Can push to `ibm-el-japan/quotation-tool` ✅ |

### Current PAT

| Key | Value |
|---|---|
| PAT in `~/.bob/settings/mcp.json` | `<PAT — see ~/.bob/settings/mcp.json>` |
| Authenticated as | `kevinshim-space` |
| Can push to | `ibm-el-japan/quotation-tool` ✅ |
| Cannot push to | `ibmkevin/playground` ❌ (PAT expired) |

> ⚠️ Do not commit the PAT value. IBM Vault Radar will block the commit.

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
3. 入力 → 「💾 保存 → GitHubへ」ボタンでwork/に直接コミット
4. 次の担当者が「📂 GitHubから開く」でファイルを選択して継続
5. ファイル開封時に差分モーダルが自動表示 → 変更点をすぐ確認可能

---

## Version Management

### バージョンアップ手順（管理者）

1. `quotation-tool.html` を編集
2. バージョンを更新: `APP_VERSION`, `APP_RELEASE_DATE`, `ver-badge`, バージョン情報タブ内テーブル
3. スナップショット作成: `cp quotation-tool.html releases/quotation-tool_vX.Y.Z.html`
4. `CHANGELOG.md` に追記
5. Python urllib スクリプトで `main` と `gh-pages` の両ブランチに push

### Push パターン（Python urllib）

```python
import urllib.request, json, base64

PAT = "<token from ~/.bob/settings/mcp.json>"
OWNER = "ibm-el-japan"
REPO = "quotation-tool"

headers = {
    "Authorization": f"token {PAT}",
    "Accept": "application/vnd.github.v3+json",
    "Content-Type": "application/json",
    "X-GitHub-Api-Version": "2022-11-28",
}

# Get current SHA first via GET /repos/{owner}/{repo}/contents/{path}?ref={branch}
# Then PUT with sha, content (base64), message, branch
```

### バージョニングポリシー
| 種別 | 変化 |
|---|---|
| フォーマット変更・機能追加 | マイナー +1 (例: v1.2.0 → v1.3.0) |
| バグ修正・軽微な修正 | パッチ +1 (例: v1.3.0 → v1.3.1) |
| 大規模再設計 | メジャー +1 (例: v1.x.x → v2.0.0) |

---

## Discoveries / Design Decisions

- **Save format v4**: `prevState` embedded in saved HTML — used by next open to compute diff. `kosuTrips`/`kosuNights` removed.
- **Diff modal**: `buildDiffHtml(prev, curr)` computes per-sheet diffs; `showDiffModal(prev, curr, fileName)` renders the modal. Called from `loadProject()` and `execGhLoad()`.
- **admin-lock removed from HTML**: Fields no longer have `admin-lock` class. Startup code strips it from old saved files for backward compatibility.
- **UTF-8 decode**: `ghGetFileContent` uses `TextDecoder('utf-8')` — fixes Japanese garbling from base64 atob().
- **raw.githubusercontent.com caches aggressively** — always verify at GitHub Pages URL, not raw URL.
- **GitHub API 403 from browser**: `kevinshim-space` PAT lacks write from browser context for `work/` — resolved by using the download + manual upload workflow or PAT with correct scope.

---

## Known Issues / Follow-up Items

- `ibmkevin/playground` へのプッシュは現在不可（古いPATが失効）。新しい `repo` スコープ付きPATが必要。
- Portal `index.html` (gh-pages) still shows v1.2.0 badge — update if needed.
- Customer data in `work/` must not contain real customer info (repo is public).

---

## How to Resume Work

1. Open Bob in the playground workspace (`/Users/kevinshim/.bob/playground`).
2. Activate this skill: `/quotation-tool` or ask about the quotation tool.
3. Read `quotation-tool.html` for current state.
4. Push changes using Python urllib script with PAT from `~/.bob/settings/mcp.json`.

---

## How to Update This Skill

Live skill location (Bob reads this):
```
~/.bob/skills/quotation-tool/SKILL.md
```

After editing locally, sync to GitHub:
```bash
cp ~/.bob/skills/quotation-tool/SKILL.md \
   /Users/kevinshim/.bob/playground/docs/quotation-tool-skill.md
# then push docs/quotation-tool-skill.md to ibm-el-japan/quotation-tool via urllib script
```
