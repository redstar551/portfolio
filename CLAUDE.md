# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## 概要

映像作家・坂根大悟のポートフォリオサイト。https://daigosakane.com で Cloudflare Pages により公開。`main` へのプッシュで自動デプロイされる。オーナーは GitHub の Web UI 上で直接編集することが多い（「Add files via upload」「Update index.html」などのコミット）。本文は日本語と英語の両方を持ち、言語切り替えで出し分ける（後述）。

ビルド工程・パッケージマネージャ・リンター・テストは存在しない。各ページは単一の自己完結型 HTML ファイルで、`<style>` をインラインで持つ（`<script>` もインライン）。プレビューはブラウザでファイルを直接開くか、ディレクトリを静的配信する：

```bash
python -m http.server 8000
```

`.claude/launch.json` に同じコマンドが `portfolio` という名前で登録されている（プレビューツールの `preview_start` で使う）。

README.md は古い（`work-template.html` の記述、列車が「左に」走り去るなど）。食い違ったときはこのファイルと実際のコードを正とする。

## 構成

- `index.html` — トップページ：固定ナビ（右上のみ。ロゴは廃止し、作品一覧の下に「坂根大悟 ＝ Daigo Sakane」を表示）、Works グリッド、SNS（`#sns`）、列車フッター。ナビの About は `about.html`、Contact は `contact.html` へ遷移する。グリッチ演出はすべて末尾のインラインスクリプト1つにまとまっている（スクロール連動の RGB ズレ／ストライプ／テキストグリッチ、定期的なアンビエントグリッチ、`#` ナビリンクのスムーズスクロール）。
- `<作品スラッグ>.html`（例：`jinko-haikyo-m2.html`、`e-waste-invaders.html`）— 作品個別ページ。スタイルシートとマークアップの骨格は全ページで同一のものを複製している：上部の動画埋め込み（`.work-video-top` の iframe、YouTube または Vimeo。動画がない作品は `.work-video-top img` で画像を置く）→ `.work-title` / `.work-meta` → 日本語の `.work-description` → `.work-details` の各行（Year/Genre/Venue/Tech）→ 任意の `.work-gallery` → `.work-english` セクション → `.work-nav`（左 `← Prev`・中央 `All Works`・右 `Next →`。トップのカードの並び順につなぎ、最初の作品は Prev、最後の作品は Next の代わりに空の `<span>` を置く）→ フッター。作品ページの JS はロゴのグリッチだけ。
- `about.html` — プロフィールと経歴。作品ページと同じ CSS 基盤に、自己紹介（`.about-bio`）・プロフィール写真・経歴（`.about-cv` の `.cv-block`／`.cv-item`、日本語版と英語版）を置く。以前はトップページ内の `#about` にあったが独立させた。
- `contact.html` — お問い合わせフォーム（名・姓・メール・件名・本文）。作品ページと同じ CSS 基盤。送信は FormSubmit（formsubmit.co）経由で diego.sakane@gmail.com に届く。JS が `<form action>` の URL を `/ajax/` 版に変えて fetch で送る。有効化後はアドレス部分を FormSubmit 発行のランダム文字列に置き換えてアドレスを隠す。
- 言語切り替え：全ページのナビ右端に `.lang-toggle`（EN ⇄ 日本語）がある。日本語の要素に `.lang-ja`、英語の要素に `.lang-en` を付け、`html[lang="en"]` で出し分ける。選んだ言語は `localStorage.lang` に保存し、`<head>` の小さなスクリプトが読み込み時に `<html lang>` へ反映する。作品ページでは日本語の `.work-description.lang-ja` の直後に英語の `.work-description.lang-en` を置く（以前の末尾 EN セクションは廃止）。作品ページを追加するときは英語版の本文も用意する。
- 作品ページの `.work-detail-label` は `<span class="lang-ja">年</span><span class="lang-en">Year</span>` のように両言語を持つ（年／ジャンル／展示場所／使用技術／映画祭／上映時間 など）。
- 英字の見出し・ナビ・`← Works` などのページ下リンク・列車の文字（DAIGO / SAKANE）・言語ボタンは、テキストではなく **GD高速道路ゴシックJA をアウトライン化したインライン SVG**（`<svg class="gd" role="img" aria-label="…">`）。フォントファイルはリポジトリに置かない（配布しない）方針なので、文言を変えたり新しいラベルを足したりするには、そのフォントのグリフから `<path>` を生成し直す必要がある。テキストを書き換えるだけでは反映されない。グリッチ対象（`.glitch-text`）の SVG は `<defs>` の1つの path を `<use class="gd-c">`・`gd-b`・`gd-m` の3層で参照して色ズレを出す。path の `id`（例：`gd-about-6993`）はページ内で一意にする。
- トップのカードの作品名・年号は、フォントポにほんご（日本語／IPAフォントライセンス）と Arimo（英数字／Apache）のアウトラインを合成したインライン SVG（`<svg class="cardtext">`）。em 基準で高さをそろえているので、`height: 1.1em` で文字サイズと一致する。作品名を変えるときは SVG を作り直す。
- 列車の車体は、鉄路タイプフェイスのアウトライン約190語を放射状に配置したインライン SVG（`.train-words`）。列車の絵は `scaleX(-1)` で反転表示されるため、先頭部（曲線）は画面右側にある。配置座標を変えるときはこの反転を考慮する。
- `index.html` の `.works-grid` はブラウン管テレビの筐体風（角丸の画面、下にスピーカー・つまみ・脚、ホバーで画面がふくらむ `::before`/`::after` と transform）。列は 3 列、1100px 以上で 4 列、768px 以下で 2 列、480px 以下で 1 列。ホバー演出は transform と opacity だけを動かす。
- CSS はファイルごとにコピーされているため、サイト全体に効かせたいスタイル変更はすべての作品ページ（共通ルールなら `index.html` も）に反映する必要がある。

