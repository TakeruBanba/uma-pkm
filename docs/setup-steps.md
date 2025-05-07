# Zettelkastenシステム構築手順

## Step 1: 環境準備

1. 必要なツールのインストール
   - Git
     - [Git公式サイト](https://git-scm.com/)からダウンロードしてインストール
     - インストール後、以下のコマンドで設定
     ```bash
     git config --global user.name "あなたの名前"
     git config --global user.email "あなたのメールアドレス"
     ```

   - VS Code
     - [Visual Studio Code公式サイト](https://code.visualstudio.com/)からダウンロードしてインストール
     - 日本語化は「Japanese Language Pack」をインストール

   - VS Code拡張機能（すべて拡張機能マーケットプレースからインストール）
     - Markdown All in One: マークダウン編集の基本機能
     - Markdown Preview Enhanced: プレビュー機能強化
     - Paste Image: 画像の貼り付けと管理
     - Git Graph: Gitの履歴可視化

2. Notionのセットアップ
   - アカウント作成（未作成の場合）
     - [Notion公式サイト](https://www.notion.so/)でアカウント作成
     - 無料プランで十分な機能あり
   
   - 一時メモ用のデータベース作成
     ```
     データベースのプロパティ設定例：
     - タイトル（デフォルト）
     - タグ（マルチセレクト）
     - ステータス（セレクト：一時メモ、文献候補、永続候補）
     - 作成日（日付）
     - 更新日（日付）
     - URL（URL）
     - メモタイプ（セレクト：アイデア、引用、疑問、etc）
     ```

   - タグ体系の初期設定
     ```
     基本タグ例：
     - #concept（概念）
     - #method（手法）
     - #tool（ツール）
     - #quote（引用）
     - #idea（アイデア）
     - #question（疑問）
     - #answer（回答）
     ```

## Step 2: リポジトリのセットアップ

1. リポジトリの作成
```bash
# ローカルリポジトリの作成
mkdir UMA-PAN
cd UMA-PAN
git init

# 初期ブランチ名の設定（必要な場合）
git branch -M main
```

2. 基本ディレクトリ構造の作成
```bash
# Windowsの場合
mkdir docs
mkdir notes
mkdir notes\permanent
mkdir notes\literature
mkdir indexes
mkdir templates
mkdir assets

# macOS/Linuxの場合
mkdir -p docs
mkdir -p notes/{permanent,literature}
mkdir -p indexes
mkdir -p templates
mkdir -p assets
```

3. 初期ファイルの配置
   - .gitignoreの設定
   ```
   # .gitignore の内容
   .DS_Store
   .vscode/
   *.log
   _temp/
   ```

   - 各ディレクトリに.gitkeepを配置（空ディレクトリの追跡用）
   ```bash
   touch notes/permanent/.gitkeep
   touch notes/literature/.gitkeep
   touch indexes/.gitkeep
   touch assets/.gitkeep
   ```

## Step 3: テンプレートの準備

1. 永続ノートテンプレートの配置
   - `templates/permanent-note.md`の具体的な内容：
   ```markdown
   ---
   title: "{{title}}"
   date: {{date}}
   type: "permanent"
   tags: []
   related: []
   status: "draft"
   ---

   # {{title}}

   ## 概要

   <!-- ノートの主要な概念や考えを簡潔に説明 -->

   ## 本文

   <!-- メインコンテンツをここに記述 -->

   ## 関連する考察

   <!-- このノートから派生する思考や疑問 -->

   ## 参考文献

   <!-- 参考にした文献やリソース -->

   ## メタデータ

   - 作成日: {{date}}
   - 最終更新日: {{date}}
   - ステータス: draft

   ## 関連ノート

   <!-- 関連するノートへのリンク -->
   ```

2. 文献ノートテンプレートの配置
   - `templates/literature-note.md`の具体的な内容：
   ```markdown
   ---
   title: "{{title}}"
   date: {{date}}
   type: "literature"
   author: ""
   source: ""
   tags: []
   related: []
   status: "draft"
   ---

   # {{title}}

   ## 文献情報

   - 著者: 
   - 出典: 
   - 発行日: 
   - URL: 

   ## 要約

   <!-- 文献の主要なポイントを簡潔に要約 -->

   ## 重要な引用

   <!-- 重要な引用や参照したい箇所 -->

   ## 考察

   <!-- 文献から得られた洞察や考察 -->

   ## キーワード

   <!-- 重要なキーワードやコンセプト -->

   ## メタデータ

   - 作成日: {{date}}
   - 最終更新日: {{date}}
   - ステータス: draft

   ## 関連ノート

   <!-- 関連するノートへのリンク -->
   ```

## Step 4: MCP連携の設定

1. Notion APIの設定
   - APIキーの取得
     1. [Notion Developers](https://developers.notion.com/)にアクセス
     2. 「View my integrations」をクリック
     3. 「New integration」で新規統合を作成
     4. 必要な権限を設定（Read & Write）
     5. 生成されたAPIキーを安全に保管

   - 統合の設定
     1. Notionデータベースで「...」をクリック
     2. 「コネクトの追加」を選択
     3. 作成した統合を選択して接続

2. MCPの設定
   - API設定の構成例：
   ```python
   # config.py
   NOTION_API_KEY = "your-api-key"
   NOTION_DATABASE_ID = "your-database-id"
   
   # Notionデータベースのプロパティ名マッピング
   PROPERTY_MAPPINGS = {
       "タイトル": "title",
       "タグ": "tags",
       "ステータス": "status",
       "作成日": "created_at",
       "更新日": "updated_at"
   }
   ```

## Step 5: 初期コンテンツの作成

1. インデックスの作成
   - メインインデックス（`indexes/main.md`）の例：
   ```markdown
   # メインインデックス

   ## 概念マップ
   - [[概念A]]
   - [[概念B]]
     - [[サブ概念B1]]
     - [[サブ概念B2]]

   ## プロジェクト
   - [[プロジェクトA]]
   - [[プロジェクトB]]

   ## 研究テーマ
   - [[テーマA]]
   - [[テーマB]]
   ```

   - タグインデックス（`indexes/tags.md`）の例：
   ```markdown
   # タグインデックス

   ## 概念 (#concept)
   - [[概念ノートA]]
   - [[概念ノートB]]

   ## 手法 (#method)
   - [[手法ノートA]]
   - [[手法ノートB]]

   ## ツール (#tool)
   - [[ツールノートA]]
   - [[ツールノートB]]
   ```

2. 最初のノートの作成
   - システム説明ノート（`notes/permanent/YYYYMMDDHHMMSS-system-usage.md`）
   - タグ体系説明ノート（`notes/permanent/YYYYMMDDHHMMSS-tag-system.md`）
   - ワークフロー説明ノート（`notes/permanent/YYYYMMDDHHMMSS-workflow.md`）

## Step 6: 運用ルールの設定

1. ファイル名規則
   ```
   形式: YYYYMMDDHHMMSS-title-slug.md
   例：
   - 20240320143000-zettelkasten-basics.md
   - 20240320143500-note-taking-methods.md
   ```

2. タグ付けルール
   ```
   基本ルール：
   - 小文字のみ使用
   - スペースの代わりにハイフンを使用
   - 複数のタグはカンマで区切る
   
   例：
   tags: [concept, note-taking, zettelkasten]
   ```

3. リンク作成ルール
   ```
   内部リンク：[[ノートタイトル]]
   外部リンク：[タイトル](URL)
   バックリンク：自動生成（VSCode拡張で管理）
   ```

## Step 7: バックアップと同期の設定

1. Gitリポジトリの設定
```bash
# リモートリポジトリの追加
git remote add origin <repository-url>

# 初期コミット
git add .
git commit -m "Initial commit"

# プッシュ
git push -u origin main
```

2. バックアップ設定
   - 自動バックアップスクリプト例（PowerShell）：
   ```powershell
   # backup.ps1
   $source = "パス\to\UMA-PAN"
   $destination = "パス\to\backup\UMA-PAN"
   $date = Get-Date -Format "yyyyMMdd"
   
   # バックアップ実行
   Copy-Item -Path $source -Destination "$destination-$date" -Recurse
   ```

## Step 8: 定期メンテナンスの設定

1. 週次レビューチェックリスト
```markdown
# 週次レビュー

## Notionメモの確認
- [ ] 未処理の一時メモの確認
- [ ] 文献ノート候補の選別
- [ ] 永続ノート候補の選別

## ノートのメンテナンス
- [ ] 新規作成したノートの確認
- [ ] リンクの整合性確認
- [ ] タグの適切性確認

## システムの確認
- [ ] バックアップの実行確認
- [ ] Git同期の確認
- [ ] MCPの動作確認
```

2. 月次メンテナンスチェックリスト
```markdown
# 月次メンテナンス

## コンテンツレビュー
- [ ] すべてのインデックスの更新
- [ ] 孤立したノートの確認
- [ ] タグ体系の見直し

## システムメンテナンス
- [ ] 古いバックアップの整理
- [ ] テンプレートの見直し
- [ ] ディレクトリ構造の最適化

## 改善計画
- [ ] 運用上の課題の特定
- [ ] 改善案の検討
- [ ] 次月の目標設定
```

## Step 9: 使用開始

1. 試験運用のワークフロー例
```markdown
1. Notionでメモ作成
   - 読んだ記事の要点をメモ
   - タグ付け：#article, #tech
   - ステータス：文献候補

2. 文献ノート作成
   - テンプレート使用
   - Notionメモから情報転記
   - 追加の考察を記入

3. 永続ノート作成
   - 関連する文献ノートをリンク
   - 独自の洞察を追加
   - タグとリンクの設定
```

2. システム評価チェックリスト
```markdown
## 使用感評価
- [ ] ノート作成の容易さ
- [ ] 情報検索の効率
- [ ] リンク管理の使いやすさ

## 技術面の評価
- [ ] MCPの応答性
- [ ] 同期の安定性
- [ ] バックアップの確実性

## 運用面の評価
- [ ] ワークフローの自然さ
- [ ] メンテナンスの負担
- [ ] 改善ポイントの特定
```

## 補足: トラブルシューティング

1. 一般的な問題と解決策
   ```markdown
   ## リンク切れ
   - 症状：[[リンク]]が機能しない
   - 解決：ファイル名とリンク名の一致確認
   
   ## 同期問題
   - 症状：プッシュ/プルでコンフリクト
   - 解決：ローカルの変更をstashしてプル
   
   ## タグ重複
   - 症状：類似タグの乱立
   - 解決：タグインデックスでの整理
   ```

2. MCP関連の問題
   ```markdown
   ## API接続エラー
   - 症状：Notion APIに接続できない
   - 確認：APIキーの有効性
   - 解決：統合の再設定
   
   ## 同期エラー
   - 症状：自動同期が失敗
   - 確認：ネットワーク接続
   - 解決：手動での同期実行
   
   ## テンプレート適用エラー
   - 症状：テンプレート変数が展開されない
   - 確認：変数名の正確性
   - 解決：テンプレート構文の修正
   ``` 