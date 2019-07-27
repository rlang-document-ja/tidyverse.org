---
title: tidymodelsパッケージ
author: マックスクーン
date: '2018-08-06'
slug: tidymodels-0-0-1
description: tidymodels 0.0.1がCRANにあります。
categories:
- パッケージ
---

`tidymodels`パッケージをCRAN上で公開しています 。姉妹パッケージ`tidyverse`と同様に、モデリングと分析のためのtidyverseパッケージをインストールしてロードすると利用可能です。現在は、 [`broom`](https://broom.tidyverse.org/), [`dplyr`](http://dplyr.tidyverse.org), [`ggplot2`](https://ggplot2.tidyverse.org/), [`infer`](http://infer.netlify.com/), [`purrr`](https://purrr.tidyverse.org/), [`recipes`](https://tidymodels.github.io/recipes/), [`rsample`](https://tidymodels.github.io/rsample/), [`tibble`](https://tibble.tidyverse.org/), や[`yardstick`](https://tidymodels.github.io/yardstick/)が同時にインストールされます。

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

`tidymodels`はパッケージのリストを内包していて、分析の目的に応じてインストール可能なパッケージを検索できます。たとえば、テキストデータを分析するための整然としたツールが必要な場合は、次のようにします。

```r
tag_attach("text analysis")
#> ── Attaching packages ───────────────────────────────── tidymodels 0.0.1 ──
#> ✔ tidytext 0.1.9     ✔ keras    2.1.6
```

`tidymodels`のバージョンアップに応じて、パッケージのリストも更新されます。

tidyverseなモデリングパッケージは開発され続けており、その数が増えています。一方向モデリングパッケージの数は増え続けています。開発中も含めて、次のようなパッケージもあります。

- [`parsnip`](https://topepo.github.io/parsnip) ：モデルへの統一的なインターフェースを提供します。異なるパッケージであっても関数を標準化させる機能を持ち、パラメータ名も統一させることで、パッケージが持つ特有の構文を記憶する手間を大幅に減らします。

- [`dials`](https://tidymodels.github.io/dials) ：パラメータチューニングのためのツール。 `dials`は、パラメータを作成および検証することができ、グリッドサーチをする機能もあります。`parsnip`にシームレスな連携を前提に設計しています。

- [`embed`](https://topepo.github.io/embed) : recipes用のアドオンパッケージ。これは、Likelihood EncodingやEntity Embeddingのような教師あり学習を使用して、クラスの多い多クラス分類を効率的に符号化するために使用します。

- [`modelgenerics`](https://tidymodels.github.io/modelgenerics) : 開発者向けツール。この軽量パッケージは、パッケージを跨って利用される汎用的なメソッドを提供することによって、パッケージの依存関係を減らすのに役立ちます。例えば、新しい`tidy`な方法を自身のモデルのために作成しているなら、`broom`（およびその依存関係）を使うのがオススメです。

最新の情報については、 [Tidymodels organization page](https://github.com/tidymodels)をご覧ください 。
