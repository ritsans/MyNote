## 🧭 Planモード

Claude Code における「**計画と実装を分離するためのモード**」です。
ざっくり言えば、**いきなりコードを書き始めるのではなく、まず“設計”に集中するフェーズ**を明示的に切り替える仕組みです。

### 🧠 Planモードの目的とメリット

| 特徴                     | 解説                                                         |
| ------------------------ | ------------------------------------------------------------ |
| **設計フェーズの明確化** | 「まずは何を作るか」「どう作るか」をClaudeに考えさせるためのモード。いきなりコードを書かせない。 |
| **暴走防止**             | ClineやCopilotのように“いきなり実装”してしまうAIの暴走を防ぎ、**人間の意図と整合した計画**を立てられる。 |
| **チーム開発との相性◎**  | Planモードで出力された設計をレビュー・承認してから実装に進めるため、**チームでの合意形成がしやすい**。 |

------

### 🛠️ 使い方のイメージ

1. `claude` でREPLを起動
2. `Planモードをオンにして` と入力**(または `Shift + Tab` でモード切替)**
3. 「このアプリの設計を考えて」と自然言語で指示
4. Claudeが**構成・技術選定・画面遷移・データ構造**などを提案
5. 計画が固まったら、モードを切り替えて**Actモード（実装フェーズ）に移行**

------

### 🧪 例：ホラーゲームのWebアプリを作る場合

Planモードでの出力例：

- 「プレイヤーの行動を記憶する“何か”が必要」
- 「正気度システムを導入して、UIに影響を与える」
- 「技術スタックはReact + Three.js + Zustand」

このように、**人間顔負けの設計案**を出してくれるのがPlanモードの魅力です。

------





## レート制限（使用制限）の回避・緩和

**トークン消費の最適化**と**チャットの分割戦略**がカギになります👇

- **チャットをトピックごとに分割する**
   長い会話を続けると、Claudeは**毎回履歴全体を参照**するためトークン消費が増えます。
   → 新しい話題は**新しいチャットで開始**するのがベスト。
  
   ClaudeCodeで必要なタスクが終わったり関係ない話をする場合は`/clear`を実行してコンテキストをすべて消去しましょう。

- **入力情報を最小限に絞る**
   長文の資料をそのまま貼るのではなく、**必要な部分だけを抽出**して渡すことで負荷を軽減できます。

- **事前に要約してから渡す**
   ChatGPTやGeminiなど他のAIで要約してからClaudeに渡すと、トークン節約になります。

- **段階的に作業を進める**
   **一度に大きなタスクを処理せず、小さな単位に分けて**やり取りするのが効果的です。

- **トークン消費を意識したプロンプト設計**
   冗長な説明や繰り返しを避け、**簡潔な指示**を心がけましょう。

- **定期的にチャットをクリアする**
   同じスレッドで延々と続けるより、**定期的に新しいチャットを立てる**ことで制限にかかりにくくなります。

- **他のAIツールと併用する**
   Claudeだけに頼らず、ChatGPTやGeminiなどと**役割分担**することで負荷を分散できます。

  

---



## Context MPC server

日本語ドキュメント：
https://github.com/upstash/context7/blob/master/docs/README.ja.md

最新のドキュメントとコード例を直接取得し、プロンプトに直接配置します。



### インストール方法：

ターミナルから以下のコマンドでインストール。Claude Codeの中ではないので注意

`claude mcp add context7 -s project -- npx -y @upstash/context7-mcp`

**pnpm**パッケージマネージャーを使う場合はこちら

```bash
claude mcp add context7 -s project -- pnpm dlx @upstash/context7-mcp@latest
```



### MCP確認方法：

**`/mcp`と入力してみてください**
インストールしたMCPサーバーリストが表示されたらとりあえずOK

通常のプロンプトのあとに **use context7**を追加するだけ。
`Appルーターで基本的なNext.jsプロジェクトを作成します。 use context7`

