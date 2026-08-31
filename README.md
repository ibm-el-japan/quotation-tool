# 見積もり作成ツール (Quotation Tool)

**Current Release: v1.1.0** | **Organization: ibm-el-japan**

ブラウザ上で動作するスタンドアロンの概算見積もり作成ツールです。  
インストール不要。下記のポータルリンクからクリックするだけで使用できます。

---

## 🚀 ワンクリックアクセス

| 項目 | URL |
|---|---|
| **チームポータル（ここから開始）** | https://ibm-el-japan.github.io/quotation-tool/ |
| **ツール直接リンク** | https://ibm-el-japan.github.io/quotation-tool/quotation-tool.html |
| **GitHub リポジトリ** | https://github.com/ibm-el-japan/quotation-tool |

---

## ファイル構成

```
quotation-tool/
├── quotation-tool.html              ← 最新版フォーマット（常にここが正式版）
├── releases/
│   └── quotation-tool_v1.1.0.html  ← バージョン付きスナップショット
├── work/                            ← チームの案件作業ファイル置き場
│   └── README.md                    ← 命名規則・フロー説明
├── docs/
│   └── quotation-tool-skill.md     ← Bob スキル / プロジェクトコンテキスト
├── WORK_LOG.md                      ← チーム変更履歴ログ
├── CHANGELOG.md                     ← バージョン変更履歴
└── README.md                        ← このファイル
[gh-pages branch]
├── index.html                       ← チームポータルページ
└── quotation-tool.html              ← Pages配信用ツール本体
```

---

## チームメンバー向け — 使い方

### ▶ ワンクリックで開く（推奨）

👉 **https://ibm-el-japan.github.io/quotation-tool/**

ポータルページの「📋 見積もり作成ツールを開く」ボタンをクリックするだけです。

### ▶ 案件ファイルの保存・再開

- 入力後は **💾 保存** ボタンで案件データを `.html` ファイルとしてダウンロード
- 次回は **📂 読み込み** で案件ファイルを選択して再開
- 作業ファイルは `work/` フォルダにアップロードして共有

> ⚠️ お客様情報を含む案件ファイルは手元で管理してください。機密情報はリポジトリにアップロードしないでください。

---

## GitHub アクセス権限 (Organization: ibm-el-japan)

| ロール | 対象 | 操作可能な範囲 |
|---|---|---|
| **Owner / Admin** | フォーマット管理者 | 全設定・メンバー管理・全ファイル変更 |
| **Write** | チームメンバー（担当SE等） | ファイル閲覧・ダウンロード・work/へのアップロード |
| **Read** | 参照専用メンバー | ファイル閲覧・ダウンロードのみ |

### メンバー招待手順（Admin のみ）

1. https://github.com/ibm-el-japan/quotation-tool/settings/access を開く
2. **Add people** をクリック → GitHub ユーザー名 or メールを入力
3. ロールを選択 → **Add to repository** → 招待メール送信

---

## ツール内の権限モード

| モード | 操作可能な項目 | 制限される項目 |
|---|---|---|
| **一般ユーザー**（デフォルト） | お客様名・案件名・工数・交通費・出力 | Labor単価・Contingency・GP |
| **管理者モード**（🔑ボタン） | 全項目（Labor単価・Contingency・GP含む） | なし |

---

## チーム共同作業フロー（work/ フォルダ）

### ファイル命名規則
```
[案件略称]_[YYYYMMDD]_v[番号]_[担当者イニシャル].html
例: ABC社_20250711_v1_KS.html
```

### フロー
1. ポータル https://ibm-el-japan.github.io/quotation-tool/ を開く
2. ツールを開いて入力 → **💾 保存** → 命名規則でリネーム
3. [work/ フォルダ](https://github.com/ibm-el-japan/quotation-tool/tree/main/work) に Add file → Upload files → Commit
4. [WORK_LOG.md](https://github.com/ibm-el-japan/quotation-tool/blob/main/WORK_LOG.md) に変更内容を追記
5. 次の担当者が `work/` の最新ファイルを取得して継続

---

## フォーマット更新フロー（管理者向け）

```bash
# 1. quotation-tool.html を編集（APP_VERSION, ver-badge, CHANGELOGタブ内も更新）
# 2. スナップショット作成
cp quotation-tool.html releases/quotation-tool_vX.Y.Z.html
# 3. CHANGELOG.md, README.md, WORK_LOG.md を更新
# 4. gh-pages ブランチの quotation-tool.html も更新
# 5. コミット＆プッシュ（main + gh-pages）
```

---

## バージョン履歴

| Version | リリース日 | 主な変更 |
|---|---|---|
| **v1.1.0** | 2025-07-11 | 移動回数/宿泊数連動、管理者モード、バージョン情報タブ追加 |
| v1.0.0 | 2025-07-01 | 初版リリース（5タブ構成、保存/読み込み、お客様提出用出力） |

詳細は [CHANGELOG.md](CHANGELOG.md) を参照してください。

---

## 対応ベース

- IBM CostPlanSheet2026.xlsm
- 動作環境: Chrome / Edge / Safari / Firefox（インターネット接続不要）
