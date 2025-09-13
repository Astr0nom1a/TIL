# Windows11上にDocker Desktopをインストールしてみた

タイトルの通り、Windows 11 Pro上でDocker Desktopをインストールしてみました。

インストール手順と躓いたところを記載していきます。

---

## 1. 公式サイトからインストーラーをダウンロード

公式サイトにアクセスし、Docker Desktop Installer.exeをダウンロードする。

**公式サイト**: [https://www.docker.com/](https://www.docker.com/)

私のCPUはIntel製だったので **AMD64** を選択しました。

## 2. Docker Desktop Installer.exe を実行する

`Docker Desktop Installer.exe` をダブルクリックで実行し、特に設定をいじらずにインストールします。

## 3. Windows機能を有効にする

Dockerを使うためには仮想化技術を有効にする必要があるため、Windows PowerShellを **管理者権限** で実行し、以下のコマンドを入力します。


### WSL (Windows Subsystem for Linux) を有効化

PowerShellで

```dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart```

### 仮想マシンプラットフォームを有効化

これも同様に

```dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart```


私の場合は、このコマンドを入力中に
以下のエラーが出てしまいました

    エラー: 14098
    コンポーネントストアが壊れています
    解決方法：コンポーネントストアの修復

これを解決するために

### 管理者権限のコマンドプロンプトで以下を順番に実行しました

・コンポーネントストアの状態確認
```DISM /Online /Cleanup-Image /CheckHealth```

・詳細スキャン　
```DISM /Online /Cleanup-Image /ScanHealth```

・修復実行(結構時間がかかります）
```DISM /Online /Cleanup-Image /RestoreHealth```


・コンポーネントストア修復完了後：
```sfc /scannow```
   
ここで再起動をかけ、再度、仮想マシンプラットフォームを有効化を試したところ成功しました。

Docker Desktop起動し、無事Dockerのインストールが終了しました。

---
# 感想

### Githubで見やすく書くのが難しかったです。

ただインストールするだけと思いきや、前提の部分でだいぶ詰まってしまいました。

次は、Oracle Database 23ai freeをDocker上で動かしたいと思います。


---

参考リンク

Docker公式ドキュメント[https://docs.docker.com/desktop/setup/install/windows-install/]

WSL 2インストールガイド[https://learn.microsoft.com/ja-jp/windows/wsl/install]
