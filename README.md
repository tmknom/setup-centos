# このスクリプトは何？

CentOSをコマンド一発でセキュアにセットアップするスクリプト

## 概要

CentOSの環境構築を高速化するため、Fabricのスクリプトとして提供する。

さくらのVPSで標準OSをインストールした場合に、最適化されている。

CentOSであればある程度流用が可能と思われるが、未検証なので注意すること。


## 前提条件

FabricのインストールとSSH公開鍵の作成は事前に行っておくこと。


# ダウンロード

```bash
$ git clone git@github.com:tmknom/setup-centos.git
$ cd setup-centos
```

## 事前設定

cloneしてきたら、fabfile.pyをテキストエディタで開いて編集する。

```bash
$ vi fabfile.py
```

以下の項目は必ず変更すること。

* 作業用ユーザ名：USER_NAME
* 作業用ユーザのパスワード：PASSWORD
* 接続先のVPSサーバのIPアドレス：env.hosts

次の項目も変更をすることを推奨。

* rootのメールアドレス：ROOT_MAIL_ADDLESS
* SSHのポート番号：SSH_PORT


## 使い方

提供しているFabricのタスクを実行する。

```bash
$ fab <task_name>
```

### setup タスク

初期セットアップ時に推奨される設定の変更及びツールのインストールを行う。

```bash
$ fab setup
```

主なセットアップ内容は以下のとおり。

1. 作業用ユーザの作成とsu/sudo権限の付与
2. 設定ファイルの自動バージョン管理(etckeeperのインストール)
3. SSHの設定：ポート変更／公開鍵認証の設定／パスワードログイン禁止／rootログイン禁止
4. iptables(ファイアウォール)の設定
5. システムの最新化とyum-cron導入による自動更新
6. IPv6と不要サービスの停止
7. ログ監視ツール(logwatch)の導入


### setup_all タスク

**setup** タスクの内容に加え、セキュリティ系ツールのインストールを行う。

```bash
$ fab setup_all
```

インストールされるセキュリティ系ツールは以下のとおり。

1. fail2ban
2. Clam AntiVirus
3. Rootkit Hunter
4. Tripwire


### sakura タスク

さくらのVPSでネットワークの通信速度が出ない時に実行する。

```bash
$ fab sakura
```

他のタスクとの併用も可能。

```bash
$ fab sakura setup
$ fab sakura setup_all
```

### setup_min タスク

最低限のセットアップのみを行う。

```bash
$ fab sakura_min
```

主なセットアップ内容は以下のとおり。

1. 作業用ユーザの作成とsu/sudo権限の付与
2. 設定ファイルの自動バージョン管理(etckeeperのインストール)
3. SSHの設定：ポート変更／公開鍵認証の設定／パスワードログイン禁止／rootログイン禁止
4. iptables(ファイアウォール)の設定
5. システムの最新化とyum-cron導入による自動更新



### install_security_tools タスク

セキュリティ系ツールのインストールのみ行う。

```bash
$ fab install_security_tools
```

**setup** タスク実行後に、セキュリティ系ツールを導入する時などに使う。

なお、以下の二つのコマンドの動作は同じである。

```bash
$ fab setup_all
$ fab setup install_security_tools
```


## 参考情報

### Fabricのインストール

Mac OS Xの場合

```bash
$ brew install fabric
$ fab -V
Fabric 1.10.1
```

その他の場合は[Fabric公式サイト](http://www.fabfile.org/)などを参照してほしい。


### SSH公開鍵の作成

```bash
$ ssh-keygen -t rsa -C "your_email@example.com"
```


## ライセンス

MIT License, see LICENSE.txt.


