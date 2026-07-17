# わくわくパステル デザイン仕様書

燕岳登山プランで使用している「かわいい・ワクワク」テーマのデザインシステム。
他のページに引き継ぐ際は、この仕様書とCSSテンプレートをそのまま流用してください。

---

## 1. コンセプト

- **キーワード**：かわいい／ワクワク／やわらかい／親しみやすい
- **印象**：パステルカラー＋丸みの強い角＋ふんわりした影＋絵文字アクセント
- **読みやすさ最優先**：薄いピンクは「背景・枠・装飾」だけに使い、**文字には必ず濃い色**を使う

---

## 2. カラーパレット

CSS変数（`:root`）で管理します。**「文字用」と「背景・装飾用」を必ず使い分ける**のが最大のポイントです。

```css
:root {
  /* ベース */
  --col-bg:            #fef6f0;  /* ページ背景（クリーム） */
  --col-surface:       #ffffff;  /* カード背景 */
  --col-border:        #f4dfd0;  /* 枠線 */

  /* テキスト（濃い＝読みやすい） */
  --col-text:          #3a2e28;  /* 本文（濃茶） */
  --col-muted:         #6b5349;  /* 補足・ラベル */
  --col-faint:         #8a7266;  /* URLなど最も控えめ */

  /* メインアクセント：ピンク */
  --col-accent:        #ff8fab;  /* ★背景・枠・装飾専用★ */
  --col-accent-text:   #c2185b;  /* ★文字用の濃いローズ★ */
  --col-accent-light:  #ffe3ec;  /* 淡い背景 */

  /* サブカラー（文字＝濃い / light＝背景） */
  --col-blue:    #2f6da8;  --col-blue-light:   #e3f0fb;  /* 1日目・バス */
  --col-amber:   #c56a00;  --col-amber-light:  #fff2dd;  /* 温泉・注意 */
  --col-red:     #c62828;  --col-red-light:    #ffe8e8;  /* 警告 */
  --col-teal:    #1f8a80;  --col-teal-light:   #e0f7f5;  /* YAMAP・ランチ */
  --col-purple:  #7e57c2;  --col-purple-light: #f0e9fc;  /* 予備 */

  /* グラデーション */
  --grad-hero: linear-gradient(135deg, #f0567d 0%, #d96a9c 45%, #5c8fd6 100%);
  --grad-sun:  linear-gradient(135deg, #ffd86b, #ff9a5a);
}
```

### コントラストのルール
| 用途 | 使う変数 | ダメな例 |
|------|----------|----------|
| 文字色 | `--col-accent-text`（濃いローズ） | ✗ `--col-accent`（薄ピンク）を文字に使う |
| 背景・淡い塗り | `--col-accent-light` | |
| 枠線・アイコン | `--col-accent` | |

---

## 3. タイポグラフィ

```css
@import url('https://fonts.googleapis.com/css2?family=Kosugi+Maru&family=M+PLUS+Rounded+1c:wght@400;500;700;800&display=swap');

body {
  font-family: 'M PLUS Rounded 1c', 'Kosugi Maru', sans-serif;
  font-size: 14px;
  line-height: 1.75;
}
```

- 見出し・強調：`font-weight: 800`
- 本文の項目名・数値：`font-weight: 700`
- 小さい注釈でも **12px 以上**（10〜11pxは避ける）

---

## 4. 共通スタイルの原則

| 要素 | 値 |
|------|-----|
| カードの角丸 | `border-radius: 20px`（小要素は 14〜18px） |
| 枠線 | `2px solid var(--col-border)` |
| 影 | `box-shadow: 0 6px 18px rgba(255,143,171,.10)` |
| ホバー | `transform: translateY(-3px)` でふわっと浮かせる |
| セクション見出し | 先頭に🌸、末尾に点線の飾り罫 |
| カラー背景 | 単色よりも `linear-gradient` を優先 |
| 絵文字 | 見出し・バナー・カードヘッダに1つずつ添える |

---

## 5. 主要コンポーネント（コピペ用CSS）

### 5-1. ヒーロー（ページ最上部）
虹色グラデ＋浮遊ドット＋ゆらゆら絵文字。

