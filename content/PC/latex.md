---
title: "WSLでLatexとPandoc"
weight: 20
---

## はじめに

- WSLを使えば仮想環境のLinuxでtexのコンパイルを行うことが可能
  - Windowsでもtex環境は構築できるがコンパイルが遅い
  - Windowsは文字コードの問題など，ならではのトラブルも多い
  - とはいえ文書を編集するときは使い慣れたWindows環境の方が捗るので，ソースの編集はWindows上で行い，コンパイルはWSLで行うという話
- PandocもWSLに入れておいた方が便利
  - Pandocはmarkdownで書いたファイルをいろいろな形式(PDF, Word, tex, htmlなどなど)に変換できるコンバータ
  - markdownはtexに比べてシンプルで可読性が高い  
  →基本的に執筆はmarkdownで行って，tex経由でPDF化するのが良いと思う
  - 数式や図表の挿入，文献参照(bibtex)などかなりの部分はmarkdownでも記述できる
  - 相互参照(texの\label, \ref)はpandoc-crossrefというフィルターをインストールすれば可能

## Latex

### Tex Liveのインストール

- fontconfigパッケージをインストールしておく

```bash
sudo apt intall libfontconfig
```

- Tex Liveインストーラを利用する
- [ここ](https://ftp.yz.yamagata-u.ac.jp/pub/CTAN/systems/texlive/Images/)からisoファイルをダウンロード
- ダウンロードしたisoファイルをWindowsでマウント
- WindowsのGドライブにマウントしたとすれば，以下のコマンドでWSLにもマウントできる

```bash
 sudo mkdir /mnt/g
 sudo mount -t drvfs G: /mnt/g
```

- マウントしたディレクトリに移動して，インストーラを実行(デフォルト設定で問題ないので"I"を入力してインストールを開始)

```bash
cd /mnt/g
sudo ./install-tl
```

- パスを通す(tlmgrコマンドを使って/usr/local/binにシンボリックリンクを追加，削除したいときは"add"を"remove"に)

```bash
sudo /usr/local/texlive/????/bin/*/tlmgr path add
```

- パッケージのアップデート

```bash
sudo tlmgr update --self --all
```

### Windowsからの使い方

- Windowsのコマンドラインで実行したいコマンドの前にwslをつけるだけ
- たとえば，Linuxのuplatexを使ってsample.texをコンパイルするには

```powershell
wsl uplatex sample.tex
```

- Latex用エディタの設定でコンパイルに用いるコマンドの頭にwslをつけておけばOk

### Latex環境の構築はVisual Studio Codeがおすすめ

- WSLを使うのであれば，Visual Studio Codeが便利
  - VSCodeがLinuxのインターフェイスになる(VSCodeを使って，Linux上のファイルの編集やコンパイラの呼び出しなどができる)
  - VSCodeはローカルのWSLだけでなく，リモート環境にもSSHで接続でき
  - 非力なラップトップでもVSCodeだけインストールしておけば，自宅などのPCにリモートアクセスしてソースの編集やコンパイルなどが可能
- VSCode用拡張機能のTex Workshopが便利

## Pandoc

### Pandocのインストール

- UbuntuのaptでインストールされるPandocはかなり古い
- 最新版は[Github](https://github.com/jgm/pandoc/releases/)から入手可能
- debファイルをダウンロード

```bash
wget https://github.com/jgm/pandoc/releases/download/2.14.0.1/pandoc-2.14.0.1-1-amd64.deb
```

補足：MacでおなじみのhomebrewはLinuxでも使える。homebrewのpandocは新しいので，homebrewを使ってインストールするのも良いと思う。アップデートが楽。

- インストールする

```bash
sudo apt install ./pandoc-2.14.0.1-1-amd64.deb
```

### Pandoc-crossrefのインストール

- Pandoc-crossrefはPandoc本体に対応したバージョンを使う
  - [Github](https://github.com/lierdakil/pandoc-crossref/releases/)からビルド済みのバイナリをダウンロード

```bash
wget https://github.com/lierdakil/pandoc-crossref/releases/download/v0.3.11.0a/pandoc-crossref-Linux.tar.xz
```

- 解凍

```bash
tar -xf pandoc-crossref-Linux.tar.xz
```

- ファイルを/usr/bin に移動

```bash
sudo mv pandoc-crossref /usr/bin
```

## 補足：やっぱり秀丸が良い？

- [秀丸](https://hide.maruo.co.jp/software/hidemaru.html)＋[祝鳥](https://www.ms.u-tokyo.ac.jp/~abenori/soft/fortex.html)＋[Sumatra PDF](https://www.sumatrapdfreader.org/free-pdf-reader)は論文執筆環境としてわたしも長らくお世話になってきた
  - 秀丸は起動も速いし動作が軽い(VSCodeは少し重い)
  - 祝鳥はWSL上のコンパイラを指定できる？(ちゃんと調べていないので不明)
  - 補完機能などは祝鳥を使い，コンパイルは自作マクロ(latexmkを実行するだけ)で良いかな  
  - PDFファイルはSumatra PDFで見る(Acrobatとは比較にならないくらい軽い上に，ファイルがロックされないのでPDFを開いたままTexのコンパイルが可能，しかもファイルが更新されると勝手に読み込み直してくれるので便利)
- WindowsのTex専用エディタ＋WSL上のコンパイラ環境なら，Texmakerがおすすめ
  - コンパイラの設定で，すべてのコマンドの前に"ubuntu run "を付ければwslにインストールしたTexでコンパイルされる
  - 日本語ならLatexに"ubuntu run uplatex \%.tex"，Bib(la)texに"ubuntu run upbibtex \%"，Dvipdfmに"ubuntu run dvipdfmx \%.dvi"くらい設定すれば十分
- 英語しか書かないならTexmakerから派生したTexStudioが良い(日本語も使えなくはないが，私の環境では漢字変換中の文字列が見えなくなるのであきらめた)

### WindowsでSynctexを使うときの問題

- Synctexは，PDFを編集中の場所(エディタのカーソル位置)にジャンプさせたり，PDFファイルからソースファイルの対応部分にジャンプさせたりすることができる機能
- VSCodeのWSL拡張ではまったく問題なく使えるが，WindowsのTex専用エディタを使うには工夫が必要
  - SynctexはLinux形式のパスでファイル名が書き込まれるので，Windowsのエディタでそのまま使うことはできない
  - .synctexファイルに書き込まれたLinux形式のパスをWindows形式に置換すれば，Windows上のエディタでもSynctexが使える
    - WSLのLinuxでWindowsのドライブのパスは/mnt/ドライブ名
    - Linuxパス(/mnt/c/...)をWindowsパス(c:\...)に変換するには，wslpathコマンドを使う
  - .synctexファイルを圧縮なしで出力(コンパイルのオプションに -synctex=-1 をつける)し，以下のようなhスクリプトに渡せば自動化可能
  - たとえば，texmakerで，Latex→DVIPDF→下の変換スクリプト→PDFビューアの順で実行するユーザーコマンドを登録すればよい

```bash
FILEPATH=$(wslpath $1) 
array=($(grep "/mnt/" $FILEPATH)) 
for i in ${array[@]}
do
FNL=${i##*:}
FNW=$(wslpath -m  ${FNL##*:})
sed -i -e "s|$FNL|$FNW|" $FILEPATH
done
```
