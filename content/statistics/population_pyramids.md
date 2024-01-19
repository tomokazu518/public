---
title: "人口ピラミッドの推移"
weight: 20
---

## はじめに

人口プラミッドの推移を描く。データは国立社会保障・人口問題研究所のホームページから取得（1965年から2065年の予測まで，Excelファイルで提供されている）。

- 人口ピラミッドの作り方については，[人口ピラミッドの作成(2020年国勢調査)]({{< ref "census.md" >}})を参照。
- 複数年の人口ピラミッドのグラフをきれいに並べるために，patchworkパッケージを使う。 

## 結果

{{< figure src="../pyramids.png" alt="Population pyramids" >}}

## Rのコード

{{< gist tomokazu518 c4cd5a6808154ba398ff1a1eab209cb7 population_pyramids.R >}}
