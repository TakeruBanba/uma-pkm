# セットアップガイド

## 必要なツール

1. Git
2. VS Code（推奨エディタ）
3. VS Code拡張機能
   - Markdown All in One
   - Markdown Preview Enhanced
   - Paste Image
   - Git Graph

## 初期セットアップ手順

1. リポジトリのクローン
```bash
git clone <repository-url>
cd UMA-PAN
```

2. 必要なディレクトリの作成
```bash
mkdir -p notes/{permanent,literature}
mkdir -p indexes
mkdir -p templates
mkdir -p assets
```

3. VS Codeの設定

settings.jsonに追加する設定:
```json
{
    "editor.wordWrap": "on",
    "markdown.preview.breaks": true,
    "markdown.links.openLocation": "beside",
    "markdown.extension.toc.levels": "2..6",
    "markdown.extension.toc.orderedList": false,
    "markdown.extension.toc.unorderedList.marker": "-"
}
```

## ノート作成ワークフロー

1. 新規ノートの作成
   - templatesフォルダから適切なテンプレートをコピー
   - ファイル名は`YYYYMMDDHHMMSS-title-slug.md`形式で作成

2. ノートの編集
   - フロントマターの記入
   - 本文の作成
   - タグの付与
   - 関連ノートへのリンク

3. コミット
   - 意味のあるコミットメッセージを記入
   - 定期的にプッシュ

## メンテナンス手順

### 週次メンテナンス

1. 新規ノートのレビュー
2. リンクの確認
3. タグの整理
4. 小規模な修正

### 月次メンテナンス

1. インデックスの更新
2. 古いノートのレビュー
3. タグ体系の見直し
4. バックアップの確認

## トラブルシューティング

1. リンク切れの修正
   - VS Codeの「リンク先を開く」機能で確認
   - 必要に応じてリンクを更新

2. 画像の取り扱い
   - assets/フォルダに保存
   - 相対パスで参照

3. マージコンフリクト
   - 複数デバイスでの編集時に注意
   - こまめなプル/プッシュを心がける

## Notionとの連携

### 一時メモの管理

1. Notionの利用目的
   - フリーティングノート（一時的なメモ）の記録
   - アイデアやインスピレーションの即時キャプチャ
   - 文献の初期メモ作成

2. Notionでのメモ取り方
   - 簡潔で素早いメモを心がける
   - 可能な限りタグを付与
   - 後で文献ノートや永続ノートに発展させる可能性のあるものにはフラグを立てる

3. メモの整理方法
   - 定期的なレビュー
   - 重要なメモの選別
   - 文献ノートや永続ノートへの変換判断

### Notionからノートへの展開プロセス

1. 文献ノートへの展開
   - Notionで取ったメモを文献ノートテンプレートに沿って整理
   - 必要な文献情報の補完
   - タグと関連ノートの設定

2. 永続ノートへの展開
   - 複数のNotionメモを統合して新しい洞察を形成
   - 既存の永続ノートとの関連付け
   - 適切なタグ付けと参照関係の構築

3. MCP連携の活用
   - NotionのAPIを使用した自動データ転送
   - メタデータの自動付与
   - 関連ノートの推奨

### メンテナンスとレビュー

1. 週次レビュー時の追加タスク
   - Notionメモの確認
   - 発展させるべきメモの選別
   - ノートへの変換作業

2. 月次メンテナンス時の追加タスク
   - 未処理メモの一括確認
   - 長期保存が不要なメモの整理
   - MCP連携の動作確認 