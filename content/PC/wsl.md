---
title: "WSL(Windows Subsystem for Linux) 2 導入メモ"
weight: 10
---


{{< toc >}}

## WSL2？

- WSLはWindows上(仮想環境)で動作するLinux
- LinuxのツールをWindowsから使いたいときに便利
  - LatexのコンパイルなどはLinuxの方が速いし安定しているのでWSLでやるのがおすすめ
  - RStudio Serverも動く
- 基本的にはコマンド操作だがGUIアプリも動く

## WSL2のインストール

- BIOSで仮想化支援機能を有効にする
  - 有効になっているかどうかは[VirtualChecker](https://openlibsys.org/index-ja.html)で確認できる(最初から有効になっている場合もある)
  - BIOSでの設定方法はメーカーによって異なるが，IntelだとIntel Virtualization Technology(VTx)，AMDだとAMD-VとかSVMとかいう設定項目
- Windows 11では，PowerShellで以下のように入力するだけでインストールできる(仮想化支援機能の有効化は必要)。デフォルトでは，Ubuntuがインストールされる。

```powershell
wsl --install
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
  - ただし，LinuxからWindows側ファイルへのアクセスには時間がかかる  
  →Texのコンパイルなど頻繁に多くのファイルを書き換えるような場合には体感できるくらい遅い
  - わたしは[Syncthing](https://syncthing.net/)というツールを使って，WindowsとLinuxの作業ディレクトリを同期している(両方に同じ内容のファイルが存在することになるので冗長ではある)
  - ちなみにSyncthingは良くできていて，ほかのマシンとファイルを同期するのに便利(分散型なのでサーバは必要ないが，一台は最新バージョンが同期されたマシンの電源が入っていないといけないので，Raspberry Piなどを拠点にするのが良いかも)