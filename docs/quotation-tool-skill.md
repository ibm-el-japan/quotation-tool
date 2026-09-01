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

It is managed in the team GitHub repository:

- **チーム共有リポジトリ（正式）:** https://github.com/ibm-el-japan/quotation-tool (Public)
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
| Release Version | **v1.4.0** |
| Release Date | 2025-09-01 |
| File | `quotation-tool.html` (~2500 lines) |
| Save format version | 4 (includes `prevState` for diff tracking; `kosuTrips`/`kosuNights` removed) |

### Key features in v1.4.0

- **6 tabs**: 基本情報 / チェックシート / 工数 / 交通費等 / 作成・出力 / バージョン情報

- **🔍 変更履歴ビュー（差分表示）**
  - ファイルを開いた際に前回保存時との差分をモーダルで自動表示
  - 基本情報・工数・交通費・チェックシートの各シートごとに追加/削除/変更を色分け表示
  - `prevState` フィールドを保存データに埋め込み、次回開封時の比較基準として使用
  - ファイルロード後に `window.__SAVED_STATE__` を更新 → 次回保存で正しく prevState が設定される
  - `prevState` の中の `prevState` は除去して保存（無限ネスト防止）

- **GitHub連携機能**
  - 「💾 保存 → GitHubへ」: ダウンロード後に GitHub の work/ フォルダへ手動アップロードをガイド
  - 「☁️ API保存」(管理者モード): PAT を使って GitHub API で直接コミット
  - 「📂 GitHubから開く」: `work/` 一覧をAPIで取得し、クリックで読み込み
  - PAT は `sessionStorage` 保持（ページを閉じると自動削除）

- **ユーザー登録フロー（一般ユーザー／管理者を分離）**
  - **一般ユーザー**: バージョン情報タブに「🙋 GitHub アカウント登録申請（ステップ1〜4）」ガイドを表示
    - ステップ1: GitHub アカウント作成（signup URL リンク付き）
    - ステップ2: 「📧 管理者へ招待を申請する」ボタン（メール下書きを自動生成）
    - ステップ3: 招待承認後の PAT 取得手順（6ステップ、スコープ設定まで詳述）
    - ステップ4: ツールでの PAT 使い方
  - **管理者専用**: 管理者モードでのみ表示される「🔑【管理者専用】ユーザー登録・権限管理」カード
    - 一般ユーザー（Write権限）の登録手順
    - 管理者（Admin権限）の登録手順 + パスワード別途伝達の注意喚起
    - 権限一覧表（GitHub ロール × ツール内モード対応）
    - コラボレーター管理・コミット履歴への直リンク

- **管理者モード（🔑ボタン）**
  - ヘッダー右上の 🔑 ボタン → パスワード入力
  - ログイン時: 管理者バー表示 + 管理者専用カード表示
  - ログアウト時: 管理者バー非表示 + 管理者専用カード非表示
  - 管理者のみ操作可能: チェックシート項目の追加・削除・フォーマット変更

- **全ユーザーが編集可能**（v1.4.0 以降）
  - Labor単価・Contingency・GP — フィールドに制限なし
  - 基本情報・工数・交通費等すべて

- 保存/読み込み機能（ローカル保存も引き続き利用可）
- お客様提出用出力・印刷機能

---

## Key Constants in quotation-tool.html

| Constant/ID | Value | Line (approx) |
|---|---|---|
| `APP_VERSION` | `'v1.4.0'` | ~729 |
| `APP_RELEASE_DATE` | `'2025-09-01'` | ~730 |
| `ADMIN_PASSWORD` | `'P@ssw0rd!'` | ~732 |
| `ver-badge` (HTML) | `v1.4.0` | ~207 |
| `GH_REPO_OWNER` | `'ibm-el-japan'` | ~1843 |
| `GH_REPO_NAME` | `'quotation-tool'` | ~1844 |
| `GH_WORK_PATH` | `'work'` | ~1845 |
| `openGhRegisterRequestMail()` | 管理者メールアドレスは関数内 `to` 変数 | ~790 |

