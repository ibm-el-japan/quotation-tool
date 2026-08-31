# Change History — 見積もり作成ツール

All notable changes to `quotation-tool.html` are documented here.

**Repository:** https://github.com/ibm-el-japan/quotation-tool  
**Portal:** https://ibm-el-japan.github.io/quotation-tool/

---

## [v1.1.0] — 2025-07-11

### 機能追加 (Added)
- 工数シートに **移動回数** / **宿泊数** 列を追加
- 工数シートの移動回数・宿泊数の合計が交通費等タブの対応行へ**自動連動**
- **管理者モード** 実装（🔑ボタン）
  - 一般ユーザー: お客様名・案件名・eDOU設定・工数・交通費・出力生成のみ操作可
  - 管理者モード: Labor単価・Contingency・GP・フォーマット変更が可能
- **バージョン情報 / 変更履歴タブ** 追加（ツール内で閲覧可能）
- GitHub コラボレーター設定・権限管理ガイドをツール内に組み込み
- バージョンバッジをヘッダーに表示（`v1.1.0`）
- 保存データに `appVersion` フィールドを追加（保存形式 version: 3）
- **GitHub Pages** 設定（https://ibm-el-japan.github.io/quotation-tool/）
- **チームポータル** (index.html) 追加 — ワンクリックでツールにアクセス可能
- **work/** フォルダ追加 — チームの案件作業ファイル共有場所
- **WORK_LOG.md** 追加 — 誰が・いつ・何を変更したか記録する変更履歴ログ
- Organization `ibm-el-japan` へ移行（旧: `ibmkevin/quotation-tool`）

### 変更 (Changed)
- ヘッダーのバージョン表示を `CostPlanSheet2026 対応版` → `v1.1.0` に更新
- 工数サマリーバーに「移動回数（合計）」「宿泊数（合計）」を追加表示
- `TRAVEL_DEFAULT` に `linkedKosuField` プロパティを追加（工数連動フラグ）

### 変更者
- ibmkevin

---

## [v1.0.0] — 2025-07-01

### 初版リリース (Initial Release)
- 基本情報・チェックシート・工数・交通費等・作成出力の **5タブ**構成
- CostPlanSheet2026.xlsm 対応の価格計算ロジック実装
- **保存/読み込み機能**: プロジェクトデータを `.html` ファイルとして保存・復元
- **お客様提出用出力**: 前提シート + 見積もりシートを印刷/PDF 出力対応

### 変更者
- ibmkevin

---

## リリースファイル一覧

| Version | ファイル | リリース日 |
|---|---|---|
| v1.1.0 | `releases/quotation-tool_v1.1.0.html` | 2025-07-11 |
| v1.0.0 | *(初版 — GitHub コミット履歴参照)* | 2025-07-01 |

---

## バージョニングポリシー

| 種別 | バージョン変化 |
|---|---|
| フォーマット変更・機能追加 | マイナー +1 (v1.1.0 → v1.2.0) |
| バグ修正・軽微な修正 | パッチ +1 (v1.1.0 → v1.1.1) |
| 大規模な再設計 | メジャー +1 (v1.x.x → v2.0.0) |
