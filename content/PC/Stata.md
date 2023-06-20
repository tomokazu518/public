---
title: "WSLでStata"
weight: 100
---

## はじめに

StataのLinux版はWSLでも使うことができる。Linux版はCUIで使うこともできるし，GUIも普通に動く。とくにWindows版のStataで困っていなければまったく必要ない。個人的には，シェルやRStudioからStataのコードを実行できるのが便利。Jupyter notebookでも使える。

## Stataのインストール

WSLにLinux版Stataをインストールする方法は，[公式ページ](https://www.stata.com/support/faqs/unix/install-download-on-linux/)に説明されている通り。
ただし，足りないパッケージがあって起動しないので，libtinfoとlibncursesをインストールしておく。

 ```bash
sudo apt install -y libtinfo5 libncurses5
 ```

上記の公式ページの通りにインストールを実行，終わったら画面に表示される指示にしたがい，(rootのままで)stinitを実行する。

```bash
/usr/local/stata17/stinit
```

指示にしたがいながらライセンス情報を入力すればOk。

WSLでもGUIが使える。ただし，libgtk2.0-0というパッケージが足りないのでインストールしておく。

```bash
sudo apt install libgtk2.0-0
```

Stataを起動するには，/usr/local/stata17/にインストールされた以下のコマンドを実行(わたしのライセンスはMPだが，全部インストールされた)。

|バージョン|CUI起動コマンド|GUI起動コマンド|
|-|-|-|
|Stata BE|stata|xstata|
|Stata SE|stata-se|xstata-se|
|Stata MP|stata-mp|xstata-mp|

/usr/local/stata17にPathを通すか，エイリアスを作成しておくと便利。あと，rootになっているついでに，Stataを起動してupdate allしておくと良い。

## RStudioでStataを使う

個人的にStataのDoファイル・エディタはあまりしっくりこないので，RStudioを使っている。RStudioのターミナルでStataを起動しておいて，エディタの右下にある言語を"Shell"に設定すれば，Ctrl+EnterでDoファイルを1行実行して次の行に移動できる。

## Jupyter notebookでStataを使う

PythonなどでJupyter notebookを使っているのであれば，StataもJupyter notebookで使うと良いかもしれない。

### Jupyter ntoebookのインストール

たぶんAnacondaとかJupyter Labとかフルセットの環境を構築した方が良いと思うが，ここでは必要最低限でいきたい。

もしPythonのパッケージ・マネージャであるpipがインストールされていなければ，インストールする。

```bash
sudo apt install python3-pip
```

pipを使って，Jupyter notebookをインストール。

```bash
pip3 install notebook
```

~/.local/bin にインストールされるので，Pathが通っていることを確認しておく。

### stata_kernelのインストール

次に，Jupyter notebookでStataを操作するためのカーネルをインストール。

```bash
pip3 install stata_kernel
python -m stata_kernel.install
```

(2022.9.7現在) これでインストールされたstata_kernelは，Stata 17で使う場合にグラフ表示に不具合がある。Stata 17で使う場合には，[github](https://github.com/kylebarron/stata_kernel/tree/master/stata_kernel)からstata_session.pyの最新版をダウンロードして，~/.local/lib/python3.10/site-packages/stata_kernel/session_stata.pyを置き換えればOk。逆にStata 16以前の場合には，stata_session.pyを最新版に置き換えると不具合が起きると思う。

あとは，~/ に.stata_kernel.confが作成されているので，2行目にStataのPathを設定する(たとえば，stata_path=/usr/local/stata17/stata-mp)。

Jupyter notebookを起動して，kernelをStataに設定すれば使えるはず。

```bash
jupyter-notebook
```

自動的にブラウザが開くはずだが，ターミナルによっては開かない場合もあるみたい。その場合は，http://localhost:8888 にアクセスすればOk。

### VS Codeで使う

ブラウザで使うのも良いが，やはりVS Codeが便利。

- python
- jupyter

のextensionを入れれば快適な分析環境になる。
