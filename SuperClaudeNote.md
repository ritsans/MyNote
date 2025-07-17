## /sc:implement - 機能実装コマンド

新しい機能やコンポーネントの追加には `/sc:implement` コマンドが最適

このコマンドは機能、コンポーネント、APIなどの実装を自動的に適切な専門家ペルソナとツールで処理します 

### 基本的な使用方法

```
/sc:implement user authentication system      # 完全な機能実装  
/sc:implement --type component LoginForm      # 特定コンポーネント作成  
/sc:implement --type api user-management      # API エンドポイント構築  
/sc:implement --framework react dashboard     # フレームワーク固有の実装
```

**用途:**

- 実際のコード生成
- 機能の実装
- コンポーネントの作成
- APIエンドポイントの構築

### 有用なフラグ

- `--type component|api|service|feature|module` - 実装タイプ指定
- `--framework react|vue|express|django|etc` - 対象フレームワークを指定
- `--safe` - 保守的な実装アプローチをする
- `--iterative` - 段階的に開発してから検証
- `--with-tests` - テスト実装を含む
- `--documentation` - ドキュメント生成も同時実行



## `sc:design` - 設計コマンド

実装前の設計段階では `/sc:design` コマンドが有効

```
/sc:design user-auth-system        # ユーザー認証システム設計  
/sc:design --type api auth         # API設計のみ  
/sc:design --format spec payment   # 正式仕様書作成
```

**用途:**

- システムアーキテクチャの設計

- API仕様の作成

- コンポーネント設計

- データベース設計



### 例えば...

#### まずは設計 

`/sc:design user-auth-system --type architecture`

#### 設計に基づいて実装する

`/sc:implement user authentication system --type feature --with-tests`

「何を作るのか」を決めるのが**design**で、実際に開発を進めるのが**inplement**コマンド。

設計→実装の順序で使用することで、より効率的な開発が可能に。(CHANGELOGより)

Claudeの**Plan mode**も設計に使われますが、**実行前の計画を文章で表示する**機能です。コマンドに`--plan`フラグを付けることでプランモードで実行されます。`/sc:design`は独立した設計作業の際に使用します。



### 必要なMCPサーバー

実は無くても動作するが併用することで真価を発揮する

* Sequential MCP - 構造化された思考を実行する
* Magic MCP - フロントエンドデザインを提供（**APIキーが必要**。21st.devサービスの一部なので注意。 --no-magic で無効化することが可能）
* Context7 - パッケージの最新ドキュメントを参照
* Fetch MCP - AIがウェブページの内容を読み取りできる



## `--think`フラグの基本的な役割

`--think`フラグは、SuperClaudeに深い分析を行わせるためのフラグです。 FLAGS.md:21-25

`--think`フラグは以下の条件で自動的にアクティベートされます：

- **インポートチェーンが5ファイル以上**の場合
- **クロスモジュール呼び出しが10参照以上**の場合

### 手動で使用すべきシーン

#### 1. 複雑なシステム分析

```
/sc:analyze complex-system/ --think
```

複数のファイルにまたがる問題を理解する必要がある場合に有効。

#### 2. アーキテクチャ理解

```
/sc:analyze --focus architecture --persona-architect --think
```

システム全体の設計や依存関係を理解したい場合に使用します。

#### 3. 複雑なバグ調査

```
/sc:troubleshoot "complex bug" --think
```

複数のコンポーネントが関わる問題の調査時に有効です。 

### より深い分析が必要な場合

### `--think-hard`

- システム全体のリファクタリング
- 3つ以上のモジュールにまたがるボトルネック
- セキュリティ脆弱性調査 

### `--ultrathink`

* レガシーシステムの現代化

- 重大な脆弱性への対応

### 実用的な使い分け

```
# 基本的な分析から始める  
/sc:analyze code.js  
  
# 複雑さを感じたら--thinkを追加  
/sc:analyze code.js --think  
  
# さらに深い分析が必要なら--think-hardに  
/sc:analyze code.js --think-hard
```

`--think`フラグを使用すると、自動的にSequential MCPも有効化され、構造化された問題解決が適用されます。 **単純な問題には過剰**ですが、より複雑な問題では非常に有効です
