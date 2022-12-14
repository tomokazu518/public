---
title: "経済成長率と寄与度のグラフ作成(四半期GDP速報)"
weight: 80
---

## はじめに

国民経済計算(GDP統計)の四半期GDP速報を使って，経済成長率の折れ線グラフと寄与度の積上げ棒グラフを重ねたものを作成する。データはe-statのAPIで入手する。

e-statでは，四半期GDP速報は1994年からデータを入手可能だが，1994年から現在までを1枚のグラフにすると見にくいので，期間を指定してグラフを作成する。ここでは，2014年以降のグラフを描いた。

できあがるのは新聞などでもよく見るグラフ。


**このサービスは、政府統計総合窓口(e-Stat)のAPI機能を使用していますが、サービスの内容は国によって保証されたものではありません。**

## 結果

{{< figure src="../quarterly_gdp.png" alt="Economic Growth" >}}


## Rのコード

<script src="https://gist.github.com/tomokazu518/7d94dc7ab21ef7998a45b425f1651035.js?file=quarterly_gdp.R"></script>