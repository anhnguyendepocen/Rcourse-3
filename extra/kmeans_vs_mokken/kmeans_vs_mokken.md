Kmeans and Mokking Scale Analysis
================
Wim van der Ham

Here an example of the `kmeans()` function is shown. The `iris` dataset
is divided in clusters and the clusters are compared to the `Species`
column. A wrapper function is used to make it easy to try different
number of clusters.

``` r
iris_df <- as_data_frame(iris)

kmeans_for_n_clusters <- function(data, n_clusters) {
  iris_train <- data %>% 
    dplyr::select(-Species)
  
  kclust <- kmeans(iris_train, n_clusters)
  
  iris_pred <- augment(kclust, iris)
  iris_pred %>%
    count(Species, `.cluster`)
}
kmeans_for_n_clusters(iris_df, 2)
```

    ## # A tibble: 4 x 3
    ##   Species    .cluster     n
    ##   <fct>      <fct>    <int>
    ## 1 setosa     2           50
    ## 2 versicolor 1           47
    ## 3 versicolor 2            3
    ## 4 virginica  1           50

# Mokken scaling

[Mokken scaling](https://en.wikipedia.org/wiki/Mokken_scale) is part of
the [Item response
theory](https://en.wikipedia.org/wiki/Item_response_theory) and can be
used to assess whether a number of items measure the same underlying
concept. Here an example from the `mokken` package is used and the
results are compared to the kmeans method.

``` r
data(acl)

Communality <- acl[, 1:10]

scale <- aisp(Communality, verbose = FALSE)

Communality_t <- t(Communality)

kclust <- kmeans(Communality_t, 3)

scale
```

    ##                0.3
    ## reliable         1
    ## honest           1
    ## unscrupulous*    0
    ## deceitful*       1
    ## unintelligent*   0
    ## obnoxious*       2
    ## thankless*       2
    ## unfriendly*      2
    ## dependable       1
    ## cruel*           2

``` r
kclust
```

    ## K-means clustering with 3 clusters of sizes 1, 3, 6
    ## 
    ## Cluster means:
    ##       [,1]     [,2]     [,3]     [,4]     [,5]     [,6]     [,7]     [,8]
    ## 1 4.000000 3.000000 3.000000 2.000000 4.000000 2.000000 3.000000 3.000000
    ## 2 3.000000 3.333333 2.666667 2.666667 3.333333 3.333333 3.333333 2.666667
    ## 3 3.666667 3.166667 3.333333 2.500000 3.500000 3.166667 3.666667 3.666667
    ##   [,9]    [,10]    [,11]    [,12]    [,13]    [,14]    [,15]    [,16]
    ## 1  3.0 2.000000 4.000000 1.000000 4.000000 2.000000 4.000000 3.000000
    ## 2  3.0 3.333333 3.333333 3.000000 3.666667 3.333333 3.666667 2.666667
    ## 3  3.5 3.666667 3.500000 2.666667 3.833333 2.833333 3.666667 3.666667
    ##      [,17]    [,18]    [,19]    [,20]    [,21]    [,22] [,23]    [,24]
    ## 1 3.000000 3.000000 4.000000 4.000000 4.000000 3.000000     4 3.000000
    ## 2 4.000000 2.000000 3.666667 3.333333 4.000000 3.333333     3 3.000000
    ## 3 3.666667 3.333333 3.666667 3.833333 3.166667 3.666667     4 3.666667
    ##      [,25]    [,26]    [,27]    [,28]    [,29]    [,30] [,31] [,32]
    ## 1 3.000000 4.000000 3.000000 4.000000 4.000000 3.000000     3     4
    ## 2 3.333333 3.333333 3.333333 3.666667 4.000000 3.333333     3     4
    ## 3 3.833333 3.333333 3.000000 3.333333 3.333333 3.166667     4     3
    ##      [,33] [,34]    [,35]    [,36]    [,37]    [,38]    [,39]    [,40]
    ## 1 1.000000     4 2.000000 0.000000 3.000000 1.000000 3.000000 3.000000
    ## 2 3.666667     4 3.000000 3.333333 3.000000 2.666667 3.333333 2.666667
    ## 3 3.666667     4 2.833333 3.833333 2.333333 2.166667 3.000000 3.000000
    ##      [,41] [,42]    [,43]    [,44] [,45]    [,46]    [,47]    [,48]
    ## 1 2.000000     3 1.000000 4.000000   3.0 1.000000 4.000000 3.000000
    ## 2 2.333333     2 4.000000 3.333333   4.0 3.333333 3.000000 3.333333
    ## 3 3.666667     3 2.666667 3.666667   3.5 2.833333 3.833333 3.833333
    ##      [,49]    [,50]    [,51]    [,52] [,53] [,54]    [,55]    [,56] [,57]
    ## 1 3.000000 2.000000 3.000000 1.000000     3     2 3.000000 3.000000     4
    ## 2 3.666667 3.000000 3.666667 2.333333     4     3 3.000000 3.000000     3
    ## 3 3.000000 3.166667 3.500000 3.500000     3     2 2.333333 2.833333     4
    ##      [,58]    [,59]    [,60] [,61]    [,62]    [,63]    [,64] [,65]
    ## 1 2.000000 2.000000 4.000000   3.0 3.000000 3.000000 4.000000   3.0
    ## 2 2.666667 2.666667 3.333333   4.0 3.000000 3.666667 2.000000   3.0
    ## 3 3.333333 2.833333 3.333333   3.5 2.833333 3.833333 3.333333   3.5
    ##      [,66]    [,67]    [,68]    [,69]    [,70]    [,71]    [,72]    [,73]
    ## 1 3.000000 4.000000 4.000000 1.000000 3.000000 2.000000 2.000000 2.000000
    ## 2 3.666667 3.333333 3.666667 3.000000 3.000000 3.000000 2.333333 2.333333
    ## 3 3.500000 3.833333 3.500000 2.666667 3.166667 2.833333 3.000000 3.666667
    ##      [,74]    [,75]    [,76]    [,77]    [,78]    [,79]    [,80] [,81]
    ## 1 3.000000 2.000000 4.000000 4.000000 4.000000 3.000000 4.000000     3
    ## 2 3.000000 2.333333 3.333333 3.000000 3.000000 3.333333 3.333333     3
    ## 3 3.833333 2.666667 3.000000 3.666667 3.666667 2.666667 3.666667     3
    ##      [,82]    [,83]    [,84] [,85]    [,86]    [,87]    [,88] [,89]
    ## 1 4.000000 4.000000 2.000000   2.0 4.000000 4.000000 1.000000     3
    ## 2 3.333333 2.666667 3.000000   2.0 3.666667 4.000000 2.666667     3
    ## 3 4.000000 4.000000 2.666667   2.5 2.666667 3.666667 3.500000     3
    ##      [,90]    [,91]    [,92]     [,93]    [,94]    [,95] [,96]    [,97]
    ## 1 4.000000 2.000000 1.000000 1.0000000 3.000000 2.000000   3.0 4.000000
    ## 2 3.333333 2.666667 1.333333 0.6666667 3.333333 3.000000   4.0 3.666667
    ## 3 2.833333 3.000000 1.833333 2.3333333 3.166667 3.166667   3.5 2.833333
    ##      [,98]    [,99]   [,100]   [,101]   [,102]   [,103]   [,104] [,105]
    ## 1 1.000000 1.000000 1.000000 4.000000 3.000000 0.000000 4.000000      4
    ## 2 2.000000 3.666667 3.333333 2.666667 4.000000 3.333333 2.333333      4
    ## 3 2.833333 2.166667 3.833333 3.666667 3.833333 3.000000 4.000000      4
    ##   [,106]   [,107]   [,108]   [,109]   [,110]   [,111]   [,112]   [,113]
    ## 1    3.0 1.000000 2.000000 0.000000 2.000000 2.000000 3.000000 4.000000
    ## 2    3.0 4.000000 4.000000 2.000000 3.666667 3.333333 3.333333 3.333333
    ## 3    3.5 3.166667 2.833333 1.666667 3.000000 3.000000 3.333333 4.000000
    ##     [,114] [,115] [,116]   [,117] [,118] [,119]   [,120]   [,121] [,122]
    ## 1 4.000000      4    4.0 2.000000      2    1.0 1.000000 4.000000    4.0
    ## 2 2.666667      4    4.0 3.000000      2    4.0 2.666667 3.666667    4.0
    ## 3 3.500000      4    3.5 3.833333      3    3.5 2.666667 4.000000    3.5
    ##     [,123]   [,124]   [,125]   [,126]   [,127] [,128]   [,129]   [,130]
    ## 1 4.000000 2.000000 3.000000 3.000000 4.000000    4.0 3.000000 2.000000
    ## 2 3.333333 3.000000 3.000000 3.333333 3.000000    3.0 3.000000 2.000000
    ## 3 3.666667 3.833333 2.833333 2.833333 3.833333    3.5 3.833333 3.166667
    ##     [,131]   [,132]   [,133]   [,134]   [,135]   [,136]   [,137]   [,138]
    ## 1 3.000000 4.000000 4.000000 2.000000 2.000000 3.000000 0.000000 4.000000
    ## 2 2.666667 3.666667 2.666667 2.666667 2.000000 2.666667 2.666667 3.666667
    ## 3 3.166667 3.833333 3.000000 3.166667 3.166667 2.833333 3.500000 3.500000
    ##     [,139]   [,140]   [,141]   [,142]   [,143]   [,144]   [,145]   [,146]
    ## 1 4.000000 3.000000 3.000000 2.000000 3.000000 2.000000 3.000000 2.000000
    ## 2 3.000000 3.333333 4.000000 3.000000 3.000000 3.000000 3.333333 3.333333
    ## 3 3.833333 3.333333 2.666667 2.833333 3.333333 3.166667 3.666667 3.500000
    ##     [,147] [,148]   [,149]   [,150]   [,151]   [,152]   [,153]   [,154]
    ## 1 1.000000    3.0 4.000000 3.000000 1.000000 1.000000 4.000000 3.000000
    ## 2 2.666667    3.0 3.666667 2.333333 3.000000 2.666667 3.666667 3.333333
    ## 3 1.833333    3.5 3.500000 3.333333 3.166667 1.833333 3.833333 3.500000
    ##     [,155]   [,156] [,157]   [,158] [,159]   [,160]   [,161]   [,162]
    ## 1 3.000000 2.000000      4 3.000000    3.0 3.000000 3.000000 2.000000
    ## 2 3.666667 2.333333      3 4.000000    3.0 3.000000 3.000000 3.000000
    ## 3 2.833333 3.166667      4 3.666667    3.5 3.666667 3.333333 3.166667
    ##     [,163]   [,164] [,165]   [,166]   [,167]   [,168]   [,169]   [,170]
    ## 1 4.000000 2.000000    0.0 2.000000 4.000000 3.000000 4.000000 1.000000
    ## 2 2.000000 2.333333    1.0 3.000000 3.666667 3.666667 4.000000 3.666667
    ## 3 3.166667 3.166667    2.5 3.833333 3.833333 3.000000 3.166667 3.500000
    ##     [,171] [,172]   [,173]   [,174]   [,175]   [,176]   [,177]   [,178]
    ## 1 3.000000    3.0 3.000000 4.000000 3.000000 4.000000 3.000000 2.000000
    ## 2 4.000000    3.0 3.333333 4.000000 3.666667 3.666667 3.333333 3.666667
    ## 3 3.666667    3.5 1.833333 3.666667 3.333333 3.166667 3.500000 3.666667
    ##     [,179]   [,180] [,181]   [,182]   [,183]   [,184]   [,185]   [,186]
    ## 1 4.000000 4.000000      2 4.000000 4.000000 3.000000 0.000000 4.000000
    ## 2 2.666667 3.666667      3 3.666667 3.666667 2.333333 3.000000 2.333333
    ## 3 3.333333 3.833333      3 3.666667 3.333333 3.166667 3.166667 3.833333
    ##     [,187]   [,188]   [,189]   [,190]   [,191]   [,192]   [,193]   [,194]
    ## 1 1.000000 4.000000 3.000000 2.000000 4.000000 4.000000 4.000000 2.000000
    ## 2 1.666667 2.666667 3.333333 3.000000 3.333333 3.666667 2.666667 3.666667
    ## 3 2.833333 3.833333 2.833333 3.833333 3.333333 3.833333 3.333333 2.833333
    ##   [,195] [,196]   [,197] [,198]   [,199]   [,200]   [,201]   [,202]
    ## 1    4.0    1.0 4.000000      2 4.000000 3.000000 4.000000 4.000000
    ## 2    3.0    3.0 4.000000      4 3.000000 3.333333 4.000000 4.000000
    ## 3    3.5    3.5 3.166667      4 3.166667 3.500000 3.666667 3.666667
    ##     [,203]   [,204]   [,205]   [,206]   [,207]   [,208]   [,209] [,210]
    ## 1 3.000000 4.000000 4.000000 3.000000 3.000000 3.000000 2.000000      3
    ## 2 2.000000 3.666667 2.666667 3.333333 3.666667 2.666667 3.000000      3
    ## 3 3.833333 4.000000 3.333333 3.500000 3.500000 3.500000 2.666667      3
    ##     [,211]   [,212] [,213] [,214]   [,215] [,216]   [,217] [,218] [,219]
    ## 1 4.000000 4.000000      3    4.0 3.000000      4 4.000000      4    2.0
    ## 2 3.666667 3.000000      4    3.0 3.333333      3 2.666667      3    3.0
    ## 3 3.500000 3.166667      2    3.5 3.000000      4 3.833333      4    3.5
    ##     [,220]   [,221]   [,222]   [,223]   [,224]   [,225] [,226]   [,227]
    ## 1 3.000000 2.000000 4.000000 4.000000 4.000000 1.000000    1.0 2.000000
    ## 2 3.666667 3.000000 3.000000 3.000000 2.333333 2.000000    3.0 3.333333
    ## 3 3.333333 3.166667 3.666667 3.833333 3.833333 2.333333    2.5 2.666667
    ##     [,228] [,229]   [,230]   [,231]   [,232] [,233]   [,234]   [,235]
    ## 1 2.000000    4.0 3.000000 3.000000 4.000000      3 2.000000 2.000000
    ## 2 2.666667    3.0 3.000000 3.333333 3.000000      3 4.000000 1.666667
    ## 3 2.666667    3.5 3.333333 3.166667 3.666667      4 3.666667 3.500000
    ##     [,236]   [,237]   [,238]   [,239]   [,240]   [,241]   [,242]   [,243]
    ## 1 1.000000 2.000000 2.000000 4.000000 2.000000 4.000000 4.000000 1.000000
    ## 2 3.333333 2.666667 3.000000 4.000000 2.333333 3.333333 4.000000 3.000000
    ## 3 3.333333 2.833333 2.666667 3.833333 3.166667 3.500000 3.833333 2.833333
    ##     [,244]   [,245]   [,246]   [,247]   [,248] [,249] [,250]   [,251]
    ## 1 3.000000 2.000000 4.000000 3.000000 3.000000    3.0    4.0 4.000000
    ## 2 3.000000 3.000000 4.000000 2.000000 3.000000    3.0    3.0 3.333333
    ## 3 3.333333 3.166667 3.833333 3.166667 3.333333    3.5    3.5 4.000000
    ##     [,252]   [,253]   [,254]   [,255]   [,256]   [,257]   [,258] [,259]
    ## 1 1.000000 4.000000 1.000000 3.000000 4.000000 2.000000 1.000000    4.0
    ## 2 3.333333 3.000000 3.000000 2.666667 3.000000 2.666667 3.000000    3.0
    ## 3 3.500000 2.833333 3.333333 3.000000 3.166667 2.666667 2.666667    3.5
    ##     [,260]   [,261]   [,262]   [,263]   [,264] [,265] [,266] [,267] [,268]
    ## 1 2.000000 4.000000 3.000000 2.000000 4.000000      2      4      4    3.0
    ## 2 3.333333 4.000000 3.000000 3.666667 3.666667      3      4      4    2.0
    ## 3 2.833333 3.666667 2.833333 3.833333 3.666667      2      4      4    2.5
    ##     [,269]   [,270]   [,271]   [,272]   [,273]   [,274]   [,275]   [,276]
    ## 1 1.000000 4.000000 3.000000 2.000000 3.000000 4.000000 4.000000 3.000000
    ## 2 2.666667 3.333333 3.666667 1.666667 2.333333 4.000000 3.000000 2.666667
    ## 3 2.666667 3.166667 4.000000 2.000000 3.000000 3.333333 3.666667 4.000000
    ##   [,277]   [,278] [,279] [,280]   [,281]   [,282]   [,283]   [,284]
    ## 1      3 2.000000    4.0    4.0 3.000000 3.000000 3.000000 4.000000
    ## 2      3 3.000000    4.0    3.0 2.333333 3.000000 1.333333 3.333333
    ## 3      3 2.833333    3.5    3.5 3.333333 3.333333 2.666667 3.500000
    ##     [,285] [,286]   [,287]   [,288]   [,289]   [,290] [,291]   [,292]
    ## 1 2.000000      3 3.000000 2.000000 3.000000 3.000000      2 3.000000
    ## 2 2.666667      3 4.000000 2.000000 2.666667 2.000000      3 2.333333
    ## 3 3.166667      4 3.833333 3.166667 3.666667 3.166667      3 2.166667
    ##     [,293]   [,294]   [,295] [,296]   [,297]   [,298] [,299]   [,300]
    ## 1 3.000000 4.000000 4.000000      2 4.000000 1.000000      3 2.000000
    ## 2 4.000000 3.333333 3.666667      3 3.333333 3.333333      3 2.666667
    ## 3 3.833333 3.833333 3.666667      3 4.000000 1.833333      3 3.333333
    ##     [,301]   [,302]   [,303]   [,304]   [,305]   [,306]   [,307]   [,308]
    ## 1 4.000000 2.000000 3.000000 3.000000 3.000000 3.000000 4.000000 3.000000
    ## 2 3.000000 4.000000 2.333333 2.666667 3.000000 2.333333 4.000000 2.000000
    ## 3 3.666667 3.166667 3.666667 3.000000 3.833333 2.833333 3.166667 3.333333
    ##     [,309] [,310]   [,311] [,312]   [,313]   [,314]   [,315]   [,316]
    ## 1 2.000000    1.0 3.000000      4 4.000000 4.000000 3.000000 4.000000
    ## 2 2.666667    3.0 4.000000      4 3.666667 2.666667 3.000000 4.000000
    ## 3 2.833333    3.5 3.166667      4 3.333333 3.833333 3.333333 3.833333
    ##   [,317] [,318] [,319]   [,320]   [,321]   [,322] [,323]   [,324]   [,325]
    ## 1      3      4      4 3.000000 0.000000 4.000000    3.0 3.000000 4.000000
    ## 2      3      3      3 2.333333 3.000000 4.000000    3.0 3.000000 4.000000
    ## 3      3      4      4 2.833333 3.166667 3.833333    3.5 2.833333 3.666667
    ##     [,326] [,327]   [,328]   [,329]   [,330] [,331]   [,332]   [,333]
    ## 1 3.000000    4.0 2.000000 3.000000 4.000000    4.0 3.000000 4.000000
    ## 2 2.000000    4.0 4.000000 4.000000 3.333333    3.0 2.333333 3.666667
    ## 3 3.333333    3.5 3.833333 3.833333 4.000000    3.5 3.000000 3.833333
    ##   [,334]   [,335]   [,336]   [,337]   [,338]   [,339] [,340]   [,341]
    ## 1    3.0 2.000000 3.000000 3.000000 3.000000 3.000000      2 4.000000
    ## 2    4.0 2.666667 3.666667 2.333333 3.333333 4.000000      3 4.000000
    ## 3    3.5 2.833333 3.000000 3.166667 3.666667 3.666667      3 3.333333
    ##     [,342] [,343]   [,344]   [,345] [,346]   [,347]   [,348]   [,349]
    ## 1 4.000000      4 3.000000 2.000000    3.0 4.000000 4.000000 4.000000
    ## 2 3.000000      3 3.333333 3.000000    3.0 4.000000 3.666667 3.000000
    ## 3 2.833333      4 3.666667 3.333333    3.5 3.666667 3.333333 3.833333
    ##     [,350]   [,351]   [,352] [,353] [,354]   [,355]   [,356]   [,357]
    ## 1 1.000000 4.000000 2.000000    4.0      2 3.000000 3.000000 3.000000
    ## 2 2.666667 4.000000 3.666667    3.0      3 3.000000 2.333333 3.000000
    ## 3 3.333333 3.833333 2.500000    3.5      3 2.833333 3.000000 3.833333
    ##     [,358]   [,359]   [,360]   [,361]   [,362] [,363]   [,364]   [,365]
    ## 1 1.000000 3.000000 3.000000 4.000000 3.000000    3.0 4.000000 3.000000
    ## 2 4.000000 3.000000 3.000000 4.000000 3.000000    3.0 3.666667 3.000000
    ## 3 2.166667 3.833333 2.666667 3.333333 3.166667    3.5 3.333333 3.166667
    ##     [,366]   [,367]   [,368]   [,369]   [,370]   [,371]   [,372]   [,373]
    ## 1 3.000000 4.000000 3.000000 4.000000 3.000000 2.000000 4.000000 4.000000
    ## 2 2.000000 3.666667 2.000000 2.666667 3.000000 3.000000 3.333333 3.333333
    ## 3 2.333333 4.000000 2.833333 3.500000 3.166667 3.666667 3.666667 4.000000
    ##     [,374] [,375]   [,376]   [,377]   [,378] [,379]   [,380] [,381] [,382]
    ## 1 3.000000      3 3.000000 4.000000 2.000000      1 2.000000      4    4.0
    ## 2 3.000000      3 3.333333 4.000000 3.333333      3 2.666667      3    4.0
    ## 3 3.333333      3 3.666667 3.666667 3.666667      3 3.333333      4    3.5
    ##     [,383]   [,384] [,385] [,386]   [,387] [,388]   [,389]   [,390]
    ## 1 4.000000 4.000000      3    2.0 1.000000    4.0 2.000000 3.000000
    ## 2 3.666667 3.000000      4    3.0 2.666667    3.0 3.000000 3.333333
    ## 3 4.000000 3.666667      3    3.5 3.333333    3.5 3.166667 3.833333
    ##     [,391]   [,392] [,393] [,394]   [,395]   [,396]   [,397]   [,398]
    ## 1 4.000000 3.000000      4      4 4.000000 4.000000 4.000000 3.000000
    ## 2 3.333333 3.333333      4      4 2.666667 3.666667 2.666667 4.000000
    ## 3 4.000000 3.333333      4      4 3.666667 3.833333 3.500000 3.166667
    ##     [,399] [,400]   [,401]   [,402]   [,403] [,404]   [,405]   [,406]
    ## 1 4.000000    4.0 3.000000 3.000000 3.000000      4 3.000000 3.000000
    ## 2 3.000000    4.0 3.000000 2.000000 2.333333      3 2.000000 2.666667
    ## 3 3.333333    2.5 3.833333 3.833333 3.333333      4 2.833333 3.333333
    ##     [,407]   [,408]   [,409]   [,410]   [,411] [,412]   [,413]   [,414]
    ## 1 4.000000 4.000000 3.000000 4.000000 4.000000    4.0 2.000000 4.000000
    ## 2 3.666667 3.666667 3.000000 3.000000 3.000000    3.0 2.333333 3.666667
    ## 3 3.666667 4.000000 2.833333 3.166667 3.833333    3.5 2.833333 3.333333
    ##     [,415]   [,416] [,417]   [,418]   [,419]   [,420] [,421]   [,422]
    ## 1 3.000000 2.000000    4.0 2.000000 0.000000 0.000000    3.0 3.000000
    ## 2 3.333333 3.666667    4.0 3.333333 1.666667 1.333333    3.0 2.666667
    ## 3 2.833333 2.333333    3.5 3.166667 3.666667 2.333333    3.5 2.666667
    ##   [,423]   [,424]   [,425]   [,426]   [,427] [,428] [,429] [,430]   [,431]
    ## 1      3 3.000000 4.000000 3.000000 0.000000      1      4      4 3.000000
    ## 2      1 2.666667 2.666667 3.666667 3.666667      2      4      4 2.666667
    ## 3      3 2.166667 4.000000 3.166667 3.500000      3      4      4 3.166667
    ##     [,432]   [,433]
    ## 1 2.000000 3.000000
    ## 2 2.666667 3.333333
    ## 3 2.500000 3.166667
    ## 
    ## Clustering vector:
    ##       reliable         honest  unscrupulous*     deceitful* unintelligent* 
    ##              2              2              3              1              3 
    ##     obnoxious*     thankless*    unfriendly*     dependable         cruel* 
    ##              3              3              3              2              3 
    ## 
    ## Within cluster sum of squares by cluster:
    ## [1]    0.000  236.000 1162.333
    ##  (between_SS / total_SS =  34.8 %)
    ## 
    ## Available components:
    ## 
    ## [1] "cluster"      "centers"      "totss"        "withinss"    
    ## [5] "tot.withinss" "betweenss"    "size"         "iter"        
    ## [9] "ifault"