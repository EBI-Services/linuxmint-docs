---
title: Windowsアプリの動かし方（Winboat）
---

Linuxでは、Windowsのアプリを動かすことは基本的にできません。しかし、LinuxでもWindowsのアプリを使えるようにする方法や、Linux上でWindowsを動かす方法はいくつもあります。

**Winboat**というアプリを使うと、スムーズに使用できます。このドキュメントでは、Winboatの使い方を紹介します。

:::warning

この記事は上級者向けです。また、仮想環境を利用した場合はアンチチート機能のあるゲームなどは起動できません。

:::

## Winboatのインストール方法

### 1. 公式サイトにアクセス

https://www.winboat.app にアクセスします。

![alt text](image-31.png)

「Download」を押した後、「Debian」をクリックし、「Download .deb」を押します。

![alt text](image-34.png)

ダウンロードが完了したら、「winboat-…」をクリックして開きます。

![alt text](image-35.png)

「パッケージをインストール」を押してインストールします。

![alt text](image-36.png)

認証して続行します。

![alt text](image-37.png)

この画面になったら完了です。

![alt text](image-38.png)

アプリメニューなどから起動します。

Winboatは画面の大きさが決まっているようで、はみ出してしまいます。このまま進めましょう。「Next」を押します。

![alt text](image-39.png)

I agreeを押します。

![alt text](image-40.png)

:::tip

本来利用規約はちゃんと読むべきものですが、このライセンスは「MIT License」といい、「何かあっても開発者は責任を負いません」以外のことは利用者にとって関係ありません。その内容だけ確認し、問題なければ同意してください。

:::

Winboatを使うのに要求がいくつかあります。

![alt text](image-41.png)

- 4GBのRAMがあること
- 2コア以上のCPUであること
- BIOSで仮想化が有効になっていること
- DockerまたはPodmanをインストールしていること
- Docker Compose v2をインストールしていること
- ユーザーがdockerまたはpodmanグループに所属している＝利用権限があること
- Docker daemonが動いていること
- FreeRDPの3.?.?がインストール済みであること

この全てを満たす必要があります。一部のみ解説します。

#### BIOS仮想化の有効化

BIOSに入り、仮想化を有効にする必要があります。BIOSにより方法や可否が異なるため、BIOSの型番を特定して検索する必要があります。

### 2. 依存環境のインストールと権限付与

:::warning

このコマンドを実行すると、すべての依存環境を確認無しで自動でインストールします。1文字間違えると全く動かないため、コピー＆ペーストを推奨します。

:::

```sh
# Add Docker's official GPG key:
sudo apt update
sudo apt install ca-certificates curl freerdp3-x11
sudo install -m 0755 -d /etc/apt/keyrings
sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc

# Add the repository to Apt sources:
sudo tee /etc/apt/sources.list.d/docker.sources <<EOF
Types: deb
URIs: https://download.docker.com/linux/ubuntu
Suites: $(. /etc/os-release && echo "${UBUNTU_CODENAME:-$VERSION_CODENAME}")
Components: stable
Signed-By: /etc/apt/keyrings/docker.asc
EOF

sudo apt update
sudo apt install -y docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin

# Add user to docker group
sudo groupadd docker
sudo usermod -aG docker $USER
```

このコマンドを打ったら、再起動をします。

![alt text](image-42.png)

再起動をしたあとこの画面になったら、Nextを押します。

### 3. インストール場所の決定

インストール場所を決定します。特に興味がなければそのままNextを押してください。

![alt text](image-43.png)

### 4. Windowsの選択

使いたいWindowsのエディションと言語を選択してください。

![alt text](image-44.png)

:::warning

Windowsのライセンスキーが手元にある場合、HomeのキーでProを起動することなどはできないため注意してください。

:::

今回はWindows 11 Proと日本語で続行します。

![alt text](image-45.png)

### 5. ユーザーの作成

ユーザーを作成します。

![alt text](image-46.png)

入力して続行します。

### 6. スペックの決定

割り当てる計算リソースを決定します。

![alt text](image-47.png)

### 7. フォルダー共有の決定

WindowsとLinuxの間で共有のフォルダーを作るかどうかを決定します。

:::warning

画面にも書いてあるとおり、Windows専用のウィルスにかかってしまった時、そのフォルダーも汚染されることになります。リスクを受け入れられる場合のみチェックを入れて設定してください。

:::

![alt text](image-48.png)

### 8. インストール

問題なければインストールを実行します。

![alt text](image-49.png)

しばらく待機します。

![alt text](image-50.png)

この画面になったら完了です。

![alt text](image-51.png)

## 使い方

### ダッシュボード

この状態になっていれば起動できています。

![alt text](image-52.png)

四角のボタンを押して停止、縦線二本のボタンを押して一時停止できます。

### アプリ一覧

アプリを使うこともできるようになりました。

![alt text](image-53.png)

試しにメモ帳を開くとこのような画面になります。

![alt text](image-54.png)

## 参考文献

- https://qiita.com/Inqb8tr_jp/items/243a9ea9c8e22344e09b
- https://gigazine.net/news/20260101-winboat/