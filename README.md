# ブロークン島 入国審査 — 公開キット

英語が下手な人しか入国できない島の入国審査。日英対応・計測付きの配布用スモークテスト版です。

## 同梱ファイル

- `index.html` — アプリ本体(日英自動切替、GoatCounter計測、OGP対応)。日本語圏への配布はこのURL
- `og.png` — 日本語版Twitterカード画像(1200x630)
- `en/index.html` — **英語圏配布用ページ**(英語デフォルト+英語OGカード)。英語圏へはこちらのURL `…/broken-island/en/` を配布する
- `og-en.png` — 英語版Twitterカード画像
- `README.md` — このファイル

日英でURLを分けているのは、OGP画像がURL単位で固定なため(英語圏に日本語カードが出るとクリック率が落ちる)。副産物として、GoatCounter上で `/` と `/en/` のページビューが自動で分かれ、日英の反応を別々に計測できます。

## 公開手順(所要 約15分)

### 1. GoatCounter(アクセス解析)の設定 — 約3分

1. https://www.goatcounter.com/ で無料登録(メールのみ)
2. サイトコードを決める(例: `broken-island`) → あなたのダッシュボードは `https://broken-island.goatcounter.com` になる
3. `index.html` の**最終行付近**にあるこの行の `YOUR-CODE` を自分のコードに置き換える:

```html
<script data-goatcounter="https://YOUR-CODE.goatcounter.com/count" async src="//gc.zgo.at/count.js"></script>
```

### 2. GitHub Pages で公開 — 約10分

1. GitHub で新規リポジトリを作成(Public、名前は例: `broken-island`)
2. この3ファイルをアップロード(ブラウザなら「Add file → Upload files」にドラッグでOK)
3. リポジトリの Settings → Pages → Source を「Deploy from a branch」、Branch を `main` / `(root)` にして Save
4. 数分後 `https://あなたのユーザー名.github.io/broken-island/` で公開される

### 3. OGP(Twitterカード)のURLを修正

`index.html` と `en/index.html` それぞれの `<head>` 内にある `YOUR-GITHUB-USERNAME`(各2箇所)を自分のユーザー名に置き換えて再アップロード。GoatCounterのコード差し替え(手順1)も両ファイルに行うこと:

```html
<meta property="og:image" content="https://YOUR-GITHUB-USERNAME.github.io/broken-island/og.png">
<meta property="og:url" content="https://YOUR-GITHUB-USERNAME.github.io/broken-island/">
```

反映確認は https://cards-dev.twitter.com/validator か、実際にツイートのプレビューで。

## 計測の見方

GoatCounter のダッシュボードに、ページビューに加えて以下のイベントが `event/〜` というパスで記録されます:

| イベント | 意味 |
|---|---|
| `event/exam-start` | 録音を開始した(審査に挑んだ) |
| `event/exam-complete` | 5秒以上話して審査まで到達した |
| `event/result-pass` | 入国許可 |
| `event/result-deny` | 入国拒否(上手すぎ) |
| `event/result-arrest` | 下手偽装罪で逮捕 |
| `event/share-copy` | シェア文をコピーした(最重要指標) |
| `event/demo-pass` 等 | デモボタン閲覧 |
| `event/mic-error` | マイクが使えなかった(アプリ内ブラウザ等) |
| `event/lang-en` / `event/lang-ja` | 言語切替 |

### 見るべき数字(スモークテストの判定基準の目安)

- **審査開始率** = exam-start ÷ ページビュー。30%を超えたらフックが効いている
- **完了率** = exam-complete ÷ exam-start。50%未満なら録音UIに問題あり
- **シェア率** = share-copy ÷ (result-pass + result-deny + result-arrest)。**10%を超えたら「語りたくなる」仮説は本物** → コンセプトに従って本体開発へ
- `mic-error` が多い場合、Twitterアプリ内ブラウザからの流入が原因。ツイート文に「Safari/Chromeで開いてね」を添えると改善する

## 既知の制約

- 音声認識(文字起こし)は Chrome / Edge / Android Chrome で動作。ネット接続必須
- iOS Safari や Firefox では文字起こし不可 → 自動的に「耳審査」(沈黙率のみの簡易判定)にフォールバック。逮捕判定は無効化される
- 音声はどこにも送信・保存されません(認識はブラウザ機能、採点は端末内)。集めるのは匿名の集計イベントのみ

## 言語

ブラウザの言語設定が日本語なら日本語、それ以外は英語で表示。右上のボタンで切替可能。EN版はネイティブが受けると高確率で ENTRY DENIED になります(それが狙いです)。
