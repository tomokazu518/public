---
title: "WSLでRStudio Server"
weight: 40
---

{{< toc >}}

## RStudio Server？

- RStudioをブラウザで使うことができる，自前のRStudio Cloud
- インターフェイスや操作性はデスクトップ版RStudioとほぼ変わらない
- メリット
  - サーバー1台に入れておけば，どのマシンからも同じ環境で分析可能
  - 中断した作業を別のマシンでシームレスに再開することができる
  - iPadやスマホでも使うことができる(実際に使うかどうかは別として)
- RとRStudioはWindowsで使うといろいろな問題が起きるので，WSLにRStudioをインストールして使うのがおすすめ

## WSLへのインストール

### R本体のインストール

- 基本的には，[RStudio公式](https://support.rstudio.com/hc/en-us/articles/360049776974-Using-RStudio-Server-in-Windows-WSL2)で解説されている通り
- [Rで計量政治学入門](https://shohei-doi.github.io/quant_polisci/wsl-rstudio.html)にも解説がある  
  →こっちの方が良いと思う
- まず，証明書とレポジトリを追加

```bash
wget -qO- https://cloud.r-project.org/bin/linux/ubuntu/marutter_pubkey.asc | sudo tee -a /etc/apt/trusted.gpg.d/cran_ubuntu_key.a
sudo add-apt-repository 'deb https://cloud.r-project.org/bin/linux/ubuntu $(lsb_release -cs)-cran40/'
```

- RおよびRStudio Serverを使うために必要なパッケージをインストール

```bash
sudo apt install -y r-base r-base-core r-recommended r-base-dev gdebi-core build-essential libcurl4-gnutls-dev libxml2-dev libssl-dev
```

### RStudio Serverのインストール

- Ubuntu用のRStudio Serverをダウンロード(jammyの部分はUbuntuのバージョンによって変更)

```bash
wget https://rstudio.org/download/latest/stable/server/$(lsb_release -cs)/rstudio-server-latest-amd64.deb
```

- RStudio Serverのインストール

```bash
sudo gdebi rstudio-server-latest-amd64.deb
```

- インストールが完了するとsystemdに登録されるので，WSLを起動すれば自動的にRStudio Serverがバックグラウンドで動き始める
- [http://localhost:8787](http://localhost:8787)にアクセスしてRStudio Serverが動いていることを確認
- ユーザー名とパスワードは，Ubuntuに設定したもの
- Chromeでショートカットを作成しウィンドウで起動するようにすれば，デスクトップ版のRStudioとほぼ同じように使える
- LAN内のほかのPCやインターネット経由でもアクセスできるようにすれば便利だが，このまま外部からアクセスできるようにするのは危険なので注意

### tidyverseをインストール

- Rにtidyverseパッケージをインストールするのに必要なパッケージをインストール

```bash
sudo apt install libcurl4-openssl-dev libxml2-dev libfontconfig1-dev libharfbuzz-dev libfribidi-dev libfreetype6-dev libpng-dev libtiff5-dev libjpeg-dev
```

- わたしの場合は，上記だけでtidyverseをインストールすることができた
- 足りないパッケージがある場合にはエラーメッセージを読んでインストールする


## 通信を暗号化する

前はいろいろ具体的な方法を書いていたが，わたし自信コンピュータにそれほど詳しいわけではなく，セキュリティ上問題がある気がするので，概要にとどめておくことにした。

Chromeでは以前localhostでもhttpでアクセスするとブラウザに"このサイトへの接続は安全ではありません"とセキュリティの警告が表示されていて邪魔たったが，現在は表示されないので，LANやインターネット上のほかのマシンからアクセスする場合以外は暗号化する必要はないと思う。

ほかのマシンからアクセスする際に通信を暗号化する方法はいろいろあるが，一番手間がかからないのはSSHポート転送を使う方法。アクセス先がlocalhostになるので，ブラウザの警告も表示されない。

インターネットに直接公開するのはあり得ないと思うが，VPNでの利用を前提にNginxなどのリバースプロキシを使うのも良い。証明書は[Let's encrypt](https://letsencrypt.org/ja/)で取得できる。ドメイン名が必要だが，[MyDNS.JP](https://www.mydns.jp/)のダイナミックDNSでOk。MyDNSでLet's encryptの証明書を取得する[DirectEdit](https://github.com/disco-v8/DirectEdit/)というスクリプトが公開されている。
