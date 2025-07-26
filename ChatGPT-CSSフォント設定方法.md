# CSSフォント設定方法

ユーザーが求めているのは、日本国内の日本人ユーザー向けのCSSコードですよね。macOSに関してはシステムフォントを優先し、Windows11ではNoto Sans JPがローカルにあればそれを使用し、なければCDNか自サーバーでフォントファイルをダウンロードさせる方法を考えます。この条件に沿うコードとして、`@font-face`を使って最適な優先順を指定するのが良さそうです。

```css
@font-face { font-family: "Noto Sans JP"; src: local("Noto Sans JP"), url("font") format("woff2"); unicode-range: U+4E00-9FAF;}
body { font-family: "Noto Sans JP", system-ui, sans-serif; }
```

概要
--

日本国内向けサイトでは、日本語表示に最適化された「Noto Sans JP」をデフォルトフォントとして指定しつつ、環境ごとに最適なフォールバックを設定することがポイントです。macOS環境ではシステムフォント（`system-ui`／`-apple-system`など）を優先し高速かつ美しいレンダリングを実現します[CSS-Tricks](https://css-tricks.com/snippets/css/system-font-stack/?utm_source=chatgpt.com)。一方、Windows 11ではアップデートによりローカルにNoto Sans JPが導入されている場合があるため、まず`local()`で検出して利用し、未インストール時にはCDNまたは自サーバーからフォントファイルを取得する仕組みを導入します[Adobe Community](https://community.adobe.com/t5/photoshop-ecosystem-discussions/variable-font-noto-sans-jp-not-displaying-correctly/td-p/15245729?utm_source=chatgpt.com)[GitHub](https://github.com/google/fonts/issues/1925?utm_source=chatgpt.com)。

CSSコード例
-------

以下のコードをサイトのグローバルCSS（例：`styles.css`）に追加してください。

```css
/* 1. @font-faceでローカル優先 → CDN取得 */
@font-face {
  font-family: 'Noto Sans JP';
  font-style: normal;
  font-weight: 400;
  font-display: swap;
  src:
    local('Noto Sans JP'),
    local('NotoSansJP-Regular'),
    /* Google Fonts CDN（woff2） */
    url('https://fonts.gstatic.com/s/notosansjp/v53/-F62fjtqLzI2JPCgQBnw7HFYwQgP.woff2') format('woff2');
  /* Unicode 範囲を絞ることで不要なサブセット読み込みを抑制 */
  unicode-range:
    U+3000-303F,  /* 句読点  */
    U+3040-309F,  /* ひらがな  */
    U+30A0-30FF,  /* カタカナ  */
    U+4E00-9FFF,  /* 漢字      */
    U+FF00-FFEF;  /* 全角英数 */
}

/* 2. 全体のフォントファミリ設定 */
body {
  font-family:
    'Noto Sans JP',
    system-ui,
    -apple-system,
    BlinkMacSystemFont,
    'Segoe UI',
    Roboto,
    'Helvetica Neue',
    Arial,
    sans-serif;
}
```

解説
--

**1\. `@font-face` の `src` 順序**

*   `local('Noto Sans JP')`→`local('NotoSansJP-Regular')` で、ローカルインストール済みフォントを最優先で利用。
*   次に Google Fonts の CDN から WOFF2 形式で取得。`font-display: swap` により、フォント読み込み中もシステムフォントでテキストを表示し、読み込み完了後に置き換えます[Google Fonts](https://fonts.google.com/noto/specimen/Noto%2BSans%2BJP?utm_source=chatgpt.com)。
*   `unicode-range` を指定すると、必要な文字が含まれるサブセットのみを読み込むため、特に大規模な日本語フォントで効果を発揮します[Stack Overflow](https://stackoverflow.com/questions/79648382/how-to-import-a-css-font-conditional-on-htmllang-attribute?utm_source=chatgpt.com)[web.dev](https://web.dev/articles/api-for-fast-beautiful-web-fonts?utm_source=chatgpt.com)。

**2\. フォールバックとしてのシステムフォント**  
`body` の `font-family` では、まず `Noto Sans JP` を指定し、続けて macOS の `system-ui`／`-apple-system`、Windows の `Segoe UI` や Android の `Roboto` などを列挙します。これにより、対応環境では自動的に最適なシステムフォントが選択され、レンダリングパフォーマンスが向上します。



要約
--

Google Fontsが提供する`<link>`や`@import`経由のCSSでは、幅広いブラウザ互換性を確保するために複数フォーマット（WOFF／WOFF2／TFF等）や細かなサブセット指定を自動生成します。その一方で、直接WOFF2のURLを`@font-face`の`src`に記載すると、主に以下のメリットが得られます。

*   **軽量化・最適化**：WOFF2のみを指定し、Brotliによる圧縮効果を最大化できる[web.dev](https://web.dev/articles/font-best-practices?utm_source=chatgpt.com)。
*   **読み込み制御**：`local()`によるローカルフォント優先や`unicode-range`によるサブセット制御などを細かく自前でチューニングできる[MDN Web Docs](https://developer.mozilla.org/en-US/docs/Web/CSS/%40font-face?utm_source=chatgpt.com)。
*   **HTTPリクエスト削減**：CSSファイルを経由せずフォントファイルのみを直接取得することで、DNSルックアップや追加のリダイレクトを省略できる。

* * *

1\. Google Fonts標準のCSSリンクの仕組み
-----------------------------

1.  **複数フォーマット配信**  
    Google Fontsは多様なブラウザ互換性を担保するため、WOFF2だけでなくWOFFやTFFも自動で切り替え配信します。
2.  **動的なサブセット・unicode-range**  
    利用文字に応じてサブセットを生成する一方で、CSS側でのカスタマイズには限界があります。
3.  **追加レイヤーとしてのCSS**  
    `<link href="https://fonts.googleapis.com/…">`や`@import`経由でCSSを取得し、その中で`@font-face`が定義されます。このためフォント取得前にまずCSSリクエストが発生します。

* * *

2\. 直接WOFF2のURLを指定するメリット
------------------------

### 2.1 WOFF2の圧縮率をフル活用

*   WOFF2はBrotli圧縮によってWOFFに対し約30％のサイズ削減を実現し、モダンブラウザの主流フォーマットです[web.dev](https://web.dev/articles/font-best-practices?utm_source=chatgpt.com)。
*   不要な旧フォーマット（WOFF／TFF／EOTなど）を省き、1ファイルのみを優先的にダウンロードできます。

### 2.2 ローカルフォント優先・unicode-range制御

*   `local('Noto Sans JP')`を先に記述することで、既にインストールされた場合はダウンロードをスキップできます[MDN Web Docs](https://developer.mozilla.org/en-US/docs/Web/CSS/%40font-face?utm_source=chatgpt.com)。
*   `unicode-range`を明示すれば必要な文字範囲だけを読み込むようブラウザに指示でき、初期ロードをより高速化できます。

```css
@font-face {
  font-family: 'Noto Sans JP';
  font-style: normal;
  font-weight: 400;
  font-display: swap;
  src:
    local('Noto Sans JP'),
    url('https://fonts.gstatic.com/s/notosansjp/v53/-F62fjtqLzI2JPCgQBnw7HFYwQgP.woff2') format('woff2');
  unicode-range:
    U+3000-30FF, U+4E00-9FFF, U+FF00-FFEF;
}
```

### 2.3 リクエスト最適化

*   CSS→フォントの二段階リクエストを避け、直接フォントファイルへアクセスできる。
*   DNSルックアップやHTTPヘッダー解析のオーバーヘッドを削減し、初回表示を高速化。

* * *

### 結論

Google Fonts標準のCSSリンクは手軽さと互換性に優れますが、自前でパフォーマンスや挙動を細かく制御したい場合、WOFF2のURLを直接指定する方法が有効です。特に日本語の大きなフォントでは、サブセットやローカル優先設定がページ表示速度向上に貢献します。



---
Powered by [ChatGPT Exporter](https://www.chatgptexporter.com)