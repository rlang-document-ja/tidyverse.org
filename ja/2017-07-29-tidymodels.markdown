---
title: tidymodelsパッケージ
author: マックスクーン
date: '2018-08-06'
slug: tidymodels-0-0-1
description: tidymodels 0.0.1がCRANにあります。
categories:
- パッケージ
---

`tidymodels`パッケージが[CRANになりました](http://cran.r-project.org/web/packages/tidymodels) 。その姉妹パッケージ`tidyverse`と同様に、それはモデリングと分析に関連するtidyverseパッケージをインストールしてロードするために使用することができます。現在は、 [`broom`](https://broom.tidyverse.org/) 、 [`dplyr`](http://dplyr.tidyverse.org) 、 [`ggplot2`](https://ggplot2.tidyverse.org/) 、 [`infer`](http://infer.netlify.com/) 、 [`purrr`](https://purrr.tidyverse.org/) 、 [`recipes`](https://tidymodels.github.io/recipes/) 、 [`rsample`](https://tidymodels.github.io/rsample/) 、 [`tibble`](https://tibble.tidyverse.org/) 、および[`yardstick`](https://tidymodels.github.io/yardstick/)インストールして添付しています。

```r
library(tidymodels)
#> ── Attaching packages ───────────────────────────────── tidymodels 0.0.1 ──
#> ✔ ggplot2   3.0.0     ✔ recipes   0.1.3
#> ✔ tibble    1.4.2     ✔ broom     0.5.0
#> ✔ purrr     0.2.5     ✔ yardstick 0.0.1
#> ✔ dplyr     0.7.6     ✔ infer     0.3.0
#> ✔ rsample   0.0.2
#> ── Conflicts ──────────────────────────────────── tidymodels_conflicts() ──
#> ✖ rsample::fill() masks tidyr::fill()
#> ✖ dplyr::filter() masks stats::filter()
#> ✖ dplyr::lag()    masks stats::lag()
#> ✖ recipes::step() masks stats::step()
```

`tidymodels`は*タグ付けされたパッケージの*急増するリストも含み*ます* 。これらは特定の目的のためにパッケージのセットをインストールするために使用することができます。たとえば、テキストデータを分析するための追加の整然としたツールが必要な場合は、次のようにします。

```r
tag_attach("text analysis")
#> ── Attaching packages ───────────────────────────────── tidymodels 0.0.1 ──
#> ✔ tidytext 0.1.9     ✔ keras    2.1.6
```

新しいパッケージがリリースされるたびに、これらのタグはそれぞれのバージョンの`tidymodels`で更新されます。

一方向モデリングパッケージの数は増え続けています。開発期間のいくつかのパッケージは次のとおりです。

- [`parsnip`](https://topepo.github.io/parsnip) ：モデルへの統一インターフェースこれにより、異なるパッケージ間で1つの標準化されたモデル関数を持ち、モデル間でパラメータ名を調和させることによって、記憶する必要がある構文上の細部の量を大幅に減らすことができます。

- [`dials`](https://tidymodels.github.io/dials) ：パラメータを調整するためのツール。 `dials`は、チューニングパラメータ値を作成および検証するためのオブジェクトとメソッド、およびグリッド検索ツールが含まれています。これは、 `parsnip`シームレスに連携するように設計されています。

- [`embed`](https://topepo.github.io/embed) ： `recipes`用のアドオンパッケージ。これは、尤度符号化やエンティティ埋め込みなどの教師付き方法を使用して高濃度のカテゴリカル予測子を効率的に符号化するために使用できます。

- [`modelgenerics`](https://tidymodels.github.io/modelgenerics) ：開発者向けツールこの軽量パッケージは、パッケージ間で使用されるクラス用の一連の汎用メソッドを提供することによって、パッケージの依存関係を減らすのに役立ちます。例えば、あなたがあなたのモデルのために新しい`tidy`方法を作成しているなら、このパッケージは`broom` （およびその依存関係）の代わりに使用することができます。

最新の情報については、 [Tidymodels組織ページ](https://github.com/tidymodels)を[ご覧ください](https://github.com/tidymodels) 。
