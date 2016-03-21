# VericalRhythm.scss
「Vertical Rhythm」は要素を配置するための手法のひとつです。行の高さ（baseline）を基準に要素を配置していくことで、パターンを作り、規則性や一貫性を生み出します。

VericalRhythm.scssでは`html`タグにbaselineを指定することで、Sassの複雑な変数やmixinを使用せずに、Vertical Rhythmを作ることができます。

## セットアップ
_setting.scssにはVertical Rhythmを作るためのベースになるスタイルが定義されています。

`$base-unit`は通常の`1rem`に相当します。`$base-unit`に`html`の`font-size`を乗じた数値がベースのフォントサイズになります。

```scss
// ページの基準になるフォントサイズ
// 15px / 24px = 0.625rem
// 24px / 15px = 1.6（line-height）
$base-unit: 0.625rem !default;

/* ページの基準になるbaseline（行の高さ） */
// 0.625rem * 24px = 15px
// 0.625rem * 26px = 16.25px
html {
  font-size: 24px;
  @media (min-width: 1000px) {
   font-size: 26px;
  }
}

/**
 * `body`以降の要素のbaselineを
 * `html`に指定した`font-size`に定義する
 */
body {
  font-size: $base-unit;
  line-height: 1rem;
}
```

`body`タグ以降で`rem`を指定すると`html`タグで指定した`font-size`が基準になります。

`body`タグでフォントサイズとbaselineをリセットしているので、フォントサイズを変更しない限りは`$base-unit`で定義したフォントサイズが継承されます。

## フォントサイズの指定
_base.scssでは`h1`や`p`のようなタイプセレクタのデフォルトスタイルを指定しています。

`h1`や`h2`などでフォントサイズを変更する場合は`rem`で指定します。`line-height`を変更すると、baselineも変更することができます。

```scss
h1 {
  margin: 2rem 0;
  font-size: 1rem;
  line-height: 2rem;
}

h2 {
  margin: 2rem 0 1rem;
  font-size: 0.75rem;
}
```

## 余白の指定
`px`や`em`は使用せず、`rem`だけを使用します。

`margin`を指定する場合は`1rem`を最小単位とします。

```scss
h1, h2, h3, h4, h5, h6,
p,
ul, ol, dl,
figure, figcaption, blockquote, pre, table, caption, hr,
form, fieldset {
  margin: 0 0 1rem;
}

h2 {
  margin: 2rem 0 1rem;
  font-size: 0.75rem;
}
```

`padding`を指定する場合は、上下の余白の合計が`1rem`になるようにします。

```scss
.foo {
  padding: 0.5rem;
}
```

## ボーダーの指定
デフォルトでは`box-sizing:border-box;`を全体に指定しています。

ボーダーの指定はピクセルで指定するのが一般的ですが、ボーダーの高さの分だけ余白がズレてしまいます。

`calc()`を使用してボーダーの高さを除いた余白を指定します。`calc()`は[IE9以降とAndroid4.4以降が対応](http://caniuse.com/#search=calc)しています。

```scss
// padding - border-width
.foo {
  border: 1px solid;
  padding: calc(0.5rem - 1px);
}

// margin - border-width
.bar {
  border-bottom: 1px solid;
  margin-bottom: calc(1rem - 1px);
}
```

## デバッグ
_debug.scssにはbaselineにズレがないかを確認するためのスタイルが指定されています。

```scss
// デバッグ
// <div class="verticalRhythm">
//   <div class="baseline">&nbsp;</div>
//   <div class="baseline">&nbsp;</div>
//   <div class="baseline">&nbsp;</div>
// </div>
.verticalRhythm {
  position: absolute;
  z-index: -1;
  top: 0;
  right: 0;
  bottom: 0;
  left: 0;
  .baseline {
    margin: 0;
    font-size: 1rem;
    line-height: 1rem;
    box-shadow: 0 1px 0 #eee;
  }
}

h1, h2, h3, h4, h5, h6,
p,
ul, ol, dl,
figure, figcaption, blockquote, pre, table, caption, hr,
form, fieldset {
  box-shadow: 0 1px 0 #faa, inset 0 1px 0 #faa;
}
```

デバッグを使用する場合は_debug.scssをインポートした状態で、以下のようにマークアップします。ページの高さに応じて`.baseline`の数を増やしてください。

```html
<div class="verticalRhythm">
  <div class="baseline">&nbsp;</div>
  <div class="baseline">&nbsp;</div>
  <div class="baseline">&nbsp;</div>
</div>
```
