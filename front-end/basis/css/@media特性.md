## CSS @media 特性

> @media 媒体查询可以基于一个或多个媒体查询的结果来限制 CSS 样式的应用。

#### 1. hover

- `none`
- `hover`

查询用户的基本输入是否能悬停在标签上。如移动设备下用户手指滑动到文字上不被认为是悬停，也不会触发 CSS :hover，虽然移动设备用户点击标签时会触发 CSS :hover，但显然不符合我们对悬停的预期，因此需要限制用户设备基本输入能够悬停在标签上(悬停在标签上能够触发 CSS :hover)时才改变:hover 样式。

```css
/* 默认悬停样式 */
a:hover {
  color: black;
  background: yellow;
}

@media (hover: hover) {
  /* 设备支持hover时悬停样式 */
  a:hover {
    color: white;
    background: black;
  }
}
```

#### 2. any-hover

- `none`
- `hover`

用法与效果与 hover 类似，区别是不只会查询用户的基本输入，而是会查用户的所有输入。

```css
@media (any-hover: hover) {
  a:hover {
    background: yellow;
  }
}
```

#### 3. pointer

- `none`
- `coarse`
- `fine`

查询用户是否有指向设备（如鼠标），以及主要指定设备的准确性如何。coarse 表示主要输入设备精度有限，fine 表示主要输入设备为精确设备。

```css
input[type="checkbox"]:checked {
  background: gray;
}

@media (any-pointer: fine) {
  input[type="checkbox"] {
    appearance: none;
    width: 15px;
    height: 15px;
    border: 1px solid blue;
  }
}

@media (any-pointer: coarse) {
  input[type="checkbox"] {
    appearance: none;
    width: 30px;
    height: 30px;
    border: 2px solid red;
  }
}
```

#### 4. any-pointer

- `none`
- `coarse`
- `fine`

查询用户是否有指向设备（如鼠标），以及这些指定设备的准确性如何。coarse 表示输入设备精度有限，fine 表示有精确的输入设备。

```css
input[type="checkbox"]:checked {
  background: gray;
}

@media (any-pointer: fine) {
  input[type="checkbox"] {
    appearance: none;
    width: 15px;
    height: 15px;
    border: 1px solid blue;
  }
}

@media (any-pointer: coarse) {
  input[type="checkbox"] {
    appearance: none;
    width: 30px;
    height: 30px;
    border: 2px solid red;
  }
}
```

#### 5. aspect-ratio

查询视口的宽高比，其是一个范围功能，支持使用`min-aspect-ratio`和`max-aspect-ratio`变体分别查询最小值和最大值。

```css
/* Minimum aspect ratio */
@media (min-aspect-ratio: 8/5) {
  div {
    background: #9af; /* blue */
  }
}

/* Maximum aspect ratio */
@media (max-aspect-ratio: 3/2) {
  div {
    background: #9ff; /* cyan */
  }
}

/* Exact aspect ratio, put it at the bottom to avoid override*/
@media (aspect-ratio: 1/1) {
  div {
    background: #f9a; /* red */
  }
}
```

#### 6. color

查询用户输出设备每个颜色分量的位数，如果设备不是彩色设备，则该值为零。支持使用`min-color`和`max-color`变体分别查询最小值和最大值。

```css
p {
  color: black;
}

/* Any color device */
/* 不为 0 则符合 */
@media (color) {
  p {
    color: red;
  }
}

/* Any color device with at least 8 bits per color component */
@media (min-color: 8) {
  p {
    color: #24ba13;
  }
}
```

#### 7. color-gamut

- `srgb`
- `p3`
- `rec2020`

查询用户输出设备支持的色域，色域范围由`srgb`、`p3`、`rec2020`逐步变广。

```css
p {
  padding: 10px;
  border: solid;
}

@media (color-gamut: srgb) {
  p {
    background: #f4ae8a;
  }
}
```

#### 8. color-index

查询输出设备的颜色查找表（LUT）中的条目数，支持使用`min-color-index`和`max-color-index`变体分别查询最小值和最大值。

```css
p {
  color: black;
}

@media (color-index) {
  p {
    color: red;
  }
}

@media (min-color-index: 15000) {
  p {
    color: #1475ef;
  }
}
```

#### 9. device-aspect-ratio `已废弃`

设备纵横比，支持使用`min-device-aspect-ratio`和`max-device-aspect-ratio`变体分别查询最小值和最大值。

