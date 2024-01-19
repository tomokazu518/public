---
title: "労働力率の推移"
weight: 50
---

## はじめに

労働力調査を用いて，労働力人口の推移，年齢階級別労働力率・失業率の推移，労働供給のM字カーブのグラフを作成する。

## 結果

### 労働力人口の推移

{{< figure src="../population_laborforce.svg" alt="Labor force" >}}

### 年齢階級別労働力率の推移

{{< figure src="../labor_participation_by_age.svg" alt="Participation rates" >}}

### 年齢階級別失業率の推移

{{< figure src="../unemployment_by_age.svg" alt="Unemployment" >}}

### M字カーブ

{{< figure src="../m_curve.svg" alt="M-shape curve" >}}

## Rのコード

{{< gist tomokazu518 c4cd5a6808154ba398ff1a1eab209cb7 labor_force.R >}}