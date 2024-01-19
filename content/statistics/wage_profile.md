---
title: "学歴別賃金プロファイル(令和3年度賃金構造基本統計調査)"
weight: 70
---

## はじめに

令和3年賃金構造基本統計調査の標準労働者のデータを使って，学歴（高校，大学，大学院）別の賃金プロファイルをグラフにする(男性と女性それぞれについて作成する)。また，大学卒労働者について企業規模別の賃金プロファイルをグラフにする。

## 結果

### 学歴別賃金プロファイル

{{< figure src="../wage_profile_by_education.svg" alt="Wage profile by education" >}}


### 大学卒労働者の企業規模別賃金プロファイル

{{< figure src="../wage_profile_by_firmsize.svg" alt="Wage profile of university graduates" >}}

## Rのコード

{{< gist tomokazu518 c4cd5a6808154ba398ff1a1eab209cb7 wage_census.R >}}
