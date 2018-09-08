# SSH (Secure SHell)

## ssh-agentを使う

毎回パスフレーズ打ったりするのが面倒なので使っておくとよい。

```console
$ eval $(ssh-agent)
$ ssh-add ~/.id_rsa
```
