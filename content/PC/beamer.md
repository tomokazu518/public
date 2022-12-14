---
title: "BeamerスライドをMarkdownで簡単に作成"
weight: 30
---

{{< toc >}}

## はじめに

Beamerのスライドはかっこいいので，それを見てLatexを使いたいと思った人も多いはず。しかし，Latexを覚えるのは大変だし，いろいろコマンドが入り混じったソースコードは可読性が低い。

そこで，Markdownという非常にシンプルな記法で内容を記述し，Pandocというソフトを使ってBeamerに変換する方法を紹介する。

Markdownはテキスト・データなので，編集はPowerPointよりも軽快で内容に集中できるし，Gitでバージョン管理することもできる。ただし，Latex経由でPDFを作成するため，Latexのコンパイル環境は必要。また，PDFに変換するまでスライドの仕上がりを確認できない点や，PowerPointのようにコンテンツを柔軟にレイアウトすることが難しい点もデメリットかもしれない。

## Markdownとは

Markdownは，文書を書くための記法の一つ。テキスト・エディタで作成する。文書の構造(章・節)や，箇条書き，文字列の強調などをシンプルな記法で表現することができ，Pandocというツールを用いればhtmlやword形式，Latexなどに変換することができる(いくつかのフォーマットは逆にMarkdownへと変換することも可能)。

Markdownの書き方は非常にシンプル。まずは，以下だけ覚えれば十分。

- #がレベル1の見出し，##がレベル2の見出しといったように行頭に＃をつけるとその行が節や章の見出しとなる
- 段落の区切りは，間に一行空行を入れることで表現
- 箇条書きは，項目の頭に"- "をつける
- 番号付き箇条書きは，項目の頭に”1. "をつける(番号はすべて1.にしておけば自動で振ってくれる)
- 数式は"$"で囲ってTeX形式で書く(インラインの場合は$1個，別行立ての場合は$2個)
- 強調する文字列は"**"で囲む，イタリックにしたい文字は"*"で囲む

### Markdownの書き方例

```markdown

---
title: Markdownの書き方例 
---

# はじめに(レベル1の見出し)

## はじめの一歩(レベル2の見出し)

- 箇条書き1
- 箇条書き2

## 次の一歩(レベル2の見出し)

1. 番号付き箇条書きは，1.をつける
1. すべて1.にしておく
1. 自動で番号を振ってくれる

# 文章を書く(レベル1の見出し)

内容はテキスト・エディタなどで作成する。メモ帳でも十分だが，Markdown専用のエディタもいろいろあるので，使いやすいものを選ぼう。

段落を区切るときには，改行してさらに一行空行を挿入する。段落のはじめに一文字空ける必要はない(wordやPDFに変換するときに自動でフォーマットされる)。

文字列の**強調**，*斜体*は，”*"で囲むことで表現できる。

$$\log (1+x) \approx x$$

インライン数式は，$\log (1+x) \approx x$のように"$"で囲む。

```

htmlに変換するコマンドは以下。

```bash
pandoc sample1.md -o sample1.html"
```

## Beamerスライドの作成

MarkdownからBeamerプレゼンテーションのPDFを作成するために必要なものは以下。

- Markdownを書くためのエディタ
  - メモ帳でも書くことはできる
  - ちょっと気の利いたエディタならMarkdownのシンタックスハイライトやアウトライン解析に対応している
  - おすすめはVisual Studio Code
- Pandoc
  - 無料の文書形式変換ソフト→Markdownからだとhtmlやword，Latexなどいろいろ変換できる
  - おすすめは，WSL2(Windows Subsystem for Linux 2)上にインストールして使うこと
- Tex
  - MarkdownをWordやhtmlに変換するには不要
  - Beamerスライドの作成やLatex経由でPDFを作成するためには必須
  - こちらもおすすめはWSL2(Windows Subsystem for Linux 2)上にインストールして使うこと
- PDFファイルのビューア
  - WindowsであればSumatra PDF一択→Sumatra PDFは開いているファイルをロックしないので，ファイルを閉じなくても上書きできる(ファイルを上書きすると自動的に再読み込みされる)
  - Sumatra PDFは動作が軽いので，普段使いにもおすすめ
  - Macだとskimが良い
- フォント
  - [IPAフォント](https://moji.or.jp/ipafont/)がおすすめ

## Markdownファイルの作成

Beamerスライド作るときのポイントは以下。

- スライドタイトルを##(レベル2の見出し)で書く
  - レベル2の見出しがあると，自動的に新しいスライドが作成される
  - スライドタイトルなしで新しいスライドを作成するには，"---"を入れる
- テーマとかTeXのプリアンブルに書くような内容は，yamlヘッダの部分に書く

### Markdownファイルの作成例

```markdown

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

## yamlヘッダの説明

- yamlヘッダのメタ情報
    - title: プレゼンテーションのタイトル
    - subtitle: サブタイトル
    - author: 発表者
    - date: 日付

## TeXのプリアンブル部分の説明

1.  \\usepackage{bookmark}         : PDFにしおりを付ける
1.  \\usepackage{zxjatype}            : 日本語を使うために必要 
1.  \\usepackage[ipa]{zxjafont}     : フォントの指定(ここではIPAフォントを指定している)
1.  \\usetheme{metropolis}           : beamerのテーマ，metropolisおすすめ(TeX Liveに最初から入ってる)

---

## そのほか

- インライン数式:，$\log (1+x)=x$
- 別行立て数式
$$f(x)=\dfrac{1}{\sqrt{2\pi\sigma}}\exp(-\dfrac{(x-\mu)^ 2}{2\sigma^ 2})$$
- 表

||a|b|
|-|-|-|
|c|0|1|
|d|1|0|

---

- 図

![図のキャプション](figure.png){width=8cm}

```

### Pandocで変換

コマンドプロンプトかPowershellでサンプルの.mdと画像の入ったフォルダに移動して，以下のコマンドを実行すればPDFができるはず。

```bash
pandoc sample.md --pdf-engine=xelatex  -t beamer  -o sample.pdf 
```

### 変換結果

{{< figure src="../sample-0.png" alt="Beamer sample page 1" >}}

{{< figure src="../sample-1.png" alt="Beamer sample page 2" >}}

{{< figure src="../sample-2.png" alt="Beamer sample page 3" >}}

{{< figure src="../sample-3.png" alt="Beamer sample page 4" >}}

{{< figure src="../sample-4.png" alt="Beamer sample page 5" >}}
