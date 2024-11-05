---
# You can also start simply with 'default'
theme: seriph
# random image from a curated Unsplash collection by Anthony
# like them? see https://unsplash.com/collections/94734566/slidev
background: bg.png
# some information about your slides (markdown enabled)
title: 【サクッと!!】Laravel ローカル開発環境構築ハンズオン
info: |
  2024/11/07 [ペチオブ](https://phper-oop.connpass.com) にて開催するLaravel Sailハンズオン資料です。
# apply unocss classes to the current slide
class: text-center
# https://sli.dev/features/drawing
drawings:
  persist: false
# slide transition: https://sli.dev/guide/animations.html#slide-transitions
transition: slide-left
# enable MDC Syntax: https://sli.dev/features/mdc
mdc: true
# take snapshot for each slide in the overview
overviewSnapshots: true
favicon: /ucan.jpg
---

# Laravel ローカル開発環境構築ハンズオン

https://phper-oop.connpass.com/event/333013

---

# 自己紹介

自己紹介を書く

---

# 準備1

Dockerがインストールされていることを確認する。
(Windowsの場合はWSL2のUbuntu上で実行する)

```
$ docker -v
Docker version 27.3.1, build ce12230
```

hello-world が実行できることを確認する

```
$ docker run hello-world
Hello from Docker!

...
```

---

# 準備2

起動しているコンテナが合った場合は、コンテナを削除しておく。

```
$ docker ps -aq
$ docker rm -f $(docker ps -aq)
```

- `docker ps`: 現在稼働中のコンテナの一覧を表示
  - `-a`: 停止しているコンテナも含めてすべてのコンテナ
  - `-q`: コンテナIDだけを表示
- `docker rm`: 指定されたDockerコンテナを削除
  - `-f`: 実行中のコンテナを強制的に停止して削除

---

# 準備3

今回使用するハンズオン用ディレクトリを作成します。

```
$ mkdir sail-handson
$ cd sail-handson
```

---

# Let's Handson

```bash
$ curl -s "https://laravel.build/example-app" | bash

...

Please provide your password so we can make some final adjustments to your application's permissions.
Password: [ログインパスワードを入力]
```

- `example-app` はアプリケーション名(ディレクトリ名)を入れる
  - 英数字とハイフン、アンダーバーのみ
- インストールの最後に権限設定でログインパスワードを求められる
- mysql, redis, meilisearch, mailpit, selenium

---

# What is Laravel?

- PHP用のオープンソースWebアプリケーションフレームワーク
- 認証システム、ルーティング、キャッシュ、セッション管理などが標準搭載
- PHPのフレームワークは多数...Symfony, CakePHP, BEAR.Sunday, Zend Framework, Yii Framework, Phalcon, Slim Framework, FuelPHPなど

---

# Laravel is trend?

https://trends.google.co.jp/trends/explore?date=all&geo=JP&q=Laravel,Symfony,CakePHP,Rails,Django#TIMESERIES

<p><img src="/trend.png" class="h-90"></p>

---
layout: two-cols
---

# What is version?

| Version   | Release    | PHP      | 
| :-------: | :--------: | :------: | 
| 12.0      | 2025-1Q    | >= 8.2   | 
| 11.0      | 2024-03-12 | >= 8.2   | 
| 10.0      | 2023-02-14 | >= 8.1   | 
| 9.0       | 2022-02-08 | >= 8.0.2 | 
| 8.0       | 2020-09-08 | >= 7.3   | 
| 7.0       | 2020-03-03 | >= 7.2.5 | 
| 6.0 (LTS) | 2019-09-03 | >= 7.2   | 
| 5.8       | 2019-02-26 | >= 7.1.3 | 

::right::

<h1><span style="color: #3D3431">-</span></h1>

<table class="m-1">
    <thead>
        <tr>
            <th>Version</th>
            <th>Release</th>
            <th>PHP</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>5.7</td>
            <td>2018-09-04</td>
            <td>&gt;= 7.1.3</td>
        </tr>
        <tr>
            <td>5.6</td>
            <td>2018-02-07</td>
            <td>&gt;= 7.1.3</td>
        </tr>
        <tr>
            <td>5.5 (LTS)</td>
            <td>2017-08-30</td>
            <td>&gt;= 7.0.0</td>
        </tr>
        <tr>
            <td>5.4</td>
            <td>2017-01-24</td>
            <td>&gt;= 5.6.4</td>
        </tr>
        <tr>
            <td>5.3</td>
            <td>2016-08-23</td>
            <td>&gt;= 5.6.4</td>
        </tr>
        <tr>
            <td>5.2</td>
            <td>2015-12-21</td>
            <td>&gt;= 5.5.9</td>
        </tr>
        <tr>
            <td>5.1 (LTS)</td>
            <td>2015-05-09</td>
            <td>&gt;= 5.5.9</td>
        </tr>
        <tr>
            <td>5.0</td>
            <td>2015-02-04</td>
            <td></td>
        </tr>
    </tbody>
</table>

---

# Laravel Release Cycle

- メジャーリリース
  - 年1リリース(2〜3月が多い)
  - Laravel 8.0 までは年2リリースだった
- サポートポリシー
  - バグ修正はリリースから1年半
  - セキュリティ修正はリリースから2年間
  - LTS(Long Term Support)は廃止(Laravel 6.0)
- バージョニング
  - Laravel 6.0 からセマンティックバージョニングへ

---

# Laravel is history

- 2011年: Taylor Otwellがシンプルで使いやすいPHPフレームワークとして開発開始。Laravel1.0は簡素な作り
- 2012年-2013年: MVCアーキテクチャ、Composerサポート、Artisan CLI、データベース移行が追加され注目
- 2014年-2015年: Laravel 5.0でディレクトリ構造の刷新、HTTPミドルウェア、タスクスケジューリング等が導入
- 2016年-2019年: Laravel Scout、Passport、Horizonなどのエコシステムが拡大し、コミュニティが急成長
- 2020年以降: Jetstream、Sanctum、Livewire、Laravel 8.0以降のモダンな機能が追加。Vaporでサーバーレスデプロイも実現
現在: 世界中で人気を集め、初心者からプロまで利用されるPHPフレームワークのトップに。

---

# What is Laravel Sail?

<p><img src="/logo.svg" class="h-30"></p>

- Laravel公式提供の開発環境
- 内部的にDocker Composeを利用
- Sailコマンドを経由してLaravelに関するコマンドを実行
- Dockerの経験を必要とせずLaravel開発が行えることがコンセプトのツール

---

# ところで、そろそろビルド終わった？

終わらない人用コマンド  
`example-app` ディレクトリを削除して再実行

```bash
$ curl -s "https://laravel.build/example-app?with=mysql" | bash
```

---

# 補足: Sailサービスの選択

```bash
$ curl -s "https://laravel.build/example-app?with=mysql,redis" | bash
```

`with` クエリ文字列変数を使って、設定するサービスを選択できます。

- 利用可能なサービス: `mysql`, `pgsql`, `mariadb`, `redis`, `memcached`, `meilisearch`, `typesence`, `minio`, `selenium`, `mailpit`
- デフォルトサービス: `mysql`, `redis`, `meilisearch`, `selenium`, `mailpit`

# Docker イメージビルド&コンテナ作成

```bash
$ cd example-app
$ ./vendor/bin/sail up -d
```

`-d` を付けるとデタッチドモード(バックグラウンド)で実行します。
付けない場合はフォアグラウンドになり、ターミナルが占有されるので、別ターミナルでコマンドを打つ必要があります。

---

# 動作確認

http://localhost

---
layout: two-cols
---

## ダークモード

<p><img src="/dark.png" class="h-80"></p>

::right::

## ライトモード

<p><img src="/light.png" class="h-80"></p>

---

# localhost で接続が拒否されました。

http://localhost へアクセスして次のエラーが出た場合

<p><img src="/localhost.png" class="h-90"></p>

他にポートを占有しているサービスが不明な場合は利用しているプロセスを調べる

---

# localhost で接続が拒否された場合のコマンド

```
$ sudo lsof -P -i:80
COMMAND     PID USER   FD   TYPE             DEVICE SIZE/OFF NODE NAME
OrbStack  26865 ucan   86u  IPv4 0xe7f199b22192fe94      0t0  TCP *:80 (LISTEN)
```

表示されたPIDを殺します。
例えば `26865` PIDを削除したい場合は次のコマンドを実行します。

```
$ sudo kill -9 26865
```

再度、sailコマンドからコンテナを再作成してください。

```
$ sail down
$ sail up -d
```

---

# マイグレーションの実行

```bash
$ ./vendor/bin/sail artisan migrate

   INFO  Running migrations.

  0001_01_01_000000_create_users_table . 76.49ms DONE
  0001_01_01_000001_create_cache_table . 21.24ms DONE
  0001_01_01_000002_create_jobs_table .. 50.65ms DONE
```

マイグレーションが適用されたことを確認する

```bash
$ ./vendor/bin/sail artisan migrate:status

  Migration name ....................... Batch / Status
  0001_01_01_000000_create_users_table ........ [1] Ran
  0001_01_01_000001_create_cache_table ........ [1] Ran
  0001_01_01_000002_create_jobs_table ......... [1] Ran
```

---

# コンテナの起動確認

```bash
$ ./vendor/bin/sail ps
NAME                      IMAGE                          COMMAND                  SERVICE        CREATED         STATUS                   PORTS
example-app-laravel.test-1   sail-8.3/app                   "start-container"        laravel.test   3 minutes ago   Up 3 minutes             0.0.0.0:80->80/tcp, :::80->80/tcp, 0.0.0.0:5173->5173/tcp, :::5173->5173/tcp
example-app-mailpit-1        axllent/mailpit:latest         "/mailpit"               mailpit        3 minutes ago   Up 3 minutes (healthy)   0.0.0.0:1025->1025/tcp, :::1025->1025/tcp, 0.0.0.0:8025->8025/tcp, :::8025->8025/tcp, 1110/tcp
example-app-meilisearch-1    getmeili/meilisearch:latest    "tini -- /bin/sh -c …"   meilisearch    3 minutes ago   Up 3 minutes (healthy)   0.0.0.0:7700->7700/tcp, :::7700->7700/tcp
example-app-mysql-1          mysql/mysql-server:8.0         "/entrypoint.sh mysq…"   mysql          3 minutes ago   Up 3 minutes (healthy)   0.0.0.0:3306->3306/tcp, :::3306->3306/tcp, 33060-33061/tcp
example-app-redis-1          redis:alpine                   "docker-entrypoint.s…"   redis          3 minutes ago   Up 3 minutes (healthy)   0.0.0.0:6379->6379/tcp, :::6379->6379/tcp
example-app-selenium-1       selenium/standalone-chromium   "/opt/bin/entry_poin…"   selenium       3 minutes ago   Up 3 minutes             4444/tcp, 5900/tcp
```

---

# 補足: Sailコマンド

Sailコマンドの実態は約600行程のBashスクリプトになっている
`vendor/laravel/sail/bin/sail` に実装がある

https://github.com/laravel/sail/blob/1.x/bin/sail

---

# シェルエイリアスの設定

```
$ echo $SHELL
/bin/zsh # または /bin/bash
```

`~/.zshrc` または `~/.bash_profile` に以下のエイリアス設定を追記する

```
alias sail='[ -f sail ] && bash sail || bash vendor/bin/sail'
```

- `[ -f sail ]` 現在のディレクトリに sail ファイルが存在するか
  - 存在する場合 => bash sail
  - 存在しない場合 => vendor/bin/sail

---

# シェルエイリアスの反映

好きな方法でシェルエイリアス設定を読み込む

```
$ exec $SHELL -l

# or
$ source ~/.zshrc

# or
$ . ~/.zshrc

# or
Command + w
```

---

# 補足: Sailコマンド一覧

```
$ sail help
Laravel Sail

Usage:
  sail COMMAND [options] [arguments]

Unknown commands are passed to the docker-compose binary.

docker-compose Commands:
  sail up        Start the application
  sail up -d     Start the application in the background
  sail stop      Stop the application
  sail restart   Restart the application
  sail ps        Display the status of all containers

Artisan Commands:
  sail artisan ...          Run an Artisan command
  sail artisan queue:work

...
```

---

# 補足: バージョン確認

```
$ sail php -v
PHP 8.3.13 (cli) (built: Oct 30 2024 11:27:41) (NTS)
Copyright (c) The PHP Group
Zend Engine v4.3.13, Copyright (c) Zend Technologies
    with Zend OPcache v8.3.13, Copyright (c), by Zend Technologies
    with Xdebug v3.3.2, Copyright (c) 2002-2024, by Derick Rethans
$ sail artisan -v
Laravel Framework 11.30.0
$ sail composer -V
Composer version 2.7.9 2024-09-04 14:43:28
PHP version 8.3.11 (/usr/bin/php8.3)
$ sail node -v
v20.18.0
$ sail npm -v
10.9.0
$ sail yarn -v
1.22.22
$ sail pnpm -v
9.12.3
$ sail bun -v
1.1.33
```

---

# Composerパッケージのインストール

```
$ sail composer install
```

- ComposerはPHPのパッケージ管理ツール
- `composer.lock` を元にパッケージを `vendor` ディレクトリにインストール

---

# Nodeパッケージのインストール

```
$ sail npm install
```

- npm はNode.jsのパッケージ管理ツール
- `package.json` を元にパッケージを `node_modules` ディレクトリにインストール
- `package-lock.json` が生成される

---

# データベースマイグレーションの実行

```
$ sail artisan migrate
```

- `database/migrations/*.php` を元にSQLを発行してテーブル作成

---

# データベースマイグレーションの確認

```
$ sail artisan migrate:status

  Migration name ....................... Batch / Status
  0001_01_01_000000_create_users_table ........ [1] Ran
  0001_01_01_000001_create_cache_table ........ [1] Ran
  0001_01_01_000002_create_jobs_table ......... [1] Ran
```

- `migrations` テーブルの中身と適用されていない `database/migrations/*.php` の中身を表示

---

# データベース接続確認 1/2

```
$ sail mysql

mysql> show tables;
+-----------------------+
| Tables_in_laravel     |
+-----------------------+
| cache                 |
| cache_locks           |
| failed_jobs           |
| job_batches           |
| jobs                  |
| migrations            |
| password_reset_tokens |
| sessions              |
| users                 |
+-----------------------+
9 rows in set (0.00 sec)
```

---

# データベース接続確認 2/2

```
mysql> desc users;
+-------------------+-----------------+------+-----+---------+----------------+
| Field             | Type            | Null | Key | Default | Extra          |
+-------------------+-----------------+------+-----+---------+----------------+
| id                | bigint unsigned | NO   | PRI | NULL    | auto_increment |
| name              | varchar(255)    | NO   |     | NULL    |                |
| email             | varchar(255)    | NO   | UNI | NULL    |                |
| email_verified_at | timestamp       | YES  |     | NULL    |                |
| password          | varchar(255)    | NO   |     | NULL    |                |
| remember_token    | varchar(100)    | YES  |     | NULL    |                |
| created_at        | timestamp       | YES  |     | NULL    |                |
| updated_at        | timestamp       | YES  |     | NULL    |                |
+-------------------+-----------------+------+-----+---------+----------------+
8 rows in set (0.01 sec)

mysql> select * from users;
Empty set (0.00 sec)
```

---

# データベースシーディング

```
$ sail artisan db:seed

   INFO  Seeding database.
```

---

# tinker - 対話シェル

```
$ sail tinker

> User::all()
[!] Aliasing 'User' to 'App\Models\User' for this Tinker session.
= Illuminate\Database\Eloquent\Collection {#5941
    all: [
      App\Models\User {#5532
        id: 1,
        name: "Test User",
        email: "test@example.com",
        email_verified_at: "2024-11-02 11:45:22",
        #password: "$2y$12$ZKtazQqtG6cTn9N.snuwZOdjg05ZyPbO1ckybQ6Q.Yca/pALS4pIK",
        #remember_token: "LtZwmvKtW4",
        created_at: "2024-11-02 11:45:22",
        updated_at: "2024-11-02 11:45:22",
      },
    ],
  }
```

---

# Pint - コードフォーマッター

```
$ sail pint --test
$ sail pint
```

---

# テスト

```
$ sail test

   PASS  Tests\Unit\ExampleTest
  ✓ that true is true

   PASS  Tests\Feature\ExampleTest
  ✓ the application returns a successful response                                                                                                                                0.08s

  Tests:    2 passed (2 assertions)
  Duration: 0.13s
```

- PHPUnit: PHP用の単体テストを行うテストフレームワーク
- [Pest](https://pestphp.com)
  - 新しいテストフレームワーク
  - 

---

# コンテナシェルにログイン

```
# sailユーザーでログイン
$ sail shell

# rootユーザーでログイン
$ sail root-shell
```

コンテナの好きなコマンドを実行できる
rootユーザーで入ることはほぼない(一時的にライブラリをインストールしてみたいとか)

---

# その他

## より詳細に書いたのでよかったら読んで！

[【初心者向け】Laravel Sail でサクッとローカル開発環境を構築する](https://qiita.com/ucan-lab/items/0ef01c94e7e07a061311)

## LaravelでCIさせたくなったら読んで！

[Laravel Sail と GitHub Actions で効率的なビルド環境を手に入れろ！](https://qiita.com/ucan-lab/items/6bd242f0c7c934d7239a)

---

# おしまい

お疲れ様でした🍵
