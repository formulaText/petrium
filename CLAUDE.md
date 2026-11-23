# ペトリウム 開発メモ（Claude Code 向け）

## 構成
- `petrium-prototype.html`：ゲーム本体（Three.js を同梱した単一HTML。外部読み込みは使わない）。
  - シミュレーション（ゲームの決まり）は `Sim`（`<script>/* ===... * Sim` の塊）。描画は別の `<script>`。
  - コード内のコメントは日本語。直した箇所にも日本語で短く理由を書く。
  - 調整できる数値は `P`（パラメータの表）に名前を付けて置く。
- `index.html`：`petrium-prototype.html` へ移動するだけ。
- `.github/workflows/pages.yml`：main をサイトのトップ、dev を `/dev/` にして `gh-pages` ブランチへ書き出す。GitHub Pages は `gh-pages` を表示する。
- `CHANGELOG.md`：更新まとめ（元の開発スレッドへの共有用）。新しいものが上。

## 公開URL
- 公開版（main）：https://formulatext.github.io/petrium/
- 開発版（dev）：https://formulatext.github.io/petrium/dev/

## 作業ルール（持ち主との約束）
- 修正は dev に入れる。main への反映（マージ）は、持ち主の許可があるときだけ。
- ファイルやブランチの削除、リポジトリの公開設定の変更は、必ず許可を取る。
- 返答は日本語。専門用語は初出で一言説明。持ち主は iPhone のみで操作する。
- 持ち主への操作依頼は、画面とボタンの名前で具体的に。質問は、場面と前提を先に書く。
- 毎回の返答の最後に「今どこまで進んだか」を短い表で示す。
- 不具合修正・新機能などの節目と、頼まれたときに、**最新版の単一HTML** と **`CHANGELOG.md` に書いた更新まとめ** を持ち主にファイルで渡す。

## 確かめ方（Sim は描画なしで動かせる）
```js
const fs = require('fs');
const html = fs.readFileSync('petrium-prototype.html', 'utf8');
const src = html.match(/<script>(\/\* =+\s*\n \* Sim[\s\S]*?)<\/script>/)[1];
const Sim = new Function(src + '\nreturn Sim;')();
const M = Sim.build(10);
const S = M.create({ level: 3, seed: 1 });
// M.step(S) で1刻み（1/60秒）。出来事は S.events（使ったら S.events.length = 0）
// M.Debug.* に検証用の操作がある（pherHere / pherHot など）
```
