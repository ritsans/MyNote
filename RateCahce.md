## 3つの為替レート取得システム

### Rate.tsx - 基本的な為替レート変換コンポーネント

  - API: ExchangeRate-APIを使用（USD→JPY）
  - 機能: ドル金額を入力して円に変換
  - キャッシュ: 24時間有効なLocalStorageキャッシュ
  - 動作:
    - ユーザーがドル金額を入力
    - 「円に変換」ボタンで変換実行
    - **キャッシュされた為替レートがあれば使用、なければAPIから取得**
    - 結果を**整数円**で表示

### RateCache.tsx - カスタムフックを使った変換コンポーネント

  - 依存: RateHock.tsxのカスタムフックuseUsdToJpyRateを使用
  - 機能: より洗練されたUI/UXでドル→円変換
  - 特徴:
    - 通貨フォーマット（¥記号付き）で表示
    - **為替レート取得状態を管理**
    - **免責事項を付ける**

### RateHock.tsx - 為替レート取得カスタムフック

  - 役割: 為替レート取得ロジックを再利用可能な形で提供
  - キャッシュ: **24時間有効なLocalStorageキャッシュ**
  - API: **環境変数からAPIキーを取得**
  - 動作:
    - **コンポーネントマウント時に自動的に為替レート取得**
    - **キャッシュが有効なら即座に返す**
    - **無効/未存在なら新しいAPIリクエスト実行**

  共通の仕組み: 全てExchangeRate-APIを使用し、24時間のキャッシュでAPI呼び出しを最小化しています。

---

**1日1回のAPI取得**: 初回アクセス時または日付が変わった時のみ為替レート新たに取得

**ローカルストレージでキャッシュ**: APIコール回数を最小限に抑制

**フォールバック機能**: API取得失敗時は概算値（150円/USD）を使用。その旨をユーザーへ伝える

**トグルスイッチ**: 画面上部の「日本円に変換」でON/OFF切り替え

**日本人向け表示**: 小数第1位までの四捨五入で表示

トグルスイッチのtailwindcssのコード例：

https://flowbite.com/docs/forms/toggle/
