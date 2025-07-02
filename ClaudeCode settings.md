## Claude.md ファイルに日本語に関する設定を記述

＊＊重要＊＊ このプロジェクトでは、Claude Codeは必ず日本で回答してください。

と先頭に記述しておく。



## 前回の続きから

もし誤ってターミナルを閉じてしまった場合でもすぐに再開できます

* `claude --continue または-c `で、直前の会話から続けることができます。
* `claude --resume または -r `で、過去の会話履歴を表示します。特定の会話履歴から続けることができます。

## モードの切り替え
Shift + Tabを押すと、モードを切り替えられます。

### auto-accept edits on

ファイル編集時の確認を行わず、ガンガン進めてくれます。
ただし、Bash実行など、コマンドの実行は確認されます。
※ permissonsに許可するコマンドを記載していれば、確認されません

### plan mode on

事前にどのような変更を行うのか、整理した上で進めてくれるので便利です！

## IDE用のセットアップ

* VSCode

VSCodeでターミナルを開いて、claudeと打つだけで、勝手にプラグインが追加されます。
`/ide`で設定状況を確認できます。

## @をつけて、ファイルやフォルダを指定

* `/clear` で、現在の会話履歴をクリアします
  
* `/compact` で、現在の会話履歴は残しますが、コンテキストをサマリしたものだけを残します (tokenの節約になります)

## Hocks (新機能) 
hooks を設定するために `/hooks` カスタムスラッシュコマンドを使用できます。
もしくは Claude Code の設定ファイル（`.claude/settings.json`）を直接編集することも可能。

### ここではコーディングが終わったらprettierでコードを自動整形するようにしましょう。

VScode使えば拡張機能でやれそうだけども。

1. `/hocks`コマンドを実行

2. どのタイミングで hooks が実行されるのかを選択。
   4 つの *hooks event* が用意されています。

   - PreToolUse - Before tool execution    ツール実行前

   - PostToolUse - After tool execution　　ツール実行後

   - Notification - When notifications are sent　　通知が送信されるタイミング

   - Stop - Right before Claude concludes its response　これ以上応答がなくなったとき

3. *matcher* を追加します。
   matcher はどのツールが呼び出された後に hooks が実行されるかを指定できます。

   Claude Code によって**ファイルが変更された場合**に prettier を実行してもらいたいので、`Write|Edit|MultiEdit` を入力します。`|` で区切ることで複数の matcher を指定可能。これにより、**ファイルの書き込み、編集、複数のファイルの編集**が行われた場合に このhooks が実行されます。

4. hooks のコマンドを入力する

   続いて hooks が呼び出されたときに実行されるコマンドを入力します。

   例えばNPMコマンドなら`npx prettier --write './src/**/*.{js,ts,jsx,tsx,css,scss,md,json}`とか？

5. 最後に hooks をどの場所に保存するかを選択します。

   - `.claude/settings.local.json`: プロジェクト単位（ローカル）
   - `.claude/settings.json`: プロジェクト単位
   - `~/.claude/settings.json`: ユーザー単位（グローバル）

以上で`.claude/settings.json`に追加したHockの項目が追加されているはずです。

### 簡単な例：セッション終了時に通知音を鳴らす

Claude Code を使っていると、AI がタスクを完了したタイミングが分かりにくいことがありますよね。じっとみてるのも時間もったいない。そこでHooks を使ってセッション終了時に通知音を鳴らすことで、この問題を解決しようと思います。
Macの標準コマンド使ってるのでMacしか使えません。Windowsは音出すコマンド見つけてください。

### macOS での実装

`~/.claude/settings.json` に以下の設定を記述します：

```json
{
  "hooks": {
    "Stop": [
      {
        "matcher": "",
        "hooks": [
          {
            "type": "command",
            "command": "afplay /System/Library/Sounds/Glass.aiff"
          }
        ]
      }
    ]
  }
}
```

この設定では、`Stop` イベント（Claude Code が応答を終了するタイミング）で macOS のシステムサウンドを再生してくれます。
上記の設定は macOS での`afplay` コマンドを使用しています。`afplay` は macOS に標準搭載されている音声再生コマンドです。

参考リンク：
https://zenn.dev/ksuke_sun/articles/b9f0cc0d89496b
