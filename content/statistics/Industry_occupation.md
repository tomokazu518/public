---
title: "産業構造・職業構成の変化"
weight: 40
---


## はじめに

国勢調査から，産業構造(就業者の産業別割合)および就業者の職業構成の変化をグラフ化する。

日本標準産業分類の変更に伴い，国勢調査の産業分類が2000年以前と2005年以降で異なるため，データは完全には接続できない。ただし，1995年と2000年については新旧両方の分類のデータが公開されている。ここでは，新旧の分類を統合せず，それぞれについてグラフを作成する。

また，2009年の日本標準職業分類の変更に伴い，データはその前後で完全には接続できない。ただし，1995～2005年については新旧両方の分類のデータが公開されている。ここでは，新旧の分類を統合せず，それぞれについてグラフを作成する。

**このサービスは、政府統計総合窓口(e-Stat)のAPI機能を使用していますが、サービスの内容は国によって保証されたものではありません。**

## 結果

### 産業構造

{{< figure src="../industry_old.png" alt="Industry structure until 2000" >}}

{{< figure src="../industry_latest.png" alt="Industry structure since 1995" >}}

### 職業構成

{{< figure src="../occupation_old.png" alt="Industry structure until 2005" >}}

{{< figure src="../occupation_latest.png" alt="Industry structure since 1995" >}}

## Rのコード

新旧分類を統合して1つのグラフにしたものを作成するコードもgithub gistには置いてある。

{{< gist tomokazu518 c4cd5a6808154ba398ff1a1eab209cb7 industry_occupation_simple.R >}}
