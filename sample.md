---
title: "MarkdownからBeamer"
subtitle: "Pandocで変換"
author: "A大学　B太郎"
date: 2021/02/08
header-includes: 
- \usepackage{bookmark}
- \usepackage{zxjatype}
- \usepackage[ipa]{zxjafont}  
- \usetheme{metropolis}
---

\titlepage

## yamlヘッダの説明

- yamlヘッダのメタ情報
  - title: プレゼンテーションのタイトル
  - subtitle: サブタイトル
  - author: 発表者
  - date: 日付

## TeXのプリアンブル部分の説明

1. \\usepackage{bookmark}         : PDFにしおりを付ける
1. \\usepackage{zxjatype}            : 日本語を使うために必要
1. \\usepackage[ipa]{zxjafont}     : フォントの指定(ここではIPAフォントを指定している)
1. \\usetheme{metropolis}           : beamerのテーマ，metropolisおすすめ(TeX Liveに最初から入ってる)

---

## そのほか

- インライン数式: $\log (1+x)=x$
- 別行立て数式
$$f(x)=\dfrac{1}{\sqrt{2\pi\sigma}}\exp(-\dfrac{(x-\mu)^ 2}{2\sigma^ 2})$$
- 表

||a|b|
|-|-|-|
|c|0|1|
|d|1|0|

---

- 図

![図のキャプション](keyboard_typing.png){width=8cm}
