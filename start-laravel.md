# homestead 環境設定

## プロジェクトの新規作成

### laravel application の作成
ディレクトリを移動
```shell
~$ cd myapp
```
To install Homestead directly into your project, require it using Composer:
```shell
~/myapp$ composer require laravel/homestead --dev
```
Homestead.yml を作成する
```shell
~/myapp$ composer install
Loading composer repositories with package information
Installing dependencies (including require-dev) from lock file
Package operations: 78 installs, 0 updates, 0 removals
~

~/myapp$ vendor/bin/homestead make
Homestead Installed!
```

### IPの設定
virtual machine で使用されている、ip を確認。
```shell
~/myapp$ cat /etc/hosts
##
# Host Database
#
# localhost is used to configure the loopback interface
# when the system is booting.  Do not change this entry.
##
127.0.0.1	localhost
192.168.10.10  hoge.test
~
```
Homestead.yml の ip を他の virtual machine と被らないように変更。
```shell
~/myapp$ vim Homestead.yml
```

```yaml:Homestead.yml
-ip: 192.168.10.10
+ip: 192.168.10.11
memory: 2048
cpus: 1
provider: virtualbox
authorize: ~/.ssh/id_rsa.pub
keys:
~
```
hosts ファイルに、ホスト名と IP アドレスを追加
```shell
~/myapp$ sudo vim /etc/host
```
```:/etc/host
+192.168.10.11   laravel-project.test
```
### DB接続 (SQL クライアントで接続)
Over SSH
- Host : 127.0.0.1:3306(Host)
- User : homestead
- Password : secret (デフォルト)
- Server : 192:168:10.11(Guest)
- Port : 22(Guest)
- User : vagrant
- Password : vagrant

### Vagrant 内環境設定

```shell
~/myapp$ vagrant up

~/myapp$ vagrant ssh
~
Thanks for using
 _                               _                 _
| |                             | |               | |
| |__   ___  _ __ ___   ___  ___| |_ ___  __ _  __| |
| '_ \ / _ \| '_ ` _ \ / _ \/ __| __/ _ \/ _` |/ _` |
| | | | (_) | | | | | |  __/\__ \ ||  __/ (_| | (_| |
|_| |_|\___/|_| |_| |_|\___||___/\__\___|\__,_|\__,_|

~$ cd code

~/code$ cp .env.example .env

~/code$ php artisan key:generate
```

.env ファイルに、host 情報を追記
```shell
~/myapp$ vim .env
```
```.env
-APP_URL=http://localhost
+APP_URL=http://project.test
```

---

## homestead が入ったプロジェクト をclone するとき
### laravel application のclone
```shell
~$ git clone https://github.com/hoge/huga.git

~$ cd huga
```
Homestead.yml を作成する
```shell
~/huga$ composer install
Loading composer repositories with package information
Installing dependencies (including require-dev) from lock file
Package operations: 78 installs, 0 updates, 0 removals
~

~/huga$ vendor/bin/homestead make
Homestead Installed!

```
以下「IPの設定」に続く