---
title: "学歴別賃金プロファイル(令和3年度賃金構造基本統計調査)"
weight: 70
---

## はじめに

令和3年賃金構造基本統計調査の標準労働者のデータを使って，学歴（高校，大学，大学院）別の賃金プロファイルをグラフにする(男性と女性それぞれについて作成する)。また，大学卒労働者について企業規模別の賃金プロファイルをグラフにする。

## 結果

### 学歴別賃金プロファイル

{{< figure src="../wage_profile.png" alt="Wage profile by education" >}}


### 大学卒労働者の企業規模別賃金プロファイル

{{< figure src="../wage_profile_fs.png" alt="Wage profile of university graduates" >}}

## Rのコード

<script src="https://gist.github.com/tomokazu518/7d94dc7ab21ef7998a45b425f1651035.js?file=wage_census.R"></script>
