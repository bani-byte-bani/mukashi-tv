# 昔よくみてた番組

生まれた年と学年を選ぶと、その頃の番組・流行語・社会現象・出来事を年ごとに表示します。

## ファイル構成
- public/index.html … 画面とロジック
- public/data/programs.json … 番組（放送期間で管理）
- public/data/years/YYYY.json … 年ごとの流行語・社会現象・出来事（＋将来の音楽/映画/書籍）

## 番組の書き方（programs.json）
{"title": "名探偵コナン", "genre": "anime", "ch": "日テレ", "periods": [[1996, null]]}
- genre: anime / tokusatsu / drama / variety / uta / kids
- periods: [開始年, 終了年]。null は放送中。再放送・シリーズの中断は複数期間で書く

## 年ファイルの書き方（years/1995.json）
- buzzwords: [{"w": "流行語", "note": "受賞区分など（任意）"}]
- trends / events / music / movies / books: 文字列の配列
- sources: 出典メモ

## 収録範囲（試作）
- 生まれ年 1981〜1991（35〜45歳）、データ 1983〜2010年
- 対象を広げるときは public/index.html の BIRTH_MIN / BIRTH_MAX を変え、年ファイルを追加する

## ローカルで動かす
`data/*.json` を fetch するため、index.html を直接開くのではなくサーバー経由で開く。

```
npx wrangler dev        # http://localhost:8787
# または
cd public && python3 -m http.server
```

## デプロイ（Cloudflare Workers）
`wrangler.toml` の `[assets]` で `public/` を静的配信する構成（Workerスクリプトなし）。

1. Cloudflare ダッシュボード → Workers & Pages → 作成 → 「Import a repository」で このリポジトリを選ぶ
2. ビルドコマンドは空欄、デプロイコマンドは `npx wrangler deploy` のまま
3. 以後は main に push するたびに自動デプロイされる（URL: `https://mukashi-tv.<アカウント>.workers.dev`）