```css
.hero {
  background: var(--grad-hero);
  color: #fff;
  padding: 54px 24px 46px;
  text-align: center;
  position: relative;
  overflow: hidden;
  border-bottom-left-radius: 40% 24px;
  border-bottom-right-radius: 40% 24px;
}
.hero::before { /* 浮遊ドット模様 */
  content: '';
  position: absolute; inset: 0;
  background-image:
    radial-gradient(circle, rgba(255,255,255,.55) 2px, transparent 2.5px),
    radial-gradient(circle, rgba(255,255,255,.35) 2px, transparent 2.5px);
  background-size: 44px 44px, 44px 44px;
  background-position: 0 0, 22px 22px;
  opacity: .6;
  animation: floatdots 12s linear infinite;
}
@keyframes floatdots {
  from { background-position: 0 0, 22px 22px; }
  to   { background-position: 44px 44px, 66px 66px; }
}
.hero-emoji { font-size: 44px; margin-bottom: 8px; animation: bob 3s ease-in-out infinite; }
@keyframes bob {
  0%,100% { transform: translateY(0) rotate(-2deg); }
  50%     { transform: translateY(-8px) rotate(2deg); }
}
```

```html
<div class="hero">
  <div class="hero-emoji">⛰️✨</div>
  <div class="hero-eyebrow">わくわくプラン 2026</div>
  <div class="hero-title">タイトル</div>
  <div class="hero-sub">サブタイトル</div>
</div>
```

### 5-2. セクション見出し
```css
.section-label {
  font-size: 15px; font-weight: 800;
  color: var(--col-accent-text);
  display: flex; align-items: center; gap: 8px;
}
.section-label::before { content: '🌸'; font-size: 16px; }
.section-label::after {
  content: ''; flex: 1; height: 3px; border-radius: 3px;
  background: repeating-linear-gradient(90deg, var(--col-accent-light) 0 8px, transparent 8px 14px);
}
```

### 5-3. カード
```css
.card {
  background: var(--col-surface);
  border: 2px solid var(--col-border);
  border-radius: 20px;
  overflow: hidden;
  box-shadow: 0 6px 18px rgba(255,143,171,.10);
}
```

### 5-4. バッジ
```css
.badge {
  display: inline-block; font-size: 10px; font-weight: 800;
  padding: 3px 10px; border-radius: 20px;
  box-shadow: 0 2px 5px rgba(0,0,0,.06);
}
/* 背景はlight・文字は濃い色のペアで */
.badge-hike { background: var(--col-accent-light); color: var(--col-accent-text); }
.badge-bus  { background: var(--col-blue-light);   color: var(--col-blue); }
```

### 5-5. 強調バナー（YAMAP等）
```css
.banner {
  display: flex; align-items: center; justify-content: space-between;
  gap: 12px; flex-wrap: wrap;
  background: linear-gradient(120deg, #4ecdc4, #44b3aa);
  color: #fff;
  border-radius: 22px; padding: 18px 22px;
  box-shadow: 0 8px 20px rgba(78,205,196,.35);
  text-decoration: none;
  transition: transform .15s, box-shadow .15s;
  position: relative; overflow: hidden;
}
.banner::after { /* 透かし絵文字 */
  content: '⛰️'; position: absolute; right: 14px; bottom: -14px;
  font-size: 70px; opacity: .18;
}
.banner:hover { transform: translateY(-3px); }
```

### 5-6. リンクカード（ホバーでぷるっと動く）
```css
.link-card {
  background: var(--col-surface);
  border: 2px solid var(--col-border);
  border-radius: 16px; padding: 14px 16px;
  box-shadow: 0 4px 10px rgba(255,143,171,.08);
  transition: transform .15s, border-color .15s, background .15s, box-shadow .15s;
}
.link-card:hover {
  border-color: var(--col-accent);
  background: var(--col-accent-light);
  transform: translateY(-3px) rotate(-.5deg);
}
```

### 5-7. フッター
```css
footer {
  text-align: center; padding: 28px 24px 32px;
  font-size: 11px; color: var(--col-muted);
  background: linear-gradient(0deg, var(--col-accent-light), transparent);
}
footer::before {
  content: '🌸 ⛰️ 🎒 ☀️ 🌸';
  display: block; font-size: 18px; letter-spacing: 6px; margin-bottom: 10px;
}
```

---

## 6. 新しいページに引き継ぐ手順

1. `<head>` にフォントの `@import` を追加
2. `:root` のカラー変数をまるごとコピー
3. 使いたいコンポーネントのCSSをコピー（クラス名も同じでOK）
4. 絵文字は内容に合わせて差し替え（例：⛰️→🏖️ など）
5. **文字色は必ず `-text` 系の濃い変数**を使う（コントラスト確認）

---

## 7. 応用のヒント

- **テーマ替え**：`--col-accent` 系だけ差し替えれば全体の色調が変わる
  （例：青系にしたいなら accent=#5b9bd5 / accent-text=#2f6da8 / accent-light=#e3f0fb）
- **絵文字**は使いすぎない。見出し・バナー・カードヘッダに1つずつが目安
- **animation**（bob / floatdots）は多用すると重い・うるさいので主役だけに

---

_元ページ： https://halab18.github.io/tsubakuro-plan/ （ソースの `index.html` を参照）_
