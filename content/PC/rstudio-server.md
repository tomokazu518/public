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

## WSLへのインストール

### R本体のインストール

- 基本的には，[ここ](https://support.rstudio.com/hc/en-us/articles/360049776974-Using-RStudio-Server-in-Windows-WSL2)で解説されている通り
- まず，証明書とレポジトリを追加(jammyの部分はUbuntuのバージョンによって変更)

```bash
sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys E298A3A825C0D65DFD57CBB651716619E084DAB9
sudo add-apt-repository 'deb https://cloud.r-project.org/bin/linux/ubuntu jammy-cran40/'
```

- RおよびRStudio Serverを使うために必要なパッケージをインストール

```bash
sudo apt install -y r-base r-base-core r-recommended r-base-dev gdebi-core build-essential libcurl4-gnutls-dev libxml2-dev libssl-dev
```

### RStudio Serverのインストール

- Ubuntu用のRStudio Serverをダウンロード(jammyの部分はUbuntuのバージョンによって変更)

```bash
wget https://rstudio.org/download/latest/stable/server/jammy/rstudio-server-latest-amd64.deb
```

- RStudio Serverのインストール( “Couldn’t find an alternative telinit implementation to spawn.”というメッセージが出るが，無視してよいとのこと)

```bash
sudo gdebi rstudio-server-latest-amd64.deb
```

- RStudio Serverを起動(毎回起動させるのが面倒であれば，Windowsが起動したときにこのコマンドを実行するようタスクスケジューラに登録するとよい)

```bash
sudo rstudio-server start
```

- [http://localhost:8787](http://localhost:8787)にアクセスしてRStudio Serverが動いていることを確認
- ユーザー名とパスワードは，Ubuntuに設定したものを使う
- LAN内のほかのPCやインターネット経由でもアクセスできるようにすれば便利だが，このまま外部からアクセスできるようにするのは危険

### RStudio ServerでTex Liveを使う

- R MarkdownでPDFを作成するためには，Texが必要
- RStudioはTinyTexを推奨しているが，重複してインストールするのを避けたければTex Liveを使えばよい
  - Tex Liveをインストールしたときに，/usr/local/binにシンボリックリンクを張っておく
  - Rの環境変数PATHに/usr/local/binを追加  
  →Rの起動時に毎回実行するように，.Rprofileにこの内容を追記しておくとよい(.RprofileはR起動時に実行されるファイルでホームフォルダにある，ない場合は新規に作成してこの内容を書いておく)

```r
Sys.setenv(PATH = paste(Sys.getenv("PATH"), "/usr/local/bin", sep = ":"))
```

## 通信を暗号化する

### リバースプロキシ

- localhostでも，httpでアクセスするとブラウザに"このサイトへの接続は安全ではありません"とセキュリティの警告が表示される(localで使う分にはこのままでも良いかもしれないが，警告は消したい)
  - 通信をhttpsで行うために，Nginxのリバースプロキシを利用する
  - クライアント(ブラウザ)からはNginxを介してRStudio Serverに接続，クライアントとNginxとの間はhttpsで通信
- Nginxのインストール

```bash
sudo apt install nginx
```

### 証明書の作成

- 暗号化された通信を行うためには，証明書が必要
- mkcertというツールを使えば，簡単に証明書が作れる
- クライアント側(Windows)にmkcertをインストールして証明書を発行し，WSLのNginxで利用する
  - mkcertのインストールは，chocolateyを使うのが簡単
  - chocolateyはWindowsのパッケージマネージャ，さまざまなソフトのインストールやアップデートが簡単にできるので，インストールしておくと良い
  - chocolateyのインストールは[ここ](https://chocolatey.org/install)を見て，powershell(管理者)でインストールコマンドを実行するだけ
- Powershellでmkcertのインストール，ローカル認証局のインストール，localhostの証明書と鍵の作成

```powershell {linenos=true}
  choco install mkcert 
  mkcert -install
  mkcert localhost
```

- カレントディレクトリにlocalhost.pemとlocalhost-key.pemの2つのファイルが作成されるので，WSLでそれを/etc/nginx/sslに移動させる  
→/etc/nginx/sslフォルダがない場合は作成する

```bash {linenos=true}
mkdir /etc/nginx/ssl # フォルダが存在しない場合は作成する
sudo mv localhost.pem /etc/nginx/ssl/
sudo mv localhost-key.pem/etc/nginx/ssl/
```

### Nginxの設定

- /etc/nginx/site-available内に下の内容でファイルを作成し，/etc/nginx/site-enabledにシンボリックリンクを作成
- ファイル名は何でもよいが，localhostとでもしておく

```bash  {linenos=true}
sudo nano /etc/nginx/sites-available/localhost 
# nanoで下の設定内容を貼り付けて保存

# /etc/nginx/sites-enabledに移動して，作成したファイルのシンボリックリンクを作成
cd /etc/nginx/sites-enabled 
ln -s /etc/nginx/sites-available/localhost localhost
```

```nginx {linenos=true}
server {
   # 443ポート(https)へのアクセスを転送
    listen 443 ssl;

   # 証明書を設定
    ssl_certificate     /etc/nginx/ssl/localhost.pem;
    ssl_certificate_key /etc/nginx/ssl/localhost-key.pem;
    
   # 転送先をhttps://localhost:8787に
    location /rstudio/ {
      rewrite ^/rstudio/(.*)$ /$1 break;
      proxy_pass http://localhost:8787;
      proxy_redirect http://localhost:8787/ $scheme://$http_host/rstudio/;
      proxy_http_version 1.1;
      proxy_set_header Upgrade $http_upgrade;
      proxy_set_header Connection "upgrade";
      proxy_read_timeout 20d;
    }
  }
```

- nginxを起動すれば， "https://localhost/rstudio" でRStudio Serverにアクセスできるようになる

```bash
sudo service nginx start
```

## クライアントマシンからアクセスできるように設定

- WSLにインストールしたRStudio Serverに，LAN内の別のPCからアクセスできるようにすることも可能
- しかし，WSL2のIPアドレスは起動するたびに変わる(ホストマシンからはlocalhostでアクセス可能)ので，ほかのマシンからアクセスするためにはポートフォワーディングを設定して，ホストマシンの特定のポートに対するアクセスをWSLに転送する必要がある
- ポートフォワーディングを設定するためのbatファイルを作成

```bat {linenos=true}
# WSLのIPアドレスの取得
for /f "tokens=* usebackq" %%F in (`wsl --distribution Ubuntu --exec hostname --all-ip-addresses`) do set IP=%%F 

# ポート443をWSLのIPアドレスに転送(古い設定を削除後，新しいIPアドレスを設定)
netsh interface portproxy delete v4tov4 listenport=443 connectaddress=%IP%
netsh interface portproxy add v4tov4 listenport=443 connectaddress=%IP%

# ついでにnginx, RStudio-serverもここで起動させる
ubuntu run -u root service nginx restart
ubuntu run -u root rstudio-server restart
```

- サーバーPCの起動時に毎回このスクリプトを実行(Windowsのタスクスケジューラに登録すると良い)
  - トリガーはログオン時，ユーザーがログオンしているときのみ実行する，最上位の権限で実行するにチェックを入れる  
- これでLAN内からであれば，"https://IPadress" にアクセスして，RStudio Serverを使うことができる
- ただし，クライアントマシン側にも証明書をインストールしなければブラウザで警告が出る
  - mkcertの証明書はあくまでローカルでのテスト用なので，ホストマシン以外からもアクセスする場合に使うことは推奨しない
  - Let's Encryptというサービスで正式な証明書を取得するのが良い  
  →ダイナミックDNSサービスの[mydns.jp](https://www.mydns.jp)を利用すれば，固定IPなしでもLet's Encryptで証明書を取得できる
  - とはいえ，このままインターネットからアクセスできるようにするのは危険すぎるので，VPNを使った方が良い

## LANの外から使う

- そのままインターネットからアクセスできるようにするのは危険なので，VPNを使う
  - [Tailscale](https://tailscale.com/)を使えば簡単にVPNを構築できる，しかも無料
  - [SoftEther](https://ja.softether.org/)も比較的簡単，無料
- サーバーを常時起動していると電気代とかが気になる？
  - 電気代がほとんどかからないRaspberry PIにTailscaleかSoftEtherを入れて常時起動しておき，Raspberry PIからWake on Lanでサーバーの電源を入れると良い
  - なんならRaspberry PIでRStudio Serverホストしてもいいかも(Raspberry PI 4とかだと十分に使えそうだけど，試してない)
