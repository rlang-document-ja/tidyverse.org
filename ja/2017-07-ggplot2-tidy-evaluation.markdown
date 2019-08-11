---
title: ggplot2 3.0.0で整然とした評価を使用します。
date: '2018-07-24'
slug: ggplot2-tidy-evaluation
author: Mara Averick
categories:
- package
description: ggplot2 3.0.0における整然とした評価をする
photo:
  url: https://unsplash.com/photos/8KfCR12oeUM
  author: Christopher Burns
---

<style>
h2 code {
    font-size: 1em;
</style>

}



[ggplot2 3.0.0](https://ggplot2.tidyverse.org/)の最大の変更点の1つは、 [整然とした評価](https://adv-r.hadley.nz/evaluation.html#tidy-evaluation)のサポートです。よりプログラマブルにし、他のtidyverse との一貫性が高まります。これによりいくつかの重大な変更が発生しますが、将来的にコードをよりよくすることに貢献すると考えています。ここでは、使用例をいくつか示します。新しい開発者向けの変更についても説明します（ [以下を参照](#developer-facing-changes)）。

## `aes()`の整然とした美学

[疑似クオーテーション](https://adv-r.hadley.nz/quasiquotation.html)が、[`aes()`](https://ggplot2.tidyverse.org/reference/aes.html), [`facet_wrap()`](https://ggplot2.tidyverse.org/reference/facet_wrap.html)や[`facet_grid()`](https://ggplot2.tidyverse.org/reference/facet_grid.html)で使えます。`aes()`場合、 疑似クオーテーション(`!!`, `!!!`, `:=`)は、[`aes_()`](https://ggplot2.tidyverse.org/reference/aes_.html)や[`aec_string()`](https://ggplot2.tidyverse.org/reference/aes_.html)に置き換えます。(これらの関数はしばらくの間は使用することはできますが、非推奨です。)

円グラフを作るための関数に疑似クオーテーションを利用します。疑似クオーテーションは関数に引数を渡すときにクオートを使う必要がなくなります。[^1] 以下の初期関数はユーザが`aes()`の引数を正確に知る必要があります。

```r
piechart_basic <- function(data, mapping) {
  ggplot(data, mapping) +
    geom_bar(width = 1) +
    coord_polar(theta = "y") +
    xlab(NULL) +
    ylab(NULL)
}
piechart_basic(mpg, aes(factor(1), fill = class))
```

<img src="/articles/2017-07-ggplot2-tidy-evaluation_files/figure-html/piechart-basic-1.png" width="700px" style="display: block; margin: auto;">

別の関数の中から整然として評価関数を呼び出すために重要なことは、`enquo()`でクオートし、`!!`でアンクオートします。

```r
pie_chart <- function(data, var, ...) {
  var <- enquo(var)
  piechart_basic(data, aes(factor(1), fill = !!var))
}
pie_chart(mpg, class)
```

<img src="/articles/2017-07-ggplot2-tidy-evaluation_files/figure-html/piechart-qq-1.png" width="700px" style="display: block; margin: auto;">

散布図を描くのも同様です。

```r
scatter_plot <- function(data, x, y) {
  x <- enquo(x)
  y <- enquo(y)

  ggplot(data) + geom_point(aes(!!x, !!y))
}
scatter_plot(mtcars, disp, drat)
```

<img src="/articles/2017-07-ggplot2-tidy-evaluation_files/figure-html/scatter-by-1.png" width="700px" style="display: block; margin: auto;">

## `vars()`による整然としたfacet

facet処理での疑似クオーテーションをサポートするために、ヘルパー関数[`vars()`](https://ggplot2.tidyverse.org/reference/vars.html)があり、変数の省略ができます。`facet_grid(x + y ~ a + b)`の代わりに、`facet_grid(vars(x, y), vars(a, b))`と書くことができます。式インターフェースはなくなりませんが、新しい`vars()`インターフェースは整然とした評価をサポートし、簡単にプログラミングできます。

`vars()`は変数や式を提供するのに使われ、facetグループによるデータセットが評価されます。

```r
p <- ggplot(mpg, aes(displ, cty)) + geom_point()

p + facet_grid(rows = vars(drv))
```

<img src="/articles/2017-07-ggplot2-tidy-evaluation_files/figure-html/facet-vars-1.png" width="218px" style="display: block; margin: auto;">

`aes()`と比較して、`vars()`は名前なしの引数を受け取ります。unquote-splice operatorな`!!!`をより自然に使えるようになります。

```r
year <- 2018

d <- mpg %>%
  filter(manufacturer %in% c("dodge", "ford")) %>%
  ggplot() +
    geom_point(aes(displ, cty))

args <- quos(year, manufacturer)

d + facet_grid(vars(!!!args))
```

<img src="/articles/2017-07-ggplot2-tidy-evaluation_files/figure-html/vars-env-1.png" width="177px" style="display: block; margin: auto;">

`vars()`の内部で、facetに軸名を追加する名前を簡単に指定できます。

```r
p + facet_grid(vars(Cylinder = cyl), labeller = label_both)
```

<img src="/articles/2017-07-ggplot2-tidy-evaluation_files/figure-html/labelled-grid-1.png" width="177px" style="display: block; margin: auto;">

`vars()`はラッパー関数に変数を簡単に渡すことができます。

```r
p <- ggplot(mtcars, aes(wt, disp)) + geom_point()

wrap_by <- function(...) {
  facet_wrap(vars(...), labeller = label_both)
}

p + wrap_by(vs, am)
```

<img src="/articles/2017-07-ggplot2-tidy-evaluation_files/figure-html/wrap-by-1.png" width="700px" style="display: block; margin: auto;">

上記の`wrap_by()`では、整然としたドット([`...`](https://adv-r.hadley.nz/quasiquotation.html#dot-dot-dot-...))を使用しました。このドットは任意の数を引数を表しています。もしくは、[`enquo()`](http://rlang.r-lib.org/reference/quotation.html)で単一の名前付き引数を使えます。デフォルトの名前を作るために[`quo_name()`](http://rlang.r-lib.org/reference/quo_label.html)を使い、quosureから単純な文字列に変換します。次に、[`!!`](http://rlang.r-lib.org/reference/quasiquotation.html)(bang bang参照)やアンクオートするための`:=`を使用して適切なコンテキストで引数をアンクオートして評価します。

```r
wrap_cut <- function(var, n = 3) {
  var <- enquo(var)
  nm <- quo_name(var)
  wrap_by(!!nm := cut_number(!!var, !!n))
}

p + wrap_cut(drat)
```

<img src="/articles/2017-07-ggplot2-tidy-evaluation_files/figure-html/wrap-cut-1.png" width="700px" style="display: block; margin: auto;">

既存のggplot2オブジェクトのマッピングを計算する場合は、 [rlang](http://rlang.r-lib.org/)ツールを使用する必要もあります。

## 開発者向けの変更

ggplot2の上に拡張パッケージを構築する場合、整然とされた評価の導入により、 `aes()`が使用するデータ構造が根本的に変更されることに注意する必要があります。簡単に言うと：

```r
mapping <- aes(mpg, colour = "smoothed")

# Variables are now stored as quosures
mapping$x
#> <quosure>
#>   expr: ^mpg
#>   env:  global

# Constants (atomic vectors of length 1), remain
# as is
mapping$colour
#> [1] "smoothed"
```

## 助けを得る

整然とされた評価に慣れていないなら、進行中の第2版Advanced R、特に[メタプログラミング](https://adv-r.hadley.nz/meta.html)のセクションで学ぶのが最適です。また、Lionel HenryによるRStudioの[整然とした評価のウェビナー](https://www.rstudio.com/resources/webinars/tidy-eval/) 、または（忙しい人は）Hadleyのビデオ： [5分間の整然とした評価のウェビナー](https://www.youtube.com/watch?v=nERXS3ssntw)をチェックしてください。

また、 困った時は[community.rstudio.com](https://community.rstudio.com/)で質問してください。

[^1]: Wickham, Hadley. 2016. “Programming with Ggplot2.” In *Ggplot2: Elegant Graphics for Data Analysis*, 241–53. Cham: Springer International Publishing. doi:10.1007/978-3-319-24277-4_12.
