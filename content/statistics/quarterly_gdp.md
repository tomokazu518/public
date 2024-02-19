---
title: "経済成長率と寄与度のグラフ作成(四半期GDP速報)"
weight: 80
---

## はじめに

国民経済計算(GDP統計)の四半期GDP速報を使って，経済成長率の折れ線グラフと寄与度の積上げ棒グラフを重ねたものを作成する。データはe-statのAPIで入手する。

e-statでは，四半期GDP速報は1994年からデータを入手可能だが，1994年から現在までを1枚のグラフにすると見にくいので，期間を指定してグラフを作成する。ここでは，期間を3つに分けてグラフを描いた。

できあがるのは新聞などでもよく見るグラフ。

**このサービスは、政府統計総合窓口(e-Stat)のAPI機能を使用していますが、サービスの内容は国によって保証されたものではありません。**

## 結果

1994年から2003年まで
{{< figure src="../svg_files/quarterly_gdp-1.svg" alt="Economic Growth" >}}

2004年から2013年まで
{{< figure src="../svg_files/quarterly_gdp-2.svg" alt="Economic Growth" >}}

2014年以降
{{< figure src="../svg_files/quarterly_gdp-3.svg" alt="Economic Growth" >}}

## Rのコード

{{< gist tomokazu518 c4cd5a6808154ba398ff1a1eab209cb7 quarterly_gdp.R >}}
