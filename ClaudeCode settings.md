## 簡単な例：セッション終了時に通知音を鳴らす

Claude Code を使っていると、AI がタスクを完了したタイミングが分かりにくいことがありますよね。じっとみてるのも時間もったいない気がする、そこでHooks を使ってセッション終了時に通知音を鳴らすことで、この問題を解決しようと思います。
公式で音を鳴らす設定方法あるみたいですが、なんでか上手く動かんのでHooks利用したら上手くいくようになりました。

### macOS での実装（動作確認済み）

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
