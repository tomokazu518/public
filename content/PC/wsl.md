---
title: "WSL(Windows Subsystem for Linux) 2 導入メモ"
weight: 10
---


{{< toc >}}

## WSL2？

- WSLはLinuxのためのWindows上の仮想環境
- LinuxのプログラムをWindowsで利用したいときに便利
  - LatexのコンパイルはLinuxの方が速いし安定しているのでWSLでやるのがおすすめ
  - MacやLinuxと共通の環境を構築できる
- 基本的にはコマンド操作だがGUIアプリも動く

## WSL2のインストール

- BIOSで仮想化支援機能を有効にする
  - 最初から有効になっていることも多い
  - BIOSでの設定方法はメーカーによって異なるが，IntelだとIntel Virtualization Technology(VTx)，AMDだとAMD-VとかSVMとかいう設定項目
- Windows 11では，PowerShellで以下のように入力するだけでインストールできる(仮想化支援機能の有効化は必要)。デフォルトでは，Ubuntuがインストールされる。

```powershell
wsl --install
```

- すでにWSLがインストールされている場合は，念のためアップデートしてUbuntuをインストール。

```powershell
wsl --update
wsl install -d Ubuntu
```

## 初期設定

- インストールしたLinuxディストリビューションを起動する(スタートメニューにUbuntuのショートカットができているはず)
  - 初回起動時は，設定が行われるため数分かかる
  - 起動すると新規ユーザー名とパスワードを聞かれる(パスワードは入力しても表示されないので注意)
- (Linuxのシェルで)パッケージのアップデート

```bash
sudo apt update
sudo apt upgrade -y
```

- WindowsのコマンドプロンプトやPowerShellからLinuxのコマンドを使うには，wslコマンド，もしくはubuntu runコマンドを使う

```bash
wsl ls -l
ubuntu run ls -l
```

## WindowsとWSL間のファイル共有

- WindowsからLinuxのファイルへアクセスするには，エクスプローラーのアドレスバーに"&#x00A5;&#x00A5;wsl$"と入力
- Linuxからは，/mnt/下にWindows側のドライブがマウントされる(Cドライブは/mnt/c/)
- ただし，LinuxとWindowsでは互いにファイルシステムが違うため，アクセスに時間がかかる
  - ちょっとした読み書きには支障ないが，Texのコンパイルなど頻繁に多くのファイルを書き換えるような場合には体感できるくらい遅い
  - Linuxで使うファイル(texのソースなど)はLinux側，Windowsで使うファイル(MS Officeのファイルなど)はWindows側に置いておくのが良い
- わたしは[Syncthing](https://syncthing.net/)というツールを使って，WindowsとLinuxの作業ディレクトリを同期している(両方に同じ内容のファイルが存在することになるので冗長ではある)
  - ちなみにSyncthingは良くできていて，ほかのマシンとファイルを同期するのに便利
  - 分散型なのでサーバは必要ないが，常時起動しているPCが一台あると同期漏れがなく安心
  - Raspberry Piなどを拠点にするのが良いかも