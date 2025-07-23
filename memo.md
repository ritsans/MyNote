# 一時メモここ

## ユーザー登録

~~Reactアプリ学習の次のステップ。ユーザー登録したユーザーのみ、アプリを使えるようにしよう。~~

~~ダミートップページを作成、さらにログインフォームを作成し、まずはテスト用に１人のユーザーが入れるようにしよう。~~

~~RTSを適切に設定すれば、ユーザーごとにデータ参照・追加が簡単に実装できる便利な機能だ。~~

### about.htmlなどの静的ファイルを置いてリンクさせたい場合

* /public ディレクトリに静的ファイルやディレクトリを置く。
  
  /privacy や /help などその中にindex.htmlを通常通り置けば静的ファイルとして扱われる。静的ファイルなのでSEO対策とかにもなるはず。

  * 一旦Buildしないと反映されないので注意。開発サーバーでは扱ってくれない。

  * CSSなどを外部ファイルを参照している場合、パスがズレる可能性があるので実際にビルドしてからパスを確認すること。構造上、外部の/publicからSPA内部のファイルは参照できない。

### React Router v7を使う

#### `pnpm add react-router`でプロジェクトにパッケージを追加

使用用途によって３つのモードがありFrameworkモードが推奨されているが

**Declarative**モードが従来の書き方に近い最もシンプルなモードなのでこちらを使うことになる。

#### ↓こういうの

```javascript
import { BrowserRouter, Routes, Route, Link } from 'react-router-dom';

function App() {
  return (
    <BrowserRouter>
      <nav>
        <Link to="/">Home</Link>
        <Link to="/about">About</Link>
      </nav>
      <Routes>
        <Route path="/" element={<Home />} />
        <Route path="/about" element={<About />} />
      </Routes>
    </BrowserRouter>
  );
}
```

日本語ドキュメントはここ参照https://react-router-docs-ja.techtalk.jp/



## playwright の入れ方

1. MCPを追加する

   `claude mcp add playwright -s project -- pnpm dlx @playwright/mcp@latest`

   Claudeに入り/mcp と入力、リストにplaywrightがあれば**とりあえず成功**

2. 最初にMCPを実行すると **"chrome"**が入っていないと警告され、次にインストールが自動的に始まる。

   `pnpm add playright install chromium`

   

### Claude + Playwright MCP の活用（参考）

Claudeに自然言語で指示 → Playwright MCP経由でローカルブラウザを自動操作可能。

UIテストの記録・自動化、フォーム操作・認証テストの自動実行、スクリーンショット取得・E2Eコード生成

**使用例**：

```
プロンプト
"Open http://localhost:5173 and log in using test@example.com"
```



## Supabase Storage の活用

すでにSupabaseを使っているなら、Storageを使うのが最もシンプルで安全。

### 🎯 利用ステップ

1. Supabaseコンソールから `avatars` バケットを作成
2. JS SDKから画像をアップロード
3. 公開URLまたは署名付きURLで表示

### 実装例

```js
await supabase.storage
  .from('avatars')
  .upload(`${userId}/avatar.png`, file, { upsert: true })

const { publicUrl } = supabase.storage
  .from('avatars')
  .getPublicUrl(`${userId}/avatar.png`)
```

###  無料枠の目安（2024年7月時点）

| 項目           | 無料枠                         |
| -------------- | ------------------------------ |
| ストレージ容量 | 1GB                            |
| 転送量         | 2GB/月                         |
| 学習用途なら   | 数百画像レベルで無料で運用可能 |

