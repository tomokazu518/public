---
title: "コロプレス図(Covid-19累積死亡者数)"
weight: 90
---

## はじめに

国土交通省の国土数値情報を用いて，都道府県単位のコロプレス図を描画するための地図を作成する。

まず，三重大学の奥村先生のホームページを参考に，コロプレス図用のgeojsonファイルを作成する。国土数値情報のシェープファイルはサイズが大きいので，都道府県単位に集約し，さらにデータ量を1000分の1に減らす。無理に手元でやらなくても奥村先生が完成したgeojsonファイルを公開してくれているので，それをダウンロードして使っても良い。

geojsonファイルに都道府県単位のデータをマージすることで，コロプレス図が作成できる。ここでは，例として厚労省のオープンデータから都道府県別のCovid-19の累積死亡者数を入手し，コロプレス図を作成する (厚労省のオープンデータは新型コロナの5類移行で更新終了)。

## 結果

### 2022/05/08までの都道府県別Covid-19累積死亡者数

{{< figure src="../covid-19_deaths.png" alt="covid-19_deaths" >}}

## Rのコード

### geojsonファイルの作成

<script src="https://gist.github.com/tomokazu518/c4cd5a6808154ba398ff1a1eab209cb7.js?file=choropleth.R"></script>

### 都道府県別Covid-19累積死亡者数(2023/5/8まで)

<script src="https://gist.github.com/tomokazu518/c4cd5a6808154ba398ff1a1eab209cb7.js?file=covid19.R"></script>