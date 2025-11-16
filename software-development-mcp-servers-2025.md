# ソフト開発に便利なMCPサーバー総まとめ 2025

## 目次
1. [MCPの概要](#mcpの概要)
2. [2025年の採用状況](#2025年の採用状況)
3. [カテゴリー別MCPサーバー](#カテゴリー別mcpサーバー)
4. [公式リファレンスサーバー](#公式リファレンスサーバー)
5. [導入方法とリソース](#導入方法とリソース)
6. [まとめ](#まとめ)

---

## MCPの概要

### Model Context Protocol (MCP) とは

Model Context Protocol (MCP) は、Anthropicが**2024年11月**に発表したオープンスタンダード・オープンソースフレームワークです。AIシステム（大規模言語モデル、LLM）が外部ツール、システム、データソースと統合・データ共有する方法を標準化します。

**通称**: 「AIのUSB-C」

### アーキテクチャ

MCPは、データソースとAI駆動ツール間の**セキュアで双方向の接続**を構築するためのオープンスタンダードです：

- **MCPサーバー**: データやツールを公開する側
- **MCPクライアント**: AIアプリケーション（例: Claude Desktop、Cursor、VSCode with Copilot）

### 対応言語SDK

以下の言語で公式・コミュニティSDKが提供されています：
- Python
- TypeScript
- C#
- Go
- Java
- Kotlin
- PHP
- Ruby
- Rust
- Swift

---

## 2025年の採用状況

### 主要テック企業の採用

#### OpenAI (2025年3月)
- ChatGPTデスクトップアプリ
- OpenAI Agents SDK
- Responses API

すべてのプロダクトでMCPを統合しました。

#### Microsoft (2025年5月)
- **Microsoft Build 2025**で発表
- Windows 11で**MCPのアーリープレビュー**を開始
- セキュアで相互運用可能なエージェントコンピューティングの基盤層として採用

#### Google (2025年4月)
- Google DeepMindが**Gemini**でのMCP対応を発表

#### JetBrains (2025年5月)
- **IntelliJ IDEA 2025.1**でMCPサポートを実装
- AIアシスタントにリアルタイムでプロジェクトコンテキストへのアクセスを提供

#### その他の採用事例
- **Replit**: コーディングプラットフォーム
- **Sourcegraph**: コードインテリジェンスツール
- **GitHub**: 公式MCP ServerとMCP Registryを公開

### エコシステムの成長

- **2024年11月**: Anthropic がMCPを発表
- **2025年時点**: **1,000以上のMCPサーバー**がコミュニティにより公開
- **GitHub MCP Registry**: 公式MCPサーバーディレクトリが公開
- **PulseMCP**: 6,480以上のサーバーを毎日更新

---

## カテゴリー別MCPサーバー

### 🔧 コード管理・バージョン管理

#### 1. GitHub MCP Server ⭐ **最人気**
- **提供**: GitHub公式
- **機能**:
  - リポジトリ操作（読み取り、検索、操作）
  - Issue作成・管理
  - Pull Request管理
  - コードレビュー
  - コミット履歴の確認
- **特徴**:
  - 自然言語でGitHub操作が可能
  - `--toolsets`フラグで機能グループを制御可能
  - 開発者の生産性を劇的に向上
- **対応クライアント**: Claude Desktop, Cursor, VSCode with Copilot, Windsurf, Goose

#### 2. Git MCP
- **提供**: Anthropic公式リファレンス実装
- **機能**:
  - Gitリポジトリの読み取り
  - リポジトリ内検索
  - Git操作（コミット、ブランチなど）

#### 3. GitKraken MCP
- **機能**:
  - GitKraken API統合
  - Jira、GitHub、GitLabとの連携
  - CLI経由での操作

#### 4. Azure DevOps MCP
- **機能**:
  - Azure DevOpsの機能を直接AIエージェントに提供
  - CI/CDパイプライン管理
  - ワークアイテム追跡

#### 5. Gitee MCP
- **機能**:
  - Gitee API統合（中国のGitプラットフォーム）
  - リポジトリ・Issue管理

---

### 🧪 テスト・品質保証

#### 6. Playwright MCP
- **機能**:
  - ブラウザ自動化
  - テストフレームワーク統合
  - E2Eテストの実行

#### 7. CircleCI MCP
- **機能**:
  - **AIエージェントがCircleCIのビルド失敗を修正**
  - CI/CDパイプラインの監視
  - ビルドログの解析

#### 8. Currents MCP
- **機能**:
  - Currentsプラットフォーム経由でPlaywrightテスト失敗を修正
  - テスト結果の分析

#### 9. SonarQube MCP
- **機能**:
  - コード品質分析
  - 脆弱性検出
  - コードスメル識別

#### 10. Semgrep MCP
- **機能**:
  - 静的解析
  - セキュリティパターン検出
  - コードパターン解析

---

### 💾 データベース

#### 11. PostgreSQL/Neon MCP ⭐
- **提供**: Anthropic公式リファレンス実装（PostgreSQL）、Neon（サーバーレス）
- **機能**:
  - データベース接続
  - クエリ実行
  - スキーマ検査
  - テーブル情報取得

#### 12. Supabase MCP ⭐ **コミュニティ人気No.1**
- **機能**:
  - データベースアクセス
  - 認証機能
  - エッジファンクション
- **評価**: 「型の不整合や幻覚が激減する」と高評価

#### 13. MotherDuck/DuckDB MCP
- **機能**:
  - DuckDBでのデータクエリと分析
  - 高速な分析クエリ実行

#### 14. Neo4j MCP
- **機能**:
  - グラフデータベース操作
  - Cypherクエリサポート
  - グラフアルゴリズムの実行

---

### ☁️ クラウド・インフラストラクチャ

#### 15. AWS MCP Servers ⭐
- **提供**: AWS公式
- **機能**:
  - AWSドキュメントへのアクセス
  - ベストプラクティスガイダンス
  - コンテキスト情報の提供

#### 16. AWS CDK MCP
- **機能**:
  - Infrastructure as Code (IaC) のアドバイス
  - AWSサービスガイダンス
  - CDKスタック構築支援

#### 17. Google Cloud Run MCP
- **機能**:
  - Google Cloudへの直接デプロイ
  - Cloud Runサービス管理

#### 18. Cloudflare MCP
- **機能**:
  - Workers管理
  - KV（Key-Value）ストレージ
  - R2オブジェクトストレージ
  - D1データベース

#### 19. Terraform MCP
- **機能**:
  - インフラストラクチャプロビジョニング支援
  - Terraformコード生成
  - ベストプラクティス提案

#### 20. Docker MCP
- **機能**:
  - コンテナ起動・停止
  - ステータス確認
  - ログ検査
  - ターミナルを開かずにDocker操作が可能

---

### 📝 ドキュメント・ナレッジ

#### 21. Fetch MCP
- **提供**: Anthropic公式リファレンス実装
- **機能**:
  - Webコンテンツ取得・変換
  - HTMLをMarkdownに変換
  - 効率的なLLM利用のための前処理

#### 22. Context7 MCP
- **機能**:
  - オープンソースサードパーティライブラリのハブ
  - 最新APIドキュメントの提供
  - 複数言語エコシステムに対応

#### 23. Brave Search MCP
- **機能**:
  - Brave Search API統合
  - Web検索機能（無料枠: 2,000クエリ/月）
  - リアルタイム情報取得

#### 24. Microsoft Learn Docs MCP
- **機能**:
  - Microsoft技術の最新ドキュメントへのアクセス
  - APIリファレンス
  - ベストプラクティス（Azure SDK、C# 13、.NET Aspireなど）

---

### 🎨 デザイン・フロントエンド

#### 25. Figma/Dev Mode MCP
- **機能**:
  - デザインファイル情報の取得
  - Figmaからのコード生成
  - プロジェクト、ファイル、選択要素の詳細情報を提供

#### 26. Vercel MCP
- **機能**:
  - フロントエンド開発・ホスティングプラットフォーム統合
  - AIとの連携
- **対応クライアント**: Gemini CLI, Gemini Code Assist, Windsurf, Goose, Raycast, Devin, VSCode with Copilot, Cursor, Claude, ChatGPT

---

### 🔍 観測性・パフォーマンス

#### 27. Digma MCP
- **機能**:
  - **ランタイム観測データをAIで活用**
  - パフォーマンス問題の露呈
  - テストのフレーキネス検出
  - ボトルネック特定
  - コードレビューとリファクタリングの支援

---

### 🌐 Web自動化・スクレイピング

#### 28. Firecrawl MCP
- **機能**:
  - Webスクレイピング
  - ブラウザ自動化
  - マルチモーダルエージェント対応

#### 29. Puppeteer MCP
- **提供**: Anthropic公式リファレンス実装
- **機能**:
  - ブラウザ自動操作
  - Webスクレイピング
  - テスト自動化

---

### 🧠 AI・問題解決

#### 30. Sequential Thinking MCP
- **提供**: Anthropic公式リファレンス実装
- **機能**:
  - 動的で反省的な問題解決
  - 思考シーケンスを通じた複雑なタスクの分解
  - 構造的アプローチ

#### 31. Memory MCP
- **提供**: Anthropic公式リファレンス実装
- **機能**:
  - ナレッジグラフベースの永続メモリシステム
  - コンテキストの保持・再利用

---

### 💬 コミュニケーション

#### 32. Slack MCP ⭐
- **機能**:
  - メッセージ送信
  - チャンネル管理
  - リアクション機能
  - ユーザープロフィール取得
  - 複数トランスポート対応（Stdio、SSE）

#### 33. Gmail MCP
- **機能**:
  - OAuth2認証
  - メール送受信・検索
  - ラベル管理
  - 一括処理

#### 34. Discord MCP
- **機能**:
  - Bot経由でメッセージ送信
  - 過去メッセージ取得
  - チャンネルへの自動投稿

---

### 📦 ファイル・ストレージ

#### 35. Filesystem MCP ⭐ **定番**
- **提供**: Anthropic公式リファレンス実装
- **機能**:
  - セキュアなファイル操作
  - ファイルの読み書き・削除・一覧取得
  - 設定可能なアクセス制御
- **特徴**: MCPの基本として頻繁に使用される

#### 36. Google Drive MCP ⭐
- **提供**: Anthropic公式実装
- **機能**:
  - ファイル操作
  - 検索
  - メタデータ取得
  - クラウドストレージへの効率的なアクセス

---

### 🕒 ユーティリティ

#### 37. Time MCP
- **提供**: Anthropic公式リファレンス実装
- **機能**:
  - 時刻・タイムゾーン変換
  - 日時計算

---

### 🛠️ その他のビジネスツール

#### 38. Shopify MCP
- **機能**: ECプラットフォーム統合

#### 39. Canva MCP
- **機能**: デザインツール統合

#### 40. Stripe MCP
- **機能**: 決済処理統合

#### 41. Salesforce MCP
- **機能**: CRM統合

#### 42. HubSpot MCP
- **機能**: マーケティング・セールス統合

#### 43. Notion MCP
- **機能**: ドキュメント管理統合

#### 44. Airtable MCP
- **機能**: データベース・スプレッドシート統合

---

## 公式リファレンスサーバー

Anthropicが提供する**公式のリファレンス実装**：

| サーバー名 | 説明 |
|-----------|------|
| **Everything** | プロンプト、リソース、ツールを含む包括的なテスト・リファレンスサーバー |
| **Fetch** | Webコンテンツの取得と変換（効率的なLLM利用） |
| **Filesystem** | セキュアなファイル操作、設定可能なアクセス制御 |
| **Git** | Gitリポジトリの読み取り、検索、操作 |
| **Memory** | ナレッジグラフベースの永続メモリシステム |
| **Sequential Thinking** | 思考シーケンスを通じた動的・反省的問題解決 |
| **Time** | 時刻・タイムゾーン変換機能 |

### 公式統合パートナー（130以上）

**主要企業**:
- AWS（クラウドサービス）
- Azure（ストレージ、Cosmos DB、CLIツール）
- Atlassian（Jira、Confluence）
- GitHub（リポジトリ・コード管理）
- Slack（チームコミュニケーション）
- PostgreSQL/SQLite（データベース）

**決済・金融**: Alpaca, Stripe, Bitnovo Pay, PayPal

**データ・分析**: Databricks, BigQuery, Snowflake, Algolia

**その他**: Webスクレイピング、画像生成、暗号資産・ブロックチェーン、スマートホーム、ヘルスデータ、リーガルテックなど

---

## 導入方法とリソース

### MCPサーバーを探せるリソース

| リソース | 説明 | URL |
|---------|------|-----|
| **GitHub MCP Registry** | GitHub公式MCPサーバーディレクトリ | https://github.blog/ai-and-ml/github-copilot/meet-the-github-mcp-registry-the-fastest-way-to-discover-mcp-servers/ |
| **PulseMCP** | 6,480+サーバー、毎日更新 | - |
| **mcp.so** | 検索・Playground実行可能 | - |
| **GitHub: modelcontextprotocol/servers** | Anthropic公式リポジトリ | https://github.com/modelcontextprotocol/servers |
| **awesome-mcp-servers** | コミュニティキュレーション（punkpeye） | https://github.com/punkpeye/awesome-mcp-servers |
| **awesome-mcp-servers** | コミュニティキュレーション（wong2） | https://github.com/wong2/awesome-mcp-servers |
| **cursor/mcp-servers** | Cursor版キュレーションリスト | https://github.com/cursor/mcp-servers |
| **awesome-mcp-devtools** | 開発ツール、SDK、ライブラリ | https://github.com/punkpeye/awesome-mcp-devtools |
| **MCPServerFinder.com** | カテゴリー別検索 | - |

### 対応クライアント

MCPサーバーは以下のクライアントで利用可能です：

- **Claude Desktop**（Anthropic）
- **Claude Code**（Anthropic）
- **ChatGPT Desktop**（OpenAI）
- **Cursor**
- **VSCode with Copilot**
- **Windsurf**
- **Goose**
- **Raycast**
- **Devin**
- **Gemini CLI/Code Assist**（Google）
- **IntelliJ IDEA 2025.1+**（JetBrains）

### セキュリティアップデート（2025年6月）

2025年6月18日のチェンジログで、以下のセキュリティ更新が導入されました：

- MCPサーバーの認証処理の明確化
- MCPクライアントのリソースインジケーター実装
- 悪意のあるサーバーからのアクセストークン取得防止

---

## まとめ

### 開発者におすすめのMCPサーバートップ10

| ランク | サーバー名 | 用途 | おすすめ理由 |
|--------|-----------|------|-------------|
| 1 | **GitHub MCP** | バージョン管理 | 自然言語でGitHub操作、最も使用頻度が高い |
| 2 | **Filesystem MCP** | ファイル操作 | 基本中の基本、セキュアなファイル管理 |
| 3 | **Supabase MCP** | データベース | 型の不整合・幻覚が激減、コミュニティ人気No.1 |
| 4 | **Docker MCP** | インフラ | ターミナル不要でコンテナ管理 |
| 5 | **AWS MCP Servers** | クラウド | AWSベストプラクティスへの即座アクセス |
| 6 | **Figma/Dev Mode MCP** | デザイン→コード | デザインからコードへの変換を効率化 |
| 7 | **Slack MCP** | コミュニケーション | チームコラボレーションの自動化 |
| 8 | **CircleCI MCP** | CI/CD | AIがビルド失敗を自動修正 |
| 9 | **Digma MCP** | 観測性 | パフォーマンス問題を即座に特定 |
| 10 | **Context7 MCP** | ドキュメント | 常に最新のライブラリAPIドキュメント |

### 用途別推奨

| 用途 | 推奨MCPサーバー |
|------|----------------|
| **個人開発者** | GitHub, Filesystem, Fetch |
| **チーム開発** | GitHub, Slack, CircleCI, Supabase |
| **フロントエンド開発** | Figma, Vercel, Playwright |
| **バックエンド開発** | PostgreSQL/Supabase, Docker, AWS |
| **フルスタック開発** | GitHub, Supabase, Docker, Vercel |
| **DevOps/SRE** | Docker, AWS, Terraform, CircleCI, Digma |
| **Web3/ブロックチェーン** | 専門サーバー（130+統合から選択） |

### 2025年の展望

1. **エンタープライズ導入加速**: Microsoft、OpenAI、Googleの採用により企業導入が加速
2. **エコシステムの成熟**: 1,000以上のサーバーから10,000以上へ拡大予定
3. **セキュリティ強化**: 認証・アクセス制御の標準化が進行
4. **マルチモーダル対応**: 画像、音声、動画などへの対応拡大
5. **IDE統合の深化**: すべての主要IDEでネイティブサポート

### MCPがもたらす変革

MCPは「AIのUSB-C」として、以下の変革をもたらしています：

- **統一されたインターフェース**: 異なるツール・サービスを1つの標準で接続
- **生産性の向上**: 自然言語でツール操作、手作業の削減
- **AIエージェントの実用化**: 複数のツールをまたいだタスクの自動化
- **エコシステムの成長**: オープンスタンダードによる急速な発展

MCPは、AI駆動開発の未来を形作る重要な技術基盤となっています。

---

**最終更新**: 2025年11月
**情報源**: GitHub MCP Registry、Anthropic公式ドキュメント、Web調査

