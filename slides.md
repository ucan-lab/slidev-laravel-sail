---
# You can also start simply with 'default'
theme: seriph
# random image from a curated Unsplash collection by Anthony
# like them? see https://unsplash.com/collections/94734566/slidev
background: https://cover.sli.dev
# some information about your slides (markdown enabled)
title: Welcome to Slidev
info: |
  ## Slidev Starter Template
  Presentation slides for developers.

  Learn more at [Sli.dev](https://sli.dev)
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
---

# What is Laravel?

---

# What is Laravel Sail?

- Laravel公式提供の開発環境
- 内部的にDocker Composeを利用
- Sailコマンドを経由してLaravelに関するコマンドを実行
- Dockerの経験を必要とせずLaravel開発が行えることがコンセプトのツール

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

---

# 準備3

今回使用するハンズオン用ディレクトリを作成します。

```
$ mkdir ~/sail-handson
$ cd ~/sail-handson
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

## 補足: Sailサービスの選択

```bash
$ curl -s "https://laravel.build/example-app?with=mysql,redis" | bash
```

withというクエリ文字列変数を使って、設定するサービスを選択できます。

- 利用可能なサービス: `mysql`, `pgsql`, `mariadb`, `redis`, `memcached`, `meilisearch`, `typesence`, `minio`, `selenium`, `mailpit`

# Docker イメージビルド&コンテナ作成

```bash
$ cd example-app
$ ./vendor/bin/sail up -d
```

`-d` を付けるとデタッチドモード(バックグラウンド)で実行します。
付けない場合はフォアグラウンドになり、ターミナルが占有されるので、別ターミナルでコマンドを打つ必要があります。

---

# localhost で接続が拒否されました。

http://localhost へアクセスして次のエラーが出た場合は、他プロセスが 80 ポートをすでに利用している場合があります。

心当たりがあればそのサービスを停止してください。
不明な場合は次のコマンドからそのポートを利用しているプロセスを調べられます。

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

# 動作確認

http://localhost

シェルを再起動する

Sailファイルをカスタマイズしたい場合

https://github.com/laravel/sail/blob/1.x/bin/sail

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

PHP Commands:
  sail php ...   Run a snippet of PHP code
  sail php -v

Composer Commands:
  sail composer ...                       Run a Composer command
  sail composer require laravel/sanctum

Node Commands:
  sail node ...         Run a Node command
  sail node --version

NPM Commands:
  sail npm ...        Run a npm command
  sail npx            Run a npx command
  sail npm run prod

PNPM Commands:
  sail pnpm ...        Run a pnpm command
  sail pnpx            Run a pnpx command
  sail pnpm run prod

Yarn Commands:
  sail yarn ...        Run a Yarn command
  sail yarn run prod

Bun Commands:
  sail bun ...        Run a bun command
  sail bunx           Run a bunx command
  sail bun run prod

Database Commands:
  sail mysql     Start a MySQL CLI session within the 'mysql' container
  sail mariadb   Start a MySQL CLI session within the 'mariadb' container
  sail psql      Start a PostgreSQL CLI session within the 'pgsql' container
  sail redis     Start a Redis CLI session within the 'redis' container

Debugging:
  sail debug ...          Run an Artisan command in debug mode
  sail debug queue:work

Running Tests:
  sail test          Run the PHPUnit tests via the Artisan test command
  sail phpunit ...   Run PHPUnit
  sail pest ...      Run Pest
  sail pint ...      Run Pint
  sail dusk          Run the Dusk tests (Requires the laravel/dusk package)
  sail dusk:fails    Re-run previously failed Dusk tests (Requires the laravel/dusk package)

Container CLI:
  sail shell        Start a shell session within the application container
  sail bash         Alias for 'sail shell'
  sail root-shell   Start a root shell session within the application container
  sail root-bash    Alias for 'sail root-shell'
  sail tinker       Start a new Laravel Tinker session

Sharing:
  sail share   Share the application publicly via a temporary URL
  sail open    Open the site in your browser

Binaries:
  sail bin ...   Run Composer binary scripts from the vendor/bin directory

Customization:
  sail artisan sail:publish   Publish the Sail configuration files
  sail build --no-cache       Rebuild all of the Sail containers
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

- `composer.lock` を元にパッケージを `vendor` ディレクトリにインストール

---

# Nodeパッケージのインストール

```
$ sail npm install
```

- `package.json` を元にパッケージを `node_modules` ディレクトリにインストール
- `package-lock.json` が生成される

---

# データベースマイグレーションの実行

```
$ sail artisan migrate
```

---

# データベースマイグレーションの確認

```
$ sail artisan migrate:status

  Migration name ....................... Batch / Status
  0001_01_01_000000_create_users_table ........ [1] Ran
  0001_01_01_000001_create_cache_table ........ [1] Ran
  0001_01_01_000002_create_jobs_table ......... [1] Ran
```

---

# データベース接続

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

---

# tips: コンテナシェル

```
# sailユーザーでログイン
$ sail shell

# rootユーザーでログイン
$ sail root-shell
```
