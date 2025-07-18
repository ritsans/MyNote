# odoMeter（オドメーター）
**demo:**
https://claude.ai/chat/4b1d173f-6633-49fe-a69a-c820d3dc6291

スロットロールのように数字を回転させるアニメーションのこと。githubに公開されているodometer.jsが一番手軽に導入できるのでこれを使おう。

~~**odometer.js**
 https://github.com/HubSpot/odometer~~

**メンテされなくなったのでフォークを使う(tm-odometer.js)**

https://github.com/mtmarco87/tm-odometer

「サマリーの各通貨カードで、  
- 円（JPY）：赤色の文字色＋下枠線  
- ドル（USD）：緑色の文字色＋下枠線  
- ユーロ（EUR）：濃い青色の文字色＋下枠線  
にしたい

## CDNを読ませる

ルートディレクトリの**index.html**にCDNを配置

```html
<!DOCTYPE html>
<html lang="ja">
  <head>
    <!-- 他の<meta>などをここに -->
    <!-- CDN をここに追加 -->
    <script src="https://cdn.jsdelivr.net/npm/animatejs@x.y.z/dist/animate.min.js"></script>
  </head>
  <body>
    <div id="root"></div>
    <!-- ここはアプリ本体の読み込み -->
    <script type="module" src="/src/main.tsx"></script>
  </body>
</html>
```
HTMLファイルに直接読ませることでreact本体よりも先に読み込まれ、TSチェックも通らない（今回古いライブラリなので）

## Subscription-reactへの適用

`Object.entries(groupedByCurrency).map` 内の `<div key={currency} ...>` の部分を、下記のように修正してください。

```tsx
// ... existing code ...
{Object.entries(groupedByCurrency).map(([currency, subs]) => {
  const { monthlyTotal, yearlyTotal } = calculateTotals(subs);
  const symbol = getCurrencySymbol(currency);
  const name = getCurrencyName(currency);

  // 通貨ごとのスタイルを定義
  let borderColor = '';
  let textColor = '';
  switch (currency) {
    case 'JPY':
      borderColor = 'border-b-4 border-red-500';
      textColor = 'text-red-600';
      break;
    case 'USD':
      borderColor = 'border-b-4 border-green-500';
      textColor = 'text-green-600';
      break;
    case 'EUR':
      borderColor = 'border-b-4 border-blue-900';
      textColor = 'text-blue-900';
      break;
    default:
      borderColor = 'border-b-4 border-gray-400';
      textColor = 'text-gray-700';
  }

  return (
    <div
      key={currency}
      className={`mb-4 last:mb-0 bg-white p-4 rounded shadow ${borderColor}`}
    >
      <h3 className={`text-lg font-semibold mb-2 ${textColor}`}>{name}</h3>
      <div className="grid grid-cols-2 gap-4">
        <div className="text-center">
          <p className="text-sm text-gray-600">月額合計</p>
          <p className={`text-2xl font-bold ${textColor}`}>
            {symbol}{Math.round(monthlyTotal).toLocaleString()}
          </p>
        </div>
        <div className="text-center">
          <p className="text-sm text-gray-600">年額合計</p>
          <p className={`text-2xl font-bold ${textColor}`}>
            {symbol}{Math.round(yearlyTotal).toLocaleString()}
          </p>
        </div>
      </div>
      <p className="text-sm text-gray-500 mt-2">
        {name}のサブスクリプション数: {subs.length}個
      </p>
    </div>
  );
})}
// ... existing code ...
```

---

### 補足
- `border-b-4` はTailwind CSSの「下枠線4px」クラスです。
- `text-red-600` などは文字色を指定しています。
- `bg-white p-4 rounded shadow` でカード風の見た目を強調しています（不要なら外してください）。

---

**この修正で、各通貨ごとに目立つ色の文字＋下枠線が付きます。**  

odo-meter_requirements.md

要件整理：サマリー価格表示にオッドメーターアニメーション追加計画 

* ライブラリ: tm-odometer.js (odometer.js代替)
* 対象: Summary.tsxの月額合計と年額合計の価格表示
  * アニメーション: 初回ロード時と価格変更時の両方でスムーズなカウントアップ効果
  * 月額合計・年額合計の表示部分をオッドメーター要素に変更
  * 初回ロード時: 0から実際の価格までアニメーション
  * 価格変更時: 前の値から新しい値へアニメーション
* アニメーション設定
  * 初回表示時: 0 → 実際の金額へのカウントアップ 
  * duration: 800ms程度でスムーズな動作
  * 通貨シンボルはアニメーション対象外
* 通貨別に複数のオッドメーターインスタンスを管理 
* 初回ロード時も含めて全ての価格変更でアニメーション実行



---

SuperClaude deepwiki
https://deepwiki.com/NomenAK/SuperClaude

基本コマンドだけでペルソナやツール、MCPサーバーを自動で選択するため細かく指定する必要はないんだとか

/sc:implement

