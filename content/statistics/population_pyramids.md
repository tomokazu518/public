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

<script src="https://gist.github.com/tomokazu518/7d94dc7ab21ef7998a45b425f1651035.js?file=population_pyramids.R"></script>