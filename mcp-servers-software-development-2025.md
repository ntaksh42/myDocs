# ソフトウェア開発で活用できるMCPサーバー総合レポート 2025

## 目次
1. [MCPサーバーとは](#mcpサーバーとは)
2. [優先度別MCPサーバー一覧](#優先度別mcpサーバー一覧)
3. [詳細情報](#詳細情報)
4. [セキュリティ考慮事項](#セキュリティ考慮事項)
5. [参考リソース](#参考リソース)

---

## MCPサーバーとは

Model Context Protocol（MCP）は、2024年11月にAnthropicが発表したオープンスタンダードで、AIモデル（LLM）と外部ツール、システム、データソースとの統合を標準化するプロトコルです。2025年3月にはOpenAIも採用し、同年4月にはGoogleも対応を発表しました。

**主な特徴：**
- AIエージェントが外部ツールと安全に通信できる標準化されたインターフェース
- ローカル実行またはリモートサーバー接続が可能
- Claude Desktop、Cursor、VS Code、Windsurf等の主要AIツールで利用可能

---

## 優先度別MCPサーバー一覧

### 優先度1: 必須（開発効率を大幅に向上）

| サーバー名 | カテゴリ | 実行場所 | 概要 |
|-----------|---------|---------|------|
| GitHub MCP Server | バージョン管理 | ローカル/リモート | リポジトリ管理、PR、Issue操作 |
| Filesystem | ファイル操作 | ローカル | セキュアなファイル操作 |
| Git | バージョン管理 | ローカル | Gitリポジトリの操作 |
| PostgreSQL | データベース | ローカル→外部DB | データベースクエリ・管理 |
| Playwright MCP | テスト | ローカル | ブラウザ自動化テスト |

### 優先度2: 推奨（開発ワークフローを強化）

| サーバー名 | カテゴリ | 実行場所 | 概要 |
|-----------|---------|---------|------|
| JetBrains MCP | IDE統合 | ローカル | IDE機能（リファクタリング等）連携 |
| Semgrep MCP | セキュリティ | ローカル | 静的コード解析・脆弱性スキャン |
| Kubernetes MCP | DevOps | ローカル→クラスタ | K8sクラスタ管理 |
| Supabase MCP | バックエンド | リモート | BaaS管理（DB、認証等） |
| Memory/Knowledge Graph | コンテキスト | ローカル | 永続的メモリ・ナレッジグラフ |

### 優先度3: 高い有用性（特定ユースケースで価値大）

| サーバー名 | カテゴリ | 実行場所 | 概要 |
|-----------|---------|---------|------|
| AWS MCP Servers | クラウド | ローカル→AWS | AWS各サービス操作 |
| Azure MCP Server | クラウド | ローカル→Azure | Azure各サービス操作 |
| Figma MCP | デザイン | ローカル/リモート | デザインからコード生成 |
| Datadog MCP | 監視 | ローカル→Datadog | メトリクス・ログ監視 |
| Firecrawl MCP | Webスクレイピング | リモート | Web データ収集・構造化 |

### 優先度4: 生産性向上（チーム連携・ドキュメント）

| サーバー名 | カテゴリ | 実行場所 | 概要 |
|-----------|---------|---------|------|
| Slack MCP | コミュニケーション | ローカル→Slack API | チャンネル・メッセージ管理 |
| Notion MCP | ドキュメント | リモート | ワークスペース操作 |
| Confluence MCP | ドキュメント | ローカル→Confluence | Wiki検索・編集 |
| Linear MCP | プロジェクト管理 | ローカル→Linear | イシュー・プロジェクト管理 |
| Discord MCP | コミュニケーション | ローカル→Discord API | サーバー・チャンネル管理 |

### 優先度5: 専門用途（特定分野で高い価値）

| サーバー名 | カテゴリ | 実行場所 | 概要 |
|-----------|---------|---------|------|
| Qdrant/Chroma/Pinecone | ベクトルDB | ローカル/リモート | セマンティック検索・RAG |
| SQLite/MySQL/Redis | データベース | ローカル→DB | 各種DB操作 |
| Sentry MCP | エラー監視 | ローカル→Sentry | エラートラッキング |
| Terminal/SSH MCP | インフラ | ローカル/リモート | コマンド実行・SSH接続 |
| Puppeteer MCP | ブラウザ自動化 | ローカル | Chrome DevTools連携 |

---

## 詳細情報

### 1. GitHub MCP Server（優先度1）

**公式リポジトリ:** https://github.com/github/github-mcp-server

**実行場所:** ローカルで実行し、GitHub APIに接続

**主な機能:**
- リポジトリの閲覧・管理
- Issue・Pull Requestの作成・更新・管理
- GitHub Actions ワークフローの監視
- コード検索・分析
- GitHub Projectsの操作（2025年10月追加）

**設定例:**
```json
{
  "mcpServers": {
    "github": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-github"],
      "env": {
        "GITHUB_PERSONAL_ACCESS_TOKEN": "<YOUR_TOKEN>"
      }
    }
  }
}
```

**最新アップデート（2025年10-11月）:**
- サーバーインストラクション機能追加
- ツールの統合・最適化
- 読み取り専用モードのサポート

---

### 2. Filesystem MCP Server（優先度1）

**公式リポジトリ:** https://github.com/modelcontextprotocol/servers/tree/main/src/filesystem

**実行場所:** 完全ローカル

**主な機能:**
- ファイルの読み書き
- ディレクトリ操作
- ファイル検索
- アクセス制御によるセキュアな操作

**特徴:**
- 設定可能なアクセス制御
- 指定ディレクトリ外へのアクセス防止
- シンボリックリンクの安全な処理

---

### 3. Git MCP Server（優先度1）

**公式リポジトリ:** https://github.com/modelcontextprotocol/servers/tree/main/src/git

**実行場所:** 完全ローカル

**主な機能:**
- リポジトリの読み取り・検索
- コミット履歴の分析
- ブランチ操作
- Diff表示

---

### 4. PostgreSQL MCP Server（優先度1）

**公式（Anthropic）:** https://github.com/modelcontextprotocol/servers/tree/main/src/postgres

**拡張版:**
- Postgres MCP Pro: https://github.com/crystaldba/postgres-mcp
- PostgreSQL MCP Server: https://github.com/HenkDz/postgresql-mcp-server
- MCP-PostgreSQL-Ops: https://github.com/call518/MCP-PostgreSQL-Ops

**実行場所:** ローカルで実行し、PostgreSQLデータベースに接続

**主な機能:**
- SQLクエリ実行
- スキーマ検査
- パフォーマンス分析（拡張版）
- インデックス推奨（拡張版）

**公式版の特徴:**
- 読み取り専用アクセス
- スキーマ検査機能

**拡張版の特徴:**
- 読み書き両方のアクセス
- 30以上の管理ツール（MCP-PostgreSQL-Ops）
- PostgreSQL 12-17対応

---

### 5. Playwright MCP Server（優先度1）

**公式リポジトリ:** https://github.com/microsoft/playwright-mcp

**実行場所:** 完全ローカル

**主な機能:**
- ブラウザ自動化
- アクセシビリティツリーベースの操作（スクリーンショット不要）
- E2Eテスト生成
- フォーム自動入力
- Webスクレイピング

**特徴:**
- 高速・軽量
- LLMフレンドリー（ビジョンモデル不要）
- 決定論的なツール適用
- Chromium、Firefox、WebKit対応

**インストール:**
```bash
npx @playwright/mcp@latest
```

---

### 6. JetBrains MCP Server（優先度2）

**公式リポジトリ:** https://github.com/JetBrains/mcp-jetbrains

**実行場所:** ローカル（JetBrains IDE内）

**対応IDE:**
- IntelliJ IDEA
- PyCharm
- WebStorm
- Android Studio
- その他全てのIntelliJベースIDE

**主な機能:**
- コード検査（エラー・警告の分析）
- リファクタリング（名前変更等）
- コード補完
- バグ修正提案

**特徴:**
- 2025.2以降のIDEに標準搭載
- SSEとSTDIO両方のトランスポート対応

---

### 7. Semgrep MCP Server（優先度2）

**公式リポジトリ:** https://github.com/semgrep/mcp
**ドキュメント:** https://semgrep.dev/docs/mcp

**実行場所:** ローカル

**主な機能:**
- セキュリティ脆弱性スキャン
- カスタムルールによるスキャン
- AST（抽象構文木）取得
- 依存関係の脆弱性検出
- 5,000以上のルール

**インストール:**
```bash
uvx semgrep-mcp
# または
docker run -i --rm ghcr.io/semgrep/mcp -t stdio
```

**対応言語:**
Python, JavaScript, TypeScript, Go, Java, Ruby, C, C++等、多数

---

### 8. Kubernetes MCP Server（優先度2）

**公式（Red Hat）:** https://github.com/containers/kubernetes-mcp-server
**コミュニティ版:** https://github.com/Flux159/mcp-server-kubernetes

**実行場所:** ローカルで実行し、K8sクラスタに接続

**主な機能:**
- Pod、Deployment、Service等の管理
- マルチクラスタ対応
- kubectlラッパーではなくKubernetes API直接操作
- OpenShift対応

**特徴:**
- 単一バイナリで配布
- Linux、macOS、Windows対応
- npm、Python、Dockerイメージ提供
- 非破壊モード（読み取り専用）対応

---

### 9. Supabase MCP Server（優先度2）

**公式:** https://supabase.com/docs/guides/getting-started/mcp
**ブログ:** https://supabase.com/blog/mcp-server

**実行場所:** ローカルで実行し、Supabase APIに接続（リモートMCPサーバーも提供）

**主な機能:**
- プロジェクト作成・管理
- テーブル設計・マイグレーション
- SQLクエリ実行
- ブランチング機能

**特徴:**
- OAuth動的クライアント登録による認証
- 開発・テスト環境専用（本番接続は非推奨）
- ホスト版MCPサーバーも利用可能

---

### 10. Memory/Knowledge Graph MCP Server（優先度2）

**公式:** https://github.com/modelcontextprotocol/servers/tree/main/src/memory
**拡張版:**
- https://github.com/gannonh/memento-mcp
- https://github.com/okooo5km/memory-mcp-server

**実行場所:** 完全ローカル（拡張版はNeo4j等外部DB使用可）

**主な機能:**
- ナレッジグラフによる永続メモリ
- エンティティ・関係の作成・管理
- セッション間のコンテキスト保持
- セマンティック検索（拡張版）

**ユースケース:**
- ユーザー設定の記憶
- プロジェクト情報の蓄積
- 過去の対話履歴の保持

---

### 11. AWS MCP Servers（優先度3）

**公式:** https://github.com/awslabs/mcp
**ドキュメント:** https://awslabs.github.io/mcp/

**実行場所:** ローカルで実行し、AWSサービスに接続

**提供サーバー:**
- AWS Lambda Tool MCP Server
- Amazon ECS MCP Server
- Amazon EKS MCP Server
- AWS S3 MCP Server
- AWS Glue/EMR MCP Server
- その他多数

**主な機能:**
- Lambda関数の直接実行
- S3テーブルの管理・クエリ
- EC2インスタンス管理
- CloudWatchログ分析

**注意:** 2025年5月26日よりSSEサポートが削除

---

### 12. Azure MCP Server（優先度3）

**特徴:** AWSと異なり、単一のMCPサーバーで複数機能を統合

**実行場所:** ローカルで実行し、Azureサービスに接続

**主な機能:**
- Azure RBAC設定
- Azure Monitorログ・メトリクス分析
- Cosmos DB管理
- Azure AI Foundry連携

**対応サービス:**
CosmosDB, SQL, SharePoint, Bing, Fabric等

---

### 13. Figma MCP Server（優先度3）

**公式:** https://www.figma.com/blog/introducing-figmas-dev-mode-mcp-server/
**ヘルプ:** https://help.figma.com/hc/en-us/articles/32132100833559

**実行場所:**
- Desktop版: ローカル（Figmaデスクトップアプリ経由）
- Remote版: リモート（https://mcp.figma.com/mcp）

**主な機能:**
- デザインからコード生成
- デザイントークン抽出
- 変数・スタイル情報取得
- Code Connect連携

**パフォーマンス:**
- セレクション処理: 50-200ms
- コード生成: 2-8秒

---

### 14. Datadog MCP Server（優先度3）

**公式ドキュメント:** https://docs.datadoghq.com/bits_ai/mcp_server/
**コミュニティ版:** https://github.com/winor30/mcp-server-datadog

**実行場所:** ローカルで実行し、Datadog APIに接続

**主な機能:**
- メトリクスクエリ
- モニター・SLO管理
- インシデント管理
- ログ検索
- ダッシュボード操作

---

### 15. Firecrawl MCP Server（優先度3）

**公式:** https://github.com/firecrawl/firecrawl-mcp-server
**ドキュメント:** https://docs.firecrawl.dev/mcp-server

**実行場所:** ローカルで実行し、Firecrawl APIに接続（セルフホスト可）

**主な機能:**
- Webスクレイピング
- バッチ処理
- 構造化データ抽出
- LLMフレンドリーなMarkdown/JSON出力

**必要な環境変数:**
```
FIRECRAWL_API_KEY=<your-api-key>
```

---

### 16. Slack MCP Server（優先度4）

**公式（Anthropic）:** コミュニティメンテナンスに移行
**高機能版:** https://github.com/korotovsky/slack-mcp-server

**実行場所:** ローカルで実行し、Slack APIに接続

**主な機能:**
- チャンネル管理
- メッセージ送受信
- ユーザー情報取得
- DM・グループDM対応

**高機能版の特徴:**
- ステルスモード（権限不要）
- OAuth対応
- Enterprise Slack対応
- 月間30,000エンジニア以上が利用

---

### 17. Notion MCP Server（優先度4）

**公式:** https://developers.notion.com/docs/mcp
**GitHub:** https://github.com/makenotion/notion-mcp-server

**実行場所:**
- オープンソース版: ローカル
- ホスト版: リモート（Notion提供）

**主な機能:**
- ページの読み書き
- データベース操作
- PRD・技術仕様書作成
- AI向けに最適化されたMarkdown API

---

### 18. Confluence MCP Server（優先度4）

**Atlassian公式:** リモートMCPサーバー（Cloudflare上でホスト）
**コミュニティ版:** https://github.com/aashari/mcp-server-atlassian-confluence

**実行場所:**
- 公式: リモート（Atlassian提供）
- コミュニティ: ローカル

**主な機能:**
- スペース・ページの取得
- CQL（Confluence Query Language）検索
- Markdownフォーマットでのコンテンツ取得
- Jiraとの連携（公式版）

---

### 19. ベクトルDB MCP Servers（優先度5）

#### Qdrant MCP
**実行場所:** ローカルまたはリモート（Docker/クラウド）

**特徴:**
- 高性能ベクトル類似検索
- 複数の埋め込みプロバイダー対応
- Rust実装による高速処理
- 複雑なメタデータフィルタリング

#### ChromaDB MCP
**実行場所:** ローカル

**特徴:**
- 開発者フレンドリーなAPI
- RAGシステムへの簡単な統合
- プロトタイピング・小規模アプリ向け

#### Pinecone MCP
**実行場所:** リモート（Pinecone Cloud）

**特徴:**
- フルマネージドサービス
- 数十億ベクトルの処理
- エンタープライズグレード

---

### 20. Terminal/SSH MCP Server（優先度5）

**代表的なサーバー:**
- Terminal MCP Server: https://www.pulsemcp.com/servers/rinardnick-terminal
- SSH MCP Server: https://github.com/tufantunc/ssh-mcp

**実行場所:** ローカル（SSH経由でリモート実行も可能）

**主な機能:**
- ローカルコマンド実行
- SSH経由のリモートコマンド実行
- セッション永続化
- 環境変数設定
- ホワイトリストによるセキュリティ制御

---

## セキュリティ考慮事項

2025年4月、セキュリティ研究者がMCPの複数のセキュリティ問題を報告しています：

1. **プロンプトインジェクション**: 悪意のあるプロンプトによる意図しない動作
2. **ツール権限の組み合わせ**: 複数ツールの組み合わせによるファイル漏洩
3. **類似ツールによる置換**: 信頼されたツールの静的な置換

**推奨対策:**
- 必要最小限の権限を付与
- 本番データベースへの直接接続を避ける
- 読み取り専用モードの活用
- アクセスログの監視
- 定期的なセキュリティ監査

---

## 参考リソース

### 公式リソース
- **MCP公式サーバーリポジトリ:** https://github.com/modelcontextprotocol/servers
- **MCP仕様書:** https://modelcontextprotocol.io/
- **Anthropic MCP発表:** https://www.anthropic.com/news/model-context-protocol

### キュレーションリスト
- **appcypher/awesome-mcp-servers:** https://github.com/appcypher/awesome-mcp-servers
- **wong2/awesome-mcp-servers:** https://github.com/wong2/awesome-mcp-servers
- **DevOps特化:** https://github.com/rohitg00/awesome-devops-mcp-servers

### ディレクトリサービス
- **MCP.so:** https://mcp.so/
- **PulseMCP:** https://www.pulsemcp.com/
- **MCPServers.org:** https://mcpservers.org/

### Docker Hub
- **Docker MCP Catalog:** https://hub.docker.com/search?q=mcp

---

## まとめ

MCPサーバーは2025年現在、急速に成長しているエコシステムです。ソフトウェア開発での活用においては、以下の順序で導入を検討することを推奨します：

1. **まず導入すべき:** GitHub、Filesystem、Git、PostgreSQL、Playwright
2. **次に検討:** JetBrains、Semgrep、Kubernetes、Supabase、Memory
3. **ユースケースに応じて:** クラウド（AWS/Azure）、Figma、監視ツール
4. **チーム連携強化:** Slack、Notion、Confluence、Linear

各サーバーの実行場所（ローカル vs 外部サービス接続）を理解し、セキュリティを考慮した上で、開発ワークフローに統合することで、AI支援開発の効率を大幅に向上させることができます。

---

*最終更新: 2025年11月*
*参考: 公式ドキュメント、GitHub、各種技術ブログ*
