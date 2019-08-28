# raspi nginx

```shell
openssl req -x509 -nodes -new -keyout server.key -out server.crt -days 365
```

## basic認証
```shell
sudo echo "ユーザー名:$(openssl passwd -apr1 パスワード)" > basic
```