```css
article {
  padding: 1rem;
}

@media screen and (min-device-aspect-ratio: 16/9) {
  article {
    padding: 1rem 5vw;
  }
}
```

#### 10. device-height `已废弃`

设备高度，支持使用`min-device-height`和`max-device-height`变体分别查询最小值和最大值。

```css
@media screen and (max-device-height: 799px) {
}
```

#### 11. device-width `已废弃`

查询设备宽度，支持使用`min-device-width`和`max-device-width`变体分别查询最小值和最大值。

```css
@media screen and (max-device-width: 799px) {
}
```

#### 12. display-mode

- `fullscreen`
- `standalone`
- `minimal-ui`
- `browser`
- `window-controls-overlay`

查询应用程序的显示模式，具体字段含义可查看 [display-mode](https://developer.mozilla.org/en-US/docs/Web/CSS/@media/display-mode)。

```css
@media all and (display-mode: fullscreen) {
  body {
    margin: 0;
    border: 5px solid black;
  }
}
```

#### 13. dynamic-range

- `standard`
- `high`

查询设备支持的亮度、对比度和色彩深度的组合。

```css
@media (dynamic-range: standard) {
  p {
    color: red;
  }
}

@media (dynamic-range: high) {
  p {
    color: green;
  }
}
```

#### 14. forced-colors

- `none`
- `active`

查询用户是否启用了强制颜色模式（eg: Windows 高对比度模式），启用后部分属性将失效，改为浏览器在绘制时强制指定。该属性的用途为，当系统设置了强制颜色模式但效果不好时，前端进行微调优化。

- 强制指定为系统设置值的有：color, background-color, text-decoration-color, text-emphasis-color, border-color, outline-color, column-rule-color, -webkit-tap-highlight-color, SVG fill attribute, SVG stroke attribute；
- 强制改变的值有：box-shadow -> none, text-shadow -> none, background-image 不基于 url 的值 -> none, color-scheme -> light dark, scrollbar-color -> auto。

```css
.button {
  border: 0;
  padding: 10px;
  box-shadow: -2px -2px 5px gray, 2px 2px 5px gray;
}

@media (forced-colors: active) {
  .button {
    /* Use a border instead, since box-shadow is forced to 'none' in forced-colors mode */
    border: 2px ButtonText solid;
  }
}
```

#### 15. grid

查询设备是否使用基于网格的屏幕（eg: 纯文本终端和仅具有一种固定字体的电话 ），大多数现代计算机和智能手机都有基于位图的屏幕。

```css
:not(.unknown) {
  color: lightgray;
}

@media (grid: 0) {
  .unknown {
    color: lightgray;
  }

  .bitmap {
    color: red;
    text-transform: uppercase;
  }
}

@media (grid: 1) {
  .unknown {
    color: lightgray;
  }

  .grid {
    color: black;
    text-transform: uppercase;
  }
}
```

#### 16. height

查询视口高度，支持使用`min-height`和`max-height`变体分别查询最小值和最大值。

```css
/* Exact height */
@media (height: 360px) {
  div {
    color: red;
  }
}

/* Minimum height */
@media (min-height: 25rem) {
  div {
    background: yellow;
  }
}

/* Maximum height */
@media (max-height: 40rem) {
  div {
    border: 2px solid blue;
  }
}
```

#### 17. width

查询视口宽度，支持使用`min-width`和`max-width`变体分别查询最小值和最大值。

```css
/* 精确宽度 */
@media (width: 360px) {
  div {
    color: red;
  }
}

/* 最小宽度 */
@media (min-width: 35rem) {
  div {
    background: yellow;
  }
}

/* 最大宽度 */
@media (max-width: 50rem) {
  div {
    border: 2px solid blue;
  }
}
```

#### 18. inverted-colors

查询是否支持反转颜色（目前 Safari 支持）。

- `none`
- `inverted`

```css
p {
  color: gray;
}

@media (inverted-colors: inverted) {
  p {
    background: black;
    color: yellow;
    text-decoration: overline;
  }
}

@media (inverted-colors: none) {
  p {
    background: #eee;
    color: red;
  }
}
```

#### 19. monochrome

查询输出设备单色帧缓冲区每个像素的位数，支持使用`min-monochrome`和`max-monochrome`变体分别查询最小值和最大值。

```css
p {
  display: none;
}

/* Any monochrome device */
@media (monochrome) {
  p.mono {
    display: block;
    color: #333;
  }
}

/* Any non-monochrome device */
@media (monochrome: 0) {
  p.no-mono {
    display: block;
    color: #ee3636;
  }
}
```

#### 20. orientation

- `portrait`
- `landscape`

查询屏幕方向，区分当前是横屏或竖屏，`portrait`:竖屏, `landscape`:横屏。

```css
body {
  display: flex;
}

div {
  background: yellow;
}

@media (orientation: landscape) {
  body {
    flex-direction: row;
  }
}

@media (orientation: portrait) {
  body {
    flex-direction: column;
  }
}
```

#### 21. overflow-block

- `none`
- `scroll`
- `optional-paged`
- `paged`

查询输出设备如何处理块状内容溢出视口的内容。

```css
@media (overflow-block: scroll) {
  p {
    color: red;
  }
}
```

#### 22. overflow-inline

查询输出设备如何处理行内内容溢出视口的内容。

- `none`
- `scroll`

```css
p {
  white-space: nowrap;
}

@media (overflow-inline: scroll) {
  p {
    color: red;
  }
}
```

#### 23. prefers-color-scheme

- `no-preference`
- `light`
- `dark`

查询用户是否将系统主题设置为亮色或深色。

```css
.day {
  background: #eee;
  color: black;
}
.night {
  background: #333;
  color: white;
}

@media (prefers-color-scheme: dark) {
  .day.dark-scheme {
    background: #333;
    color: white;
  }
  .night.dark-scheme {
    background: black;
    color: #ddd;
  }
}

@media (prefers-color-scheme: light) {
  .day.light-scheme {
    background: white;
    color: #555;
  }
  .night.light-scheme {
    background: #eee;
    color: black;
  }
}

.day,
.night {
  display: inline-block;
  padding: 1em;
  width: 7em;
  height: 2em;
  vertical-align: middle;
}
```

#### 24. prefers-contrast

- `no-preference`
- `more`
- `less`
- `custom`

查询用户是否设置了系统显示模式为低对比度或高对比度，`custom`指的是除`more`与`less`之外的自定义调色板。

```css
.contrast {
  width: 100px;
  height: 100px;
  outline: 2px dashed black;
}

@media (prefers-contrast: more) {
  .contrast {
    outline: 2px solid black;
  }
}
```

#### 25. prefers-reduced-motion

查询用户是否开启了动画减弱功能。

- `no-preference`
- `reduce`

```css
.animation {
  animation: vibrate 0.3s linear infinite both;
}

@media (prefers-reduced-motion: reduce) {
  .animation {
    animation: none;
  }
}
```

#### 26. resolution

查询输出设备的分辨率，支持使用`min-resolution`和`max-resolution`变体分别查询最小值和最大值。

```css
/* Exact resolution */
@media (resolution: 150dpi) {
  p {
    color: red;
  }
}

/* Minimum resolution */
@media (min-resolution: 72dpi) {
  p {
    text-decoration: underline;
  }
}

/* Maximum resolution */
@media (max-resolution: 300dpi) {
  p {
    background: yellow;
  }
}
```

#### 27. scripting

- `none`
- `initial-only`
- `enabled`

查询脚本（eg: JavaScript）是否可执行，`initial-only`表示仅在初始化时可执行。

```css
p {
  color: lightgray;
}

@media (scripting: none) {
  .script-none {
    color: red;
  }
}

@media (scripting: initial-only) {
  .script-initial-only {
    color: red;
  }
}

@media (scripting: enabled) {
  .script-enabled {
    color: red;
  }
}
```

#### 28. update

- `none`
- `slow`
- `fast`

查询设备是否可以在呈现内容后根据 CSS 变化更新视图，`none`:渲染完成后布局无法再更新、`slow`:输出设备无法足够快地渲染或显示更改（如电子书阅读器）、`fast`:输出设备可以正常变化且速度不会受限制（如电脑屏幕）。

```css
@keyframes jiggle {
  from {
    transform: translateY(0);
  }

  to {
    transform: translateY(25px);
  }
}

@media (update: fast) {
  p {
    animation: 1s jiggle linear alternate infinite;
  }
}
```

#### 29. video-dynamic-range

- `standard`
- `high`

查询设备峰值亮度、对比度以及色彩深度的支持情况（峰值亮度是指发光设备<例如液晶显示屏>的最亮点可以产生的亮度。对于光反射设备<例如纸张或电子墨水>，峰值亮度是指至少吸收光的点）。
