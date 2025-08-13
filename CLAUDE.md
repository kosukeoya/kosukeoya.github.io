# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## プロジェクト概要

このプロジェクトは**KARTE統合テスト用のGitHub Pagesウェブサイト**です。Template Partyの無料テンプレート「tp_beginner5」をベースとし、KARTEの統合機能を検証するための2つの並行環境を提供しています。

## アーキテクチャ

### 二重環境構成
- **`docs/eva/`** - KARTE評価・開発環境（`evaluation-cdn-edge.dev-karte.com`）
- **`docs/prod/`** - KARTE本番環境（`cdn-edge.karte.io`）

両環境は同一のファイル構造を持ちますが、異なるKARTE設定を使用：
- evaプロジェクトID: `4328587c28237035654ee82a3ca7ff21`
- prodプロジェクトID: `fdc90b2422db364f09303c9ceecaa25c`

### 技術スタック
- 静的HTML5/CSS3サイト（ビルドプロセスなし）
- 580px以下の画面幅でのレスポンシブ対応
- KARTE SDK統合（index.htmlのみ）
- ChannelIO統合（プラグインキー: `111d230c-2ef5-4793-940f-df90c903c24d`）

## 重要なファイルと場所

### HTMLページ
- `index.html` - KARTEタグ設置済みホームページ
- `about.html` - テンプレート技術説明（KARTEタグなし）
- `gallery.html` - 画像ギャラリー
- `link.html` - 外部リンクページ

### 設定
- `.mcp.json` - KARTE MCP サーバー設定（認証トークン含む）
- `css/style.css` - レスポンシブスタイル
- `images/` - サイト画像アセット

## デプロイメント

- **プラットフォーム:** GitHub Pages
- **ソース:** `docs/` フォルダ
- **プロセス:** 直接git push（CI/CDパイプラインなし）
- **URL:** `https://kosukeoya.github.io/[eva|prod]/`

## 開発ワークフロー

### ローカル開発
```bash
# 簡易HTTPサーバーでテスト
cd docs && python3 -m http.server 8000
```

### 編集時の重要な注意事項

1. **KARTEタグの保護:** index.htmlのKARTEタグ部分（プロジェクトID、CDN URL）は慎重に編集
2. **環境の使い分け:**
   - eva: 新機能テスト・実験的変更
   - prod: 本番相当の安定テスト
3. **Template Party著作権:** フッターの`Web Design:Template-Party`表示は削除禁止
4. **統合範囲:** KARTEタグはindex.htmlのみ設置（サブページには未設置）

## KARTE統合の詳細

### eva環境のタグ
```html
<script>!function(n){if(!window[n]){var o=window[n]=function(){var n=[].slice.call(arguments);return o.x?o.x.apply(0,n):o.q.push(n)};o.q=[],o.i=Date.now(),o.allow=function(){o.o="allow"},o.deny=function(){o.o="deny"}}}("krt")</script>
<script async src="https://evaluation-cdn-edge.dev-karte.com/4328587c28237035654ee82a3ca7ff21/edge.js"></script>
```

### prod環境のタグ
```html
<script>!function(n){var a=window[n]=function(){var n=[].slice.call(arguments);return a.x?a.x.apply(0,n):a.q.push(n)};a.q=[],a.i=Date.now()}("krt")</script>
<script async type="module" src="https://cdn-edge.karte.io/fdc90b2422db364f09303c9ceecaa25c/edge.js"></script>
```

eva環境にはオプトイン/アウト機能が含まれており、prod環境では`type="module"`指定があることに注意してください。