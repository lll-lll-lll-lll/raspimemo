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
