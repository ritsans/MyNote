# SPA + MPA ハイブリッド構成 設計メモ
_最終更新: 2025-07-26_

本ドキュメントは、構築中Reactプロジェクトの SPA 部分と、静的 HTML で提供する MPA 部分を同じリポジトリに併存させて運用するための要点をまとめたものです。

**ページ数が少ないうちはテンプレートエンジンを導入せず、純粋な HTML と軽量 JavaScript で完結させる方針**を前提にしています。



## 1. ページ構成
| URL | 役割 | 実装 |
|-----|------|------|
| `/` | ランディングページ | 静的 HTML |
| `/about` | 会社概要 | 静的 HTML |
| `/privacy-policy` | プライバシーポリシー | 静的 HTML |
| `/contact` | お問い合わせフォーム | 静的 HTML + 外部送信サービス |
| `/app/login` | ログイン | React SPA |
| `/app/signup` | サインアップ | React SPA |
| `/app/dashboard` | メインダッシュボード | React SPA |
| `/app/logout` | ログアウト処理 | React SPA |

### 1.1 ディレクトリツリー（例）
```text
project-root/
├── public/
│   ├── index.html
│   ├── about.html
│   ├── privacypolicy.html
│   ├── contact.html
│   ├── 404.html
│   ├── _redirects
│   ├── css/
│   │   └── static.css
│   └── js/
│       └── static.js
├── src/
│   ├── App.tsx
│   ├── main.tsx
│   └── pages/
│       ├── Login.tsx
│       ├── Signup.tsx
│       ├── Dashboard.tsx
│       └── Logout.tsx
└── vite.config.ts
```

## 2. ルーティングとビルドの方針
* **SPA 側**  
  `BrowserRouter` に `basename="/app"` を設定し、`/app/` 配下のみ React Router が制御します。  
  ビルド後は `dist/app/` 以下にバンドルが出力されます。
* **MPA 側**  
  `public/` に置いた HTML はビルド後そのまま `dist/` 直下へコピーされます。  
  静的ページから SPA へは `<a href="/app/login">` のようにフルリロードで誘導します。

## 3. デプロイ本番環境(Netlify)での公開設定
* **404 対応** 
  `public/404.html` … 静的ページ用 404  
  SPA 内部の 404 は `<Route path="*">` でコンポーネントを用意。
* **SPA ルート維持**  
  `public/_redirects` に次を記述  
  
  ```
  /app/*   /app/index.html   200
  ```

## 4. SEO / OGP の最低限
各静的ページの `<head>` に次を追加しておくと、検索・SNS で表示が安定します。
```html
<title>サービス名 – ページ名</title>
<meta name="description" content="ページ概要" />
<meta property="og:title" content="サービス名 – ページ名" />
<meta property="og:description" content="ページ概要" />
<meta property="og:image" content="/ogp.png" />
```

## 5. 静的ページでの CSS と JavaScript
* 共通スタイルは `public/styles/static.css` にまとめ、各 HTML から読み込む。
* 軽量な DOM 操作は `public/scripts/static.js` に集約。  
  例：ハンバーガーメニュー開閉、スムーススクロールなど。
* 静的ページから Vite バンドル（ハッシュ付きファイル名）を直接参照しない。

## 6. お問い合わせフォーム
### 6.1 Netlify Forms
```html
<form name="contact" method="POST" data-netlify="true">
  <input type="text" name="name" required />
  <input type="email" name="email" required />
  <textarea name="message" required></textarea>
  <button type="submit">送信</button>
</form>
```
* 必須項目と Email 形式は HTML5 属性でバリデーション可能。

### 6.2 Formspree など外部のフォームサービスを使う
```html
<form action="https://formspree.io/f/xyz123" method="POST">
  …
</form>
```

## 7. 今後ページが増えた場合
* ページが 5 枚を超えたらテンプレートエンジン（EJS / Nunjucks など）の導入を検討してください。
* カラースキームや共通クラスは SPA / MPA 共通の CSS トークンとして早めに整理しておくと保守が楽になります。

---

以上を守れば、静的ページと React SPA が衝突せず、開発も運用も無理なく進められます。