## 作品の追加

1. 既存の作品ページをケバブケースの新しいファイル名でコピーし、タイトル、`<meta name="description">`、動画 URL、本文、詳細行を書き換える（README には `work-template.html` とあるが、現在リポジトリには存在しない）。
2. `index.html` の `.works-grid` 内で、該当カードを `<a href="new-work.html" class="work-card reveal glitch-block">` に変更（または追加）する。個別ページがまだないカードは `<div>`。カードは年号＋タイトル＋ジャンルのみで、`.work-bg` にサムネイル `<img class="thumb" src="images/thumb-<スラッグ>.jpg">`（4:3、1280×960 目安）を置く。サムネイルがないカードは装飾用のインライン SVG のまま。並びは新しい順を維持する。
3. 日本語説明文末尾のクレジット段落では、展示情報の見出しを「発表：」ではなく「展示場所：」とする（例：`展示場所：東京藝術大学 元町中華街校舎（横浜）／GEIDAI GAMES 07（上野）`）。
4. 使用中のジャンル表記：Game Installation、Installation、Video Installation（映像インスタレーション）、Video Work（映像作品）、CG、Short Film、Game、Event（`Installation / CG` のような組み合わせも可）。

画像は `images/` に置き、`images/<名前>.jpg` で参照する（カード用 `thumb-<スラッグ>.jpg`、動画がない作品ページの上部用 `top-<スラッグ>.jpg`）。

各ページの `<head>` には SNS 共有用の OGP タグ（`og:title` / `og:description` / `og:url` / `og:image`、`twitter:card`）がある。`og:image` は絶対 URL で `https://daigosakane.com/images/og/<スラッグ>.jpg`（1200×630。トップとお問い合わせは作品サムネイルを並べた `index.jpg`）、`og:url` は Cloudflare Pages が `.html` を省くため `https://daigosakane.com/<スラッグ>`。作品ページを追加したら、この画像とタグも用意する。作品ページのギャラリーブロックは画像追加までコメントアウトされている。

## デザイン上の制約

ロトチェンコ × カッサンドル風の構成主義デザインにグリッチ演出を重ねる。各ファイルの `:root` で定義された CSS 変数を使う：ネイビー `#062C54`、アクセント `#4A9EFF`、黒 `#0A0A0A`、クリーム／白 `#F0ECE3`。フォントは Bebas Neue（ディスプレイ）、Space Mono（本文）、Noto Sans JP（日本語）を基本に、ロゴ「坂根大悟」と作品ページの大見出しは DotGothic16（`--font-dot`）、ナビと「坂根大悟 ＝ Daigo Sakane」は Noto Sans JP の細字（100）。これらは Google Fonts から読み込む（英字の見出しやラベルは前述の GD高速道路ゴシックJA のアウトライン SVG）。ロゴには全ページ共通の CSS（`.rgb-glitch`）と末尾のスクリプトで、数秒おきに色収差のグリッチが入る。維持すべき特徴的な要素：全ページの全画面 `.scanlines` オーバーレイ、`index.html` フッターのカッサンドル「Nord Express」風の列車シルエット（右向き。ホバーすると線路 `.train-track` を残して車体 `.train-body` だけが右上へ走り去る）。
