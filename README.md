# 知らんけど検索

質問すると、方言でそれっぽい答えを返して「知らんけど」で締める検索エンジンです。

- `index.html` … 画面
- `api/ask.js` … AIに質問を投げるサーバー側の処理（Vercel の Serverless Function）
- APIキーは画面には出ません。環境変数 `ANTHROPIC_API_KEY` として Vercel に登録します。

## 公開の手順（Vercel）

1. この4つのファイル（`index.html`、`api/ask.js`、`package.json`、`README.md`）を、そのままの構成で GitHub の新しいリポジトリに置きます。
2. https://vercel.com にログインし、「Add New → Project」でそのリポジトリを選んで Import します。設定は初期値のままで構いません。
3. Import の画面（または Project の Settings → Environment Variables）で、次を追加します。
   - Name: `ANTHROPIC_API_KEY`
   - Value: Anthropic Console（https://console.anthropic.com）で作ったAPIキー
4. Deploy を押すと、`https://〜.vercel.app` のURLができます。

環境変数は追加したあとに再デプロイ（Deployments → 該当のデプロイ → Redeploy）しないと反映されません。

## 費用の守り方

- Anthropic Console の Billing で、月の利用上限（spend limit）を必ず設定してください。
- `api/ask.js` のレート制限（1分に10回/IP）は簡易的なもので、サーバーのインスタンスごとに数えるため完全ではありません。
- 使うモデルは `api/ask.js` 冒頭の `MODEL` で変えられます。
- 質問は100文字まで、返事は最大300トークンに制限しています。

## 手元で試す

```
npm i -g vercel
export ANTHROPIC_API_KEY=（自分のキー）
vercel dev
```

http://localhost:3000 で開きます。

## 方言を足す・直す

- 返事の口調と締めのことば … `api/ask.js` の `DIALECTS`
- 画面の文言とAIを使えないときの返事 … `index.html` の `D`
- 切り替えボタン … `index.html` の `<div class="dial">`
