# raspimemo


ubuntuにDockerを入れた後に `docker ps`などを実行しようとする権限でエラーになる。
なので、ユーザをDockerグループに入れてセッションを切って再度ログインし直す
```sh
sudo usermod -aG docker $USER
newgrp docker
docker ps
```
これで解決しない場合は
dockerが正常に動くか
```sh
sudo systemctl status docker
sudo systemctl start docker
```
　もしくはdocker.sockののパーミッションに問題あり
 ```sh
ls -l /var/run/docker.sock
正常

srw-rw---- 1 root docker 0 Jul 21 12:34 /var/run/docker.sock

sudo chown root:docker /var/run/docker.sock
sudo chmod 660 /var/run/docker.sock

```


postgresqlを入れる
```sh
sudo apt update
sudo apt install postgresql postgresql-contrib
```

ローカルユーザに入る
```sh
 sudo -u postgres psql
パスワード変更
ALTER ROLE postgres WITH password 'hogehoge';
```


ポートを設定する
`/etc/postgresql/16/main/postgresql.conf`を編集してTCP5432ポートを開く


pg_hba.conf を書き換えて認証を受ける。
/etc/postgresql/16/main/pg_hba.confに以下を書き換える
以下を追加してip/24 と cidrのネットワークから パスワード認証での接続が可能となる
```sh
host	all		all		192.168.42.0/24		md5

sudo systemctl restart postgresql.service
```
