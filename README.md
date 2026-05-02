# Daigo Sakane - Portfolio

映像作家・坂根大悟のポートフォリオサイト。

- **公開URL**: https://daigosakane.com
- **ホスティング**: Cloudflare Pages
- **リポジトリ**: redstar551/portfolio

---

## サイト構成

```
portfolio/
├── index.html              # トップページ（Works/About/Contact）
├── work-template.html      # 作品個別ページの雛形
├── jinko-haikyo-m2.html    # 作品ページ：人工廃墟 M2
├── e-waste-invaders.html   # 作品ページ：E-Waste Invaders
├── favicon.png             # ファビコン
├── profile.jpg             # Aboutセクションのプロフィール写真
└── README.md               # このファイル
```

---

## デザイン方針

- **コンセプト**: ロトチェンコ × カッサンドル風の構成主義デザインに、スクロール連動グリッチを重ねる
- **配色**:
  - ネイビー `#062C54`
  - アクセントブルー `#4A9EFF`
  - 黒 `#0A0A0A`
  - クリーム `#F0ECE3`
- **フォント**:
  - 英字ディスプレイ: Bebas Neue
  - 英字本文: Space Mono
  - 日本語: Noto Sans JP

### 主要演出

- スクロール時にRGBズレ・ストライプ・テキストグリッチが発火
- アンビエントグリッチ（数秒おきにランダムで発火）
- フッターに列車シルエット（カッサンドルNord Express風）。ホバーで左に走り去る
- 全画面スキャンライン常時オーバーレイ

---

## 更新の流れ

### 文章を直すだけ

1. GitHub上で `index.html` を開く
2. 右上の鉛筆アイコンで編集
3. 該当テキストを書き換えて「Commit changes」
4. 数秒で `daigosakane.com` に反映される

### 作品を追加する

1. `work-template.html` をコピーして作品名でリネーム（例：`new-work.html`）
2. 中身を書き換え（タイトル、説明、動画URL、詳細）
3. GitHubに新規ファイルとしてアップロード
4. `index.html` のWorks内に新しいカードを追加し、`<a href="new-work.html" class="work-card">` でリンク

### 画像・動画を追加する

- 画像：`images/` フォルダにアップロードして `<img src="images/ファイル名.jpg">` で参照
- 動画：YouTubeかVimeoのURLを `<iframe src="...">` に貼る

---

## カード仕様

トップページの作品カードは年号＋タイトル＋ジャンルのみ。詳細は個別ページに記載する。

ジャンルの分類：
- Game Installation
- Installation
- CG
- Short Film
- Game
- Event

---

## 連絡先

- Email: diego.sakane@gmail.com
- Instagram: [@daigorere](https://www.instagram.com/daigorere/)
- X / Twitter: [@perfect_blue5](https://x.com/perfect_blue5)

---

© 2026 Daigo Sakane
