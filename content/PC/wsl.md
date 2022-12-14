---
title: "WSL(Windows Subsystem for Linux) 2 導入メモ"
weight: 10
---


{{< toc >}}

## WSL2？

- WSLはWindows上(仮想環境)で動作するLinux環境
- Linuxには便利なツールがある(多くのツールはWindowsに移植されているが，安定性ではLinuxに軍配)
- とくにLatexのコンパイルなどはLinuxの方が速いし安定している
- メインマシンをLinuxにしようってほどは尖ってないが，Linuxのツールを使いたいという場合にはWSL2が便利
- 基本的にはコマンド操作だが，最近GUIアプリも動くようになった

## WSL2のインストール

- BIOSで仮想化支援機能を有効にする
  - 有効になっているかどうかは[VirtualChecker](https://openlibsys.org/index-ja.html)で確認できる(最初から有効になっている場合もある)
  - BIOSでの設定方法はメーカーによって異なるが，IntelだとIntel Virtualization Technology(VTx)，AMDだとAMD-VとかSVMとかいう設定項目
- Windows 11であれば，PowerShellで以下のように入力するだけでインストールできる（仮想化支援機能の有効化は必要）。デフォルトでは，Ubuntuがインストールされる。

```powershell
wsl --install
```

---

Windows 10の場合は，以下の手順でインストールする。

基本的には，[マイクロソフトのページ](https://docs.microsoft.com/ja-jp/windows/wsl/install-win10)を見ながらやればできるが，以下に要点だけ示す。

- Windowsを1903以上のバージョンに更新する
  - Windows Updateで自動的に更新されていれば問題ない
  - 自動更新が行われない場合は，[Windows更新ツール](https://www.microsoft.com/ja-jp/software-download/windows10)を使う

- (PowerShell管理者で)Linux用Windowsサブシステムを有効にする

```powershell
dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart
```

- (PowerShell管理者で)仮想マシン機能を有効にする

```powershell
dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart
```

- WSL2 Linuxカーネルの更新
  - 環境によっては，この前に再起動が必要
  - [これ]( https://wslstorestorage.blob.core.windows.net/wslblob/wsl_update_x64.msi)をダウンロードして実行

- (PowerShell管理者で)WSL2を既定のバージョンに設定

```powershell
wsl --set-default-version 2
```

- [Microsoft Store](https://aka.ms/wslstore)からLinuxのディストリビューションをインストールする（おすすめはUbuntu，以下の説明はUbuntu前提）

---

## 初期設定

- インストールしたLinuxディストリビューションを起動する
  - 初回起動時は，設定が行われるため数分かかる
  - 起動すると新規ユーザー名とパスワードを聞かれる(パスワードは入力しても表示されないので注意)

- (Linuxのシェルで)パッケージのアップデート

```bash
sudo apt update
sudo apt upgrade -y
```

- WindowsからLinuxのファイルへアクセスするには，エクスプローラーのアドレスバーに"&#x00A5;&#x00A5;wsl$"と入力
- WindowsのコマンドプロンプトやPowerShellからLinuxのコマンドを使うには，wslコマンド，もしくはubuntu runコマンドを使う

```bash
wsl ls -l
ubuntu run ls -l
```
