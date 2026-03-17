# Changelog

## [v0.3.0] - 2026-03-17

2回目の外部レビューを受けて改善。LLMがVaultを直接操作する際の事故防止に焦点を当てた。

### 変更ファイル

- `skills/daily-summary/SKILL.md`
- `commands/daily-summary.md`

### 追加された機能・仕様

#### processed: true の付与タイミング変更
- 従来：タグ付与直後（Step 1.6）に付与
- 変更後：DailySummary生成成功後（Step 2.5）にのみ付与
- 途中失敗時の取りこぼしを防止

#### URL正規化ルール
- 重複判定の前にsource URLを正規化して比較する仕様を追加
- 末尾スラッシュ除去、UTMパラメータ除去、www.除去、https統一
- frontmatterのsource自体は元URLのまま保持

#### タグ更新方針の明文化
- 既存タグを壊さない方針を明記
- daily-summary体系タグのみを追加、手動タグは一切触れない
- 同カテゴリの重複タグは追加しない

#### アーカイブ時のファイル名衝突ルール
- 同名ファイル存在時の挙動を定義
- 同一URL → スキップ、異なるURL → 連番リネーム

#### 失敗ファイルの識別（failure_reason）
- 処理失敗時にfrontmatterへ `failure_reason` と `failure_at` を記録
- 00_Inbox内で未処理と失敗済みを区別可能に
- 次回実行時はリトライ対象として再処理、failure_reasonは自動除去

#### DailySummaryの構造固定
- 固定見出し構造をSKILL.mdに明記
- weekly-summary等の後続集約で機械的にパースできるようにした

#### 同時実行回避
- 基本原則に同時実行禁止を追加
- 1コマンド完了後に次のコマンドを実行する方針

#### 処理対象の固定
- Step 1で処理対象リストを確定し、以降の新規追加は含めない方針を明記

---

## [v0.2.0] - 2026-03-17

外部レビューを受けて、daily-summaryの仕様を大幅に改善。
運用時の安全性・再実行性・追跡性を強化した。

### 変更ファイル

- `skills/daily-summary/SKILL.md`
- `commands/daily-summary.md`

### 追加された機能・仕様

#### 処理状態メタデータ
- 各クリップMDのfrontmatterに `processed: true` / `processed_at` / `summary_date` を追記する仕様を追加
- これにより再実行時に自動で既処理ファイルを除外できるようになった

#### 重複判定ルール
- source URL完全一致 → title + date一致の2段階で重複を検出
- 重複ファイルはスキップし、ログに記録する

#### frontmatter補完の優先順位
- title / source / date / tags の各フィールドについて、欠損時の補完優先順位を明文化
- frontmatter自体がない場合の生成ルールも追加

#### タグ判定基準の具体化
- 各ジャンルタグに「判定基準」カラムを追加
- 迷いやすいケース3例を明示
- 重要度に時間軸基準を追加（高＝今週中、中＝今月中、低＝トレンド把握）
- ソース判定の境界ケースを明記

#### 処理フロー概要
- コマンドファイル冒頭に9ステップのフロー概要を追加

#### ログの充実化
- ファイル別処理結果テーブルを追加
- 成功 / スキップ / 失敗の内訳を記録

#### 再実行の安全性
- 再実行時の動作を明文化（冪等性の保証）
- DailySummary追記時のsource URL重複チェックを追加
- 処理成功ファイルのみ01_Clipsに移動

#### エラー処理の追加
- frontmatter欠損時の対応を追加
- 全件失敗時の対応を追加

---

## [v0.1.0] - 2026-03-17

初回リリース。

### ファイル構成

- `.claude-plugin/plugin.json` - プラグイン定義
- `commands/daily-summary.md` - 日次まとめコマンド
- `commands/weekly-summary.md` - 週次まとめコマンド
- `skills/daily-summary/SKILL.md` - 日次まとめスキル定義
- `skills/weekly-summary/SKILL.md` - 週次まとめスキル定義
- `README.md` - プロジェクト説明

### 機能

- 00_Inboxの記事を読み込み、タグ付け・要約を生成
- 重要度別（高🔴/中🟡/低🟢）の要約スタイル
- 復習クイズ生成（Obsidian SR対応）
- 複数記事の横断まとめ
- DailySummaryへの追記対応
- 01_Clipsへの自動アーカイブ
- 99_Logsへの処理ログ記録
- 週次横断集計レポート生成