---

## Repository Structure

### チーム共有リポジトリ: ibm-el-japan/quotation-tool

```
quotation-tool/                  ← https://github.com/ibm-el-japan/quotation-tool
├── quotation-tool.html          ← チーム向け最新版テンプレート (v1.4.0)
├── releases/
│   ├── quotation-tool_v1.4.0.html
│   ├── quotation-tool_v1.3.0.html
│   ├── quotation-tool_v1.2.0.html
│   └── quotation-tool_v1.1.0.html
├── work/                        ← チームの案件作業ファイル置き場
│   └── README.md
├── docs/
│   └── quotation-tool-skill.md  ← このスキルの GitHub バックアップ（PAT を除いた版）
├── CHANGELOG.md
└── README.md
[gh-pages branch]
├── index.html                   ← チームポータル
└── quotation-tool.html          ← Pages配信用 (v1.4.0)
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
| PAT stored in | `~/.bob/settings/mcp.json` (do NOT commit this value) |
| Authenticated as | `kevinshim-space` |
| Can push to | `ibm-el-japan/quotation-tool` ✅ |
| Cannot push to | `ibmkevin/playground` ❌ (PAT expired) |

> ⚠️ IBM Vault Radar will block any commit containing the raw PAT value. Always redact it before pushing to GitHub.

---

## Team Collaboration Workflow (work/ フォルダ)

### ファイル命名規則
```
[案件略称]_[YYYYMMDD]_v[番号]_[担当者イニシャル].html
例: ABC社_20250901_v1_KS.html
```

### フロー
1. https://ibm-el-japan.github.io/quotation-tool/ を開く（ポータル）
2. 「ツールを開く」ボタンをクリック → ブラウザ上でそのまま動作
3. 入力 → 「💾 保存 → GitHubへ」ボタンでダウンロード → work/ にアップロード
4. 次の担当者が「📂 GitHubから開く」でファイルを選択して継続
5. ファイル開封時に差分モーダルが自動表示 → 変更点をすぐ確認可能

---

## Version Management

### バージョンアップ手順（管理者）

1. `quotation-tool.html` を編集
2. バージョンを更新（4箇所）:
   - `APP_VERSION` 定数
   - `APP_RELEASE_DATE` 定数
   - `ver-badge` span（HTML）
   - バージョン情報タブ内の Release Version テーブル行 + 変更履歴テーブルに新行追加
3. スナップショット作成: `cp quotation-tool.html releases/quotation-tool_vX.Y.Z.html`
4. `CHANGELOG.md` に追記（リリースファイル一覧テーブルも更新）
5. Python urllib スクリプトで `main` と `gh-pages` の両ブランチに push
6. SKILL.md を更新して GitHub に push（PAT 値は除去してから）

### Push パターン（Python urllib — Bob から実行）

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

def get_sha(path, branch="main"):
    url = f"https://api.github.com/repos/{OWNER}/{REPO}/contents/{path}?ref={branch}"
    req = urllib.request.Request(url, headers=headers)
    try:
        with urllib.request.urlopen(req) as r:
            return json.load(r).get("sha")
    except:
        return None

def push_file(gh_path, local_path, message, branch="main"):
    with open(local_path, "rb") as f:
        raw = f.read()
    content_b64 = base64.b64encode(raw).decode("utf-8")
    sha = get_sha(gh_path, branch)
    payload = {"message": message, "content": content_b64, "branch": branch}
    if sha:
        payload["sha"] = sha
    data = json.dumps(payload).encode("utf-8")
    url = f"https://api.github.com/repos/{OWNER}/{REPO}/contents/{gh_path}"
    req = urllib.request.Request(url, data=data, headers=headers, method="PUT")
    with urllib.request.urlopen(req) as r:
        result = json.load(r)
        print(f"✅ [{branch}] {gh_path} → {result['content']['sha'][:8]}")
```

