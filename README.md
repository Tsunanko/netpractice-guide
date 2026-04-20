# NetPractice Guide

> 42 Tokyo の NetPractice プロジェクトを **超初心者向け** に解説するガイドサイト。

🔗 **公開ページ**: https://tsunanko.github.io/netpractice-guide/
📦 **提出用リポジトリ**: https://github.com/Tsunanko/netpractice

---

## このサイトは何？

NetPractice（ネットワーク課題）を解くのに必要な **IP アドレス・サブネットマスク・ルーティング・双方向到達性** を、
**家の住所や郵便局のメタファー** で分かりやすく説明するガイドです。

全 4 部構成:

1. **第1部 基礎** — IP・マスク・CIDR・ルータ・ゲートウェイ・ルーティング・双方向性
2. **第2部 全 10 レベル攻略** — 各レベルの考え方と解答例
3. **第3部 抽象化された学び** — 他分野に応用できる思考パターン
4. **第4部 評価対策** — ディフェンス Q&A + 当日チートシート

---

## 特徴

- 🎨 **Mermaid 図が豊富** — IP アドレスの分割やルーティング経路を視覚化
- 🏠 **メタファー駆動** — 「家の住所」「郵便局」など身近な例え
- 📸 **実機スクリーンショット** — 各レベルの画面付き
- 💡 **「なぜ？」を丁寧に** — 単なる答えではなく考え方を解説
- 🎓 **評価対策まで** — ディフェンス Q&A、20 問の模範回答

---

## ローカルで開発する

### 必要なもの

- Python 3.9+
- `pip`

### 起動

```bash
pip install -r requirements.txt
mkdocs serve
```

http://127.0.0.1:8000 で開きます。

### ビルド

```bash
mkdocs build --strict
# ./site/ に HTML が生成される
```

---

## ファイル構成

```
netpractice-guide/
├── docs/
│   ├── index.md                    # ホーム
│   ├── 00-start-here.md            # はじめに
│   ├── 01-basics/                  # 第1部: 基礎
│   ├── 02-levels/                  # 第2部: 各レベル
│   ├── 03-learnings/               # 第3部: 抽象化
│   ├── 04-defense/                 # 第4部: 評価対策
│   ├── glossary.md                 # 用語集
│   ├── images/screenshots/         # 各レベルのスクショ
│   ├── stylesheets/extra.css
│   └── js/video-control.js
├── mkdocs.yml
├── requirements.txt
└── .github/workflows/deploy.yml    # GitHub Pages 自動デプロイ
```

---

## デプロイ

GitHub に push すると `.github/workflows/deploy.yml` が動いて
`gh-pages` ブランチに自動デプロイされます。

初回設定:
1. GitHub リポジトリの **Settings → Pages**
2. Source を **Deploy from a branch** に
3. Branch: `gh-pages` / (root) を選んで Save

---

## 関連プロジェクト

- [cub3d-guide](https://github.com/Tsunanko/cub3d-guide) — cub3D プロジェクトの解説サイト（姉妹プロジェクト）
- [netpractice](https://github.com/Tsunanko/netpractice) — 本課題の提出用リポジトリ

---

## ライセンス

学習用の個人プロジェクト。内容の再利用は自由ですが、画像・文章の無断複製・転載は避けてください。
