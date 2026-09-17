# chess-backend-dist

P2P Chess の **接続先ポインタと静的データの配信先** (GitHub Pages)。

- 中身は `chess-backend` (private) が生成して push する。**手で編集しない。**
- `rendezvous.json` だけは自宅 PC のバックエンドが起動・停止のたびに書き換える。

Base URL: `https://pad01g.github.io/chess-backend-dist/`

```
api/v1/rendezvous.json        シグナリングサーバの接続先と ICE 設定 (動的)
api/v1/health.json            配信の生存確認
api/v1/meta.json              最低サポートバージョン、お知らせ
api/v1/openings/index.json    定跡データ (静的)
api/v1/puzzles/index.json     課題局面 (静的)
```

## rendezvous.json がある理由

アプリは接続先を**ハードコードしない**。必ずこの JSON を読んでから繋ぐ。
そうしておくと、シグナリングサーバの置き場所 (Cloudflare Worker /
cloudflared のトンネル) を変えても、アプリを配り直さずに切り替えられる。

GitHub Pages の CDN は push から数分遅れることがあるため、
アプリ側は `?t=<timestamp>` を付けて取得する。
