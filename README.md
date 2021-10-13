# Narou.rb Docker Image for Raspbeery Pi

Narou.rb を Raspbeery Pi 上の Docker で実行するための Docker Image です。<br>
Docker さえあれば一切環境構築なしで Narou.rb WEB UI を立ち上げることができます。<br>
主に、Raspbeery Pi 上で起動したままにすることを想定しています。

イメージ内容は下記で構成されます。

- Debine
- Ruby 2.7
- [改造版AozoraEpub3](https://github.com/kyukyunyorituryo/AozoraEpub3
- kindlegen
  `kindlegen`がx86のみ対応なので`ebook-convert`のラッパーを使う。<br>
  - [Raspberry PI でnarou.rbのkindle(mobi)形式を作れるようにする - Qiita](https://qiita.com/hirohiro77/items/13ef7354042967e352c4)

- crontab 毎日1時に`narou u`を実行
  - [Docker で Cron を設定しようとしたときにハマったこと - Qiita](https://qiita.com/yokra9/items/f527a7892434d2464886)
  - [Raspberry Pi + Narou.rbで小説家になろう定期ダウンローダーを作る](https://boxes-stacked.blogspot.com/2016/04/raspberry-pi-narourb.html)


## Build Docker Image on Mac

```sh
docker buildx build -t my/narou-rpi  --platform linux/arm/v7 .
```

cf. [Raspberry PiでDockerを動かす - Qiita](https://qiita.com/koduki/items/0ed303dac5d32646194f)

## Run Docker Compose
`docker-compose.yml`内の`puid`、`pgid`、`/path/to/narou`を適切な値に置換してください。

```sh
docker-compose up
```

自動的に WEB UI が起動します。<br>
http://localhost:33000/ にアクセスしてください。

## See Also
- [whiteleaf7/narou-docker: Narou.rb Dockerfile](https://github.com/whiteleaf7/narou-docker)

おまけ<br>
[dip](https://github.com/bibendi/dip) を使うと便利です

dip.yml として下記を用意して、
```yml
version: "4"
interaction:
  narou:
    description: Run narou command
    service: app
    command: narou
```

```sh
$ dip narou list

# docker-compose up と同じ
$ dip up

# 下記を実行すると、narou コマンドを透過的に実行出来る様になる
$ eval "$(dip console)"
$ narou list
```

