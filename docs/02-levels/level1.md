# Level 1 — 直結リンク

!!! warning "⚠️ 数値は毎回ランダムに変わります"
    このページに書かれた IP・マスク・ルートの値は **前回プレイした時の一例** です。
    あなたの画面では違う数値になっているはずなので、**そのままコピペしても絶対に解けません**。
    真似するのは「**どう考えて解くか**」の手順だけ。数値は自分の画面から読み取って計算してください。

> 🎯 **一言で言うと:** ケーブルで直結された 2 台のホストを **同じサブネット** に揃えるだけ。

## 📖 このページは何？

NetPractice の **最初のレベル**。
ホスト 2 台が直接ケーブルで繋がっている状況で、両方のホストを **同じ町（サブネット）の住人** にしてあげれば通信できます。

このレベルで身につくこと：

1. NetPractice の画面の操作に慣れる
2. **直結 = 同じサブネットでなければならない** という大原則
3. 固定された IP/マスクから「相手の町」を逆算する習慣

---

## 📷 問題画面

[![Level 1 のスクリーンショット](../images/screenshots/level1.png)](../images/screenshots/level1.png)

---

## 🗺️ トポロジー（構造）

```mermaid
flowchart LR
    subgraph pair1 [組 1]
      A[💻 Host A<br>A1]
      B[💻 Host B<br>B1]
      A --- B
    end
    subgraph pair2 [組 2]
      C[💻 Host C<br>C1]
      D[💻 Host D<br>D1]
      C --- D
    end

    classDef leftHost fill:#E3F2FD,stroke:#1976D2,color:#000,stroke-width:2px
    classDef rightHost fill:#FFF9C4,stroke:#F9A825,color:#000,stroke-width:2px

    class A,C leftHost
    class B,D rightHost
```

→ **2 組のホストがそれぞれ直結**。ルータもスイッチもない、最もシンプルな構成。

---

## 🧩 ゴール

- ✅ A ↔ B が通信できる
- ✅ C ↔ D が通信できる

---

## 📺 画面の編集できる箇所

| 場所 | 何？ | 状態 | あなたが直すか？ |
|---|---|---|---|
| A1 IP | ホスト A のアドレス | 白 (編集可) | ✅ 直す |
| A1 Mask | ホスト A のマスク | 薄ピンク (固定) | ❌ そのまま |
| B1 IP/Mask | ホスト B のアドレス | 薄ピンク (固定) | ❌ そのまま |
| C1 IP/Mask | ホスト C のアドレス | 薄ピンク (固定) | ❌ そのまま |
| D1 IP | ホスト D のアドレス | 白 (編集可) | ✅ 直す |
| D1 Mask | ホスト D のマスク | 薄ピンク (固定) | ❌ そのまま |

→ つまりこのレベルでやることは「**A1 IP と D1 IP の 2 つを書き直す**」だけ。

---

## 🔒 固定値（あるサンプルアカウントの例）

| IF | IP | マスク | 編集可能 |
|:---|:---|:---|:-:|
| A1 | `104.93.23.313` ← **不正な数字** | `255.255.255.0` (/24) | IP のみ |
| B1 | `104.94.23.12` | `255.255.255.0` (/24) | なし |
| C1 | `211.191.62.75` | `255.255.0.0` (/16) | なし |
| D1 | `211.190.364.42` ← **不正な数字** | `255.255.0.0` (/16) | IP のみ |

!!! info "あなたの画面の数字は違うかもしれません"
    NetPractice は intra login ごとにランダムな IP を生成します。
    数値そのものではなく、**考え方** を合わせて読み替えてください。

---

## 🧠 考え方

### Step 1: 固定側の「町（サブネット）」を調べる

**B1 = `104.94.23.12/24`** が固定値。ここから「B が住んでいる町」を逆算します。

#### 🔢 計算手順

<div class="step-flow">
  <div class="step"><span class="step-num">1</span>B1 の IP<br><code>104.94.23.12</code></div>
  <div class="step"><span class="step-num">2</span>マスク<br><code>/24</code></div>
  <div class="step"><span class="step-num">3</span>第4オクテットを<br>0 にする</div>
  <div class="step"><span class="step-num">4</span>町の住所<br><code>104.94.23.0/24</code></div>
</div>

#### 🏘️ 町の中身を見える化

```mermaid
flowchart LR
    subgraph town ["🏘️ 104.94.23.0/24 — B1 が住む町（住人 254 人）"]
      direction LR
      Net[".0<br>📛 入口看板<br>住人不可"]
      H1[".1〜.11<br>👤 空き番地"]
      B[".12<br>🏠 B1（固定）"]
      H2[".13〜.254<br>👤 空き番地"]
      BC[".255<br>📣 一斉放送<br>住人不可"]
    end

    classDef unusable fill:#FFCDD2,stroke:#C62828,color:#000,stroke-width:2px
    classDef fixed fill:#FFF59D,stroke:#F9A825,color:#000,stroke-width:3px
    classDef available fill:#E8F5E9,stroke:#66BB6A,color:#000,stroke-width:2px

    class Net,BC unusable
    class B fixed
    class H1,H2 available
```

