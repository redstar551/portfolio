# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## 概要

映像作家・坂根大悟のポートフォリオサイト。https://daigosakane.com で Cloudflare Pages により公開。`main` へのプッシュで自動デプロイされる。オーナーは GitHub の Web UI 上で直接編集することが多い（「Add files via upload」「Update index.html」などのコミット）。本文は日本語で、英語セクションを併記する。

ビルド工程・パッケージマネージャ・リンター・テストは存在しない。各ページは単一の自己完結型 HTML ファイルで、`<style>` をインラインで持つ（トップページのみ `<script>` もインライン）。プレビューはブラウザでファイルを直接開くか、ディレクトリを静的配信する：

```bash
python -m http.server 8000
```

## 構成

- `index.html` — トップページ：固定ナビ、Works グリッド、About、SNS（`#sns`：Instagram / X へのリンク）、列車フッター。ナビの Contact は `contact.html` へ遷移する。グリッチ演出はすべて末尾のインラインスクリプト1つにまとまっている（スクロール連動の RGB ズレ／ストライプ／テキストグリッチ、定期的なアンビエントグリッチ、`#` ナビリンクのスムーズスクロール）。
- `<作品スラッグ>.html`（例：`jinko-haikyo-m2.html`、`e-waste-invaders.html`）— 作品個別ページ。スタイルシートとマークアップの骨格は全ページで同一のものを複製している：上部の動画埋め込み（`.work-video-top` の iframe、YouTube または Vimeo。動画がない作品は `.work-video-top img` で画像を置く）→ `.work-title` / `.work-meta` → 日本語の `.work-description` → `.work-details` の各行（Year/Genre/Venue/Tech）→ 任意の `.work-gallery` → `.work-english` セクション → `.work-nav` → フッター。作品ページに JS はない。
- `contact.html` — お問い合わせフォーム（名・姓・メール・件名・本文）。作品ページと同じ CSS 基盤。送信は FormSubmit（formsubmit.co）経由で diego.sakane@gmail.com に届く。JS が `<form action>` の URL を `/ajax/` 版に変えて fetch で送る。有効化後はアドレス部分を FormSubmit 発行のランダム文字列に置き換えてアドレスを隠す。
- CSS はファイルごとにコピーされているため、サイト全体に効かせたいスタイル変更はすべての作品ページ（共通ルールなら `index.html` も）に反映する必要がある。

## 作品の追加

1. 既存の作品ページをケバブケースの新しいファイル名でコピーし、タイトル、`<meta name="description">`、動画 URL、本文、詳細行を書き換える（README には `work-template.html` とあるが、現在リポジトリには存在しない）。
2. `index.html` の `.works-grid` 内で、該当カードを `<a href="new-work.html" class="work-card reveal glitch-block">` に変更（または追加）する。個別ページがまだないカードは `<div>`。カードは年号＋タイトル＋ジャンルのみで、`.work-bg` に装飾用の小さなインライン SVG を持つ。並びは新しい順を維持する。
3. 日本語説明文末尾のクレジット段落では、展示情報の見出しを「発表：」ではなく「展示場所：」とする（例：`展示場所：東京藝術大学 元町中華街校舎（横浜）／GEIDAI GAMES 07（上野）`）。
4. 使用中のジャンル表記：Game Installation、Installation、CG、Short Film、Game、Event（`Installation / CG` のような組み合わせも可）。

画像は `images/` に置き、`images/<名前>.jpg` で参照する。このフォルダはまだ存在せず、既存のギャラリーブロックは画像追加までコメントアウトされている。

## デザイン上の制約

ロトチェンコ × カッサンドル風の構成主義デザインにグリッチ演出を重ねる。各ファイルの `:root` で定義された CSS 変数を使う：ネイビー `#062C54`、アクセント `#4A9EFF`、黒 `#0A0A0A`、クリーム／白 `#F0ECE3`。フォントは Bebas Neue（ディスプレイ）、Space Mono（本文）、Noto Sans JP（日本語）を基本に、ロゴ「坂根大悟」と作品ページの大見出しは DotGothic16（`--font-dot`）、トップのカードの作品タイトルと年号は Sawarabi Gothic（`--font-sawarabi`）。すべて Google Fonts から読み込む。ロゴには全ページ共通の CSS（`.rgb-glitch`）と末尾のスクリプトで、数秒おきに色収差のグリッチが入る。維持すべき特徴的な要素：全ページの全画面 `.scanlines` オーバーレイ、`index.html` フッターのカッサンドル「Nord Express」風の列車シルエット（ホバーで左へ走り去る）。
