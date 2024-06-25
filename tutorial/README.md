# tutorial

## 概要

Metrics Design Automationが公開しているチュートリアルデータをDSimで動かすための環境

- DSimのチュートリアル：https://help.metrics.ca/support/solutions/articles/154000141163
- 試した時に書いた記事：https://www.tech-diningyo.info/entry/2024/06/23/174800

## チュートリアルデータの準備

```bash
make get_tutorial
```

## DSimの設定

以下はDSim Desktopのインストール先を動かしていない場合の設定

```bash
$ source ${HOME}/metrics-ca/dsim/<tool_version>/shell_activate.bash
$ export DSIM_LICENSE=<ライセンスファイルのパス>
```

DSim Desktopのインストール手順に従ってライセンスファイルをインストールした場合、インストール先は`$HOME/metrics-ca/dsim-license.json`になる

## 1-pathのシミュレーション

```bash
make sim_1path
```

## 2-pathのシミュレーション

```bash
make sim_2path
```

## 3-pathのシミュレーション

```bash
make sim_3path
```