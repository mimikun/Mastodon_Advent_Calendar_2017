# 全くの素人がMastodonインスタンスを立ててみたお話

実際に完成したものは[mimikun丼](https://mstdn.mimikun.jp)になります。

## 環境など
+ macOS 10.13
+ ConoHa VPS 1GB (Ubuntu16.04)
+ Amazon S3
+ Mailgun
+ スタードメイン

## 手順

### 1. ドメインの設定
インスタンスを立てるにあたってドメインが必要なのですが、持っていなかったので取得しました。
whois情報が出ないところが良かったのでスタードメインで取得しました。

### 2. DNSレコードの設定
スタードメインの場合、自分には分かりにくかったのでちょっと詰まりました。
(お名前やムームーはもう少し分かりやすいんでしょうか？)

### 3. VPSへのSSHログイン設定
ここで1〜2日かかってしまいました。
ファイルのパーミッション周りの設定が分かってなかったことが原因でした。
最終的にはこんな感じになりました。

1. ホスト(Mac)の`~/.ssh/config`ファイルを以下のようにして簡単に接続できるようにする

```
#ConoHa
Host conoha.hogefuga
  HostName 123.456.789.012
  User hogefuga
  Port 22
  IdentityFile ~/.ssh/id_rsa
```

2. パーミッションを適切に設定する
僕の場合はユーザをrootの他にも作ったのでそのユーザのssh鍵に対しても行いました。

```
$ sudo chmod -R 700 ~/.ssh/
$ sudo chmod 600 ~/.ssh/authorized_keys
```

### 4. VPS(Ubuntu)の更新
いつものようにコマンドを入力します。

```
$ sudo apt-get update
```

言語設定も日本語にしておきましょう。僕は後から行ったので英語が読めず、エラーメッセージをその都度Google翻訳にかけるなど無駄な時間を過ごしてしまいました。

```
$ sudo apt-get install language-pack-ja
$ sudo locale-gen ja_JP.UTF-8
```

エラー`sudo: unable to resolve host 123-456-789-012` が起きないようにする

`/etc/hosts` に以下のように追記します。

```
127.0.0.1	localhost     # 元からあった
127.0.1.1	ubuntu        # 元からあった
127.0.0.1 123-456-789-012    # 追記した
```

