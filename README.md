# obsidian-workflow

Obsidian Vaultの情報収集ワークフローを自動化するCoworkプラグイン。
Web Clipperで収集したMDファイルの整理・要約・日次/週次まとめ生成を行う。

## 概要

Obsidian + Web Clipper + Claude（Cowork）を組み合わせて、日々の情報収集を自動的に整理・要約するワークフロー。

## フォルダ構造

```
Vault/
 ├ 00_Inbox/          ← Web Clipperの吐き出し先
 ├ 01_Clips/          ← 整理済みアーカイブ（YYYYMMDD/単位）
 ├ 02_DailySummary/   ← 日次・週次まとめ
 └ 99_Logs/           ← 処理ログ
```

## コマンド

### `/daily-summary`
- 00_Inboxの記事を読み込み、タグ付け・要約を生成
- 重要度別（高🔴/中🟡/低🟢）のスタイルで要約
- 復習クイズ付き（Obsidian SR対応）
- 処理済みファイルを01_Clipsにアーカイブ

### `/weekly-summary`
- 直近7日間のDailySummaryを横断集計
- ジャンル別・重要度別の統計
- トップキーワード・重要記事の一覧
- 来週への引き継ぎメモ

## タグ体系

| カテゴリ | タグ例 |
|---|---|
| ジャンル | `ジャンル/AI` `ジャンル/テック` `ジャンル/ビジネス` `ジャンル/セキュリティ` `ジャンル/ネットワーク` `ジャンル/ツール` |
| 重要度 | `重要度/高` `重要度/中` `重要度/低` |
| ソース | `ソース/公式` `ソース/ブログ` `ソース/ニュース` `ソース/SNS` |

## 必要な環境

- Obsidian
- Claude Desktop (Cowork)
- mcp-obsidian（Local REST API経由）

## 作者

ゆうさく