- `.0` は町の **入口看板**（ネットワークアドレス）→ 誰も住めない
- `.255` は **一斉放送用**（ブロードキャスト）→ 誰も住めない
- 残りの `.1〜.254` から、**`.12` 以外** を A1 に割り当てればいい

### Step 2: A1 を B1 と同じ町に入れる

A1 の IP を `104.94.23.x`（`x` は 1〜254、12 以外）に変更。

例: **`104.94.23.13`**

```mermaid
flowchart LR
    A["🏠 A1<br>104.94.23.13<br>（新規）"]
    B["🏠 B1<br>104.94.23.12<br>（固定）"]
    A <-->|"✅ 直接通信OK<br>同じ町"| B

    classDef hostNew fill:#A5D6A7,stroke:#2E7D32,color:#000,stroke-width:3px
    classDef hostFixed fill:#FFF59D,stroke:#F9A825,color:#000,stroke-width:3px

    class A hostNew
    class B hostFixed
```

→ A1 と B1 が同じ町（`104.94.23.0/24`）の住人になったので、ケーブル直結で通信できる。

### Step 3: 同じことを C ↔ D にも

**C1 = `211.191.62.75/16`** から町を計算（手順は Step 1 と同じ）：

<div class="step-flow">
  <div class="step"><span class="step-num">1</span>C1 の IP<br><code>211.191.62.75</code></div>
  <div class="step"><span class="step-num">2</span>マスク<br><code>/16</code></div>
  <div class="step"><span class="step-num">3</span>第3〜4オクテットを<br>0 にする</div>
  <div class="step"><span class="step-num">4</span>町の住所<br><code>211.191.0.0/16</code></div>
</div>

`/24` の町は 254 人だったが、**`/16` の町は 65,534 人** の大都市。
住人範囲は `211.191.0.1 〜 211.191.255.254` で、**第3・第4オクテットの両方を自由に使える**。

D1 を `211.191.x.y`（C1 と被らない値）に。例: **`211.191.62.76`**

```mermaid
flowchart LR
    C["🏠 C1<br>211.191.62.75<br>（固定）"]
    D["🏠 D1<br>211.191.62.76<br>（新規）"]
    C <-->|"✅ 直接通信OK<br>同じ町 /16"| D

    classDef hostFixed fill:#FFF59D,stroke:#F9A825,color:#000,stroke-width:3px
    classDef hostNew fill:#A5D6A7,stroke:#2E7D32,color:#000,stroke-width:3px

    class C hostFixed
    class D hostNew
```

---

## 🎬 パケットの旅（郵便で例えると）

A から B へ手紙を出す（A↔B のゴール）：

```
1. A が手紙を書く
   宛先: 104.94.23.12 (B)
   差出人: 104.94.23.13 (A)

2. A が「B は同じ町の住人？」を確認
   A の町 = 104.94.23.0/24 (.1〜.254)
   B の住所 .12 はこの範囲？ → ✅ YES

3. A が B に手紙を直接渡す（ケーブル経由）
   ✅ 配達完了
```

→ **同じ町の住人なら、郵便屋さん（ルータ）を経由せず直接渡せる。**

---

## ✅ 解答例

```
A1 IP → 104.94.23.13
D1 IP → 211.191.62.76
```

---

## 🔗 関連概念

- 📺 [このガイドの使い方](../00-start-here.md) — 画面要素の説明含む
- 02 [サブネットマスクって何？](../01-basics/subnet-mask.md) — マスクの基礎
- 03 [CIDR 早見表](../01-basics/cidr.md) — `/24` の意味

---

## 🎓 このレベルの抽象的な学び

!!! tip "転用できる考え方 — 制約から逆算"
    **「固定された制約から逆算」** という思考パターン。
    プログラミングでも「引数の型が決まっている → 関数の中で何ができるかが決まる」のと同じ。
    NetPractice は **そのミニチュア版** で、制約に従って残りの値を埋めていく訓練です。

---

## ⚠️ よくあるミス

!!! warning "マスクが違うのに同じ町だと思う"
    B1 が /24 なのに A1 を「`100.94.23.0/16`」と勘違いすると落ちる。
    **両側のマスクを確認** してから町を計算する。

!!! warning ".0 や .255 を使う"
    `104.94.23.0` や `104.94.23.255` は使えない。
    自動で弾かれるので、**1〜254** の範囲で選ぶ。

!!! warning "不正な数字（256以上）に気づかない"
    `104.93.23.313` のように 256 以上の数字は **そもそも IP として無効**。
    これは「直してね」というヒント。

---

## ▶️ 次に読むページ

[Level 2 — 不正マスクの修正](level2.md)