### バージョニングポリシー
| 種別 | 変化 |
|---|---|
| フォーマット変更・機能追加 | マイナー +1 (例: v1.3.0 → v1.4.0) |
| バグ修正・軽微な修正 | パッチ +1 (例: v1.4.0 → v1.4.1) |
| 大規模再設計 | メジャー +1 (例: v1.x.x → v2.0.0) |

---

## Discoveries / Design Decisions

- **Save format v4**: `prevState` (1-level deep, nested `prevState` stripped) embedded in saved HTML — used by next open to compute diff. `kosuTrips`/`kosuNights` removed.
- **Diff flow**: `buildDiffHtml(prev, curr)` computes per-sheet diffs; `showDiffModal(prev, curr, fileName)` renders the modal. Called from `loadProject()` and `execGhLoad()`. Both functions now update `window.__SAVED_STATE__ = state` after restore so subsequent saves correctly capture the loaded file as the prevState baseline.
- **admin-lock removed from HTML fields**: `labor-rate`, `contingency`, `gp` fields have no restrictions — all users can edit. Startup code strips residual `admin-lock` class from old saved files for backward compatibility.
- **Admin mode remaining function**: Only controls visibility of format-editing UI (checklist item add/delete) and the admin-user-mgmt-card. Does NOT restrict any calculation fields.
- **User registration split**: General users self-register via the guide in バージョン情報 tab; admin registration is only visible after admin password login (yellow-bordered card, `id="admin-user-mgmt-card"`, `display:none` by default).
- **Registration request mail**: `openGhRegisterRequestMail()` function opens a mailto: link with pre-filled subject and body. Admin email address is the `to` variable inside that function — update as needed.
- **UTF-8 decode**: `ghGetFileContent` uses `TextDecoder('utf-8')` — fixes Japanese garbling from base64 `atob()`.
- **raw.githubusercontent.com caches aggressively** — always verify at GitHub Pages URL, not raw URL.
- **IBM Vault Radar**: Blocks commits containing raw PAT values. Always redact before pushing SKILL.md or any doc to GitHub.

---

## Known Issues / Follow-up Items

- `ibmkevin/playground` へのプッシュは現在不可（古いPATが失効）。新しい `repo` スコープ付きPATが必要。
- Portal `index.html` (gh-pages) still shows v1.2.0 badge — update if needed.
- `openGhRegisterRequestMail()` の `to` 変数のメールアドレスは現在 `ibmkevin@ibm.com` — 実際の管理者アドレスに変更すること。
- Customer data in `work/` must not contain real customer info (repo is public).

---

## How to Resume Work

1. Open Bob in the playground workspace (`/Users/kevinshim/.bob/playground`).
2. Activate this skill: ask about the quotation tool or mention `quotation-tool.html`.
3. Read `quotation-tool.html` for current state if needed.
4. Push changes using the Python urllib script above with PAT from `~/.bob/settings/mcp.json`.

---

## How to Update This Skill

**Live skill location** (Bob reads this directly):
```
~/.bob/skills/quotation-tool/SKILL.md
```

**GitHub backup** (PAT-redacted copy — for review and editing):
```
https://github.com/ibm-el-japan/quotation-tool/blob/main/docs/quotation-tool-skill.md
```

**To update the GitHub backup after editing the live skill:**
```python
# In Bob, run this Python snippet (replace PAT):
import urllib.request, json, base64

# Redact PAT before writing to disk
with open('/Users/kevinshim/.bob/skills/quotation-tool/SKILL.md') as f:
    content = f.read()
content = content.replace('<actual PAT value>', '<PAT — see ~/.bob/settings/mcp.json>')

with open('/Users/kevinshim/.bob/playground/docs/quotation-tool-skill.md', 'w') as f:
    f.write(content)

# Then push docs/quotation-tool-skill.md via the urllib push_file() function above
```

**To edit the skill on GitHub and pull it back to Bob:**
1. Edit `docs/quotation-tool-skill.md` on GitHub
2. Copy content back to `~/.bob/skills/quotation-tool/SKILL.md` (add PAT value back if needed)
