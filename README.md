
- [biotidy](#biotidy)
- [Introduction](#introduction)
- [makePerCellDF](#makepercelldf)
- [makePerFeatureDF](#makeperfeaturedf)
- [Session information](#session-information)

<!-- README.md is generated from README.Rmd. Please edit that file -->

# biotidy

<!-- badges: start -->

[![R-CMD-check](https://github.com/Yunuuuu/biotidy/actions/workflows/R-CMD-check.yaml/badge.svg)](https://github.com/Yunuuuu/biotidy/actions/workflows/R-CMD-check.yaml)
<!-- badges: end -->

# Introduction

`biotidy` package offers useful utilities for integrating Bioinformatic
objects such as SummarizedExperiment and Seurat into the tidy(verse)
framework.

It is important to note that `biotidy` serves a different purpose
compared to [tidyomics](https://github.com/tidyomics), which primarily
utilizes a pipe-based workflow for managing bioinformatic objects.
Specifically, “biotidy” provides a method for extracting a data frame
from bioinformatic objects, similar to how the `broom::tidy` function
operates on statistical model objects. The inspiration for `biotidy`
came from the functionality of the `scuttle::makePerCellDF` and
`scuttle::makePerFeatureDF` functions on the `SingleCellExperiment`
object. The `biotidy` package extends these functions to work with other
bioinformatic objects, such as `SummarizedExperiment`, `ExpressionSet`,
and `Seurat`.

``` r
library(biotidy)
mocked_se <- mockSE()
mocked_sce <- mockSCE()
mocked_es <- mockES()
mocked_seurat <- mockSeurat()
```

# makePerCellDF

`makePerCellDF` creates a per-cell data.frame (i.e., where each row
represents a sample / cell) from the Bioinformatic objects.

``` r
# SummarizedExperiment method
makePerCellDF(mocked_se)[1:5, 1:5]
#>         Gene0001 Gene0002 Gene0003 Gene0004 Gene0005
#> Cell001        0      653        0      303        0
#> Cell002        7       64        2      499        1
#> Cell003        2      814        0      299       23
#> Cell004        0      464        2      167       17
#> Cell005        2      497        8      563       46
```

``` r
makePerCellDF(mocked_se, melt = TRUE)[1:5, 1:5]
#>   Mutation_Status Cell_Cycle Treatment .features .assay
#> 1        negative         G0    treat1  Gene0001      0
#> 2        positive         G1    treat2  Gene0001      7
#> 3        negative        G2M    treat1  Gene0001      2
#> 4        negative        G2M    treat2  Gene0001      0
#> 5        negative         G1    treat2  Gene0001      2
```

``` r
makePerCellDF(mocked_se, melt = TRUE, keep_rownames = TRUE)[1:5, 1:5]
#>       .id Mutation_Status Cell_Cycle Treatment .features
#> 1 Cell001        negative         G0    treat1  Gene0001
#> 2 Cell002        positive         G1    treat2  Gene0001
#> 3 Cell003        negative        G2M    treat1  Gene0001
#> 4 Cell004        negative        G2M    treat2  Gene0001
#> 5 Cell005        negative         G1    treat2  Gene0001
```

``` r
makePerCellDF(mocked_se, features = FALSE, use_coldata = FALSE)
#> data frame with 0 columns and 200 rows
```

``` r
# SingleCellExperiment method
makePerCellDF(mocked_sce)[1:5, 1:5]
#>         Gene0001 Gene0002 Gene0003 Gene0004 Gene0005
#> Cell001        0        5       29      360        0
#> Cell002        0        1       18      776        0
#> Cell003        1        0       99      234        0
#> Cell004       15       57       36       69        0
#> Cell005        1       23      213       24        0
```

``` r
makePerCellDF(mocked_sce, melt = TRUE)[1:5, 1:5]
#>   Mutation_Status Cell_Cycle Treatment .features .assay
#> 1        positive         G0    treat1  Gene0001      0
#> 2        positive          S    treat2  Gene0001      0
#> 3        positive          S    treat2  Gene0001      1
#> 4        positive         G0    treat2  Gene0001     15
#> 5        negative          S    treat2  Gene0001      1
```

``` r
makePerCellDF(mocked_sce, melt = TRUE, keep_rownames = TRUE)[1:5, 1:5]
#>       .id Mutation_Status Cell_Cycle Treatment .features
#> 1 Cell001        positive         G0    treat1  Gene0001
#> 2 Cell002        positive          S    treat2  Gene0001
#> 3 Cell003        positive          S    treat2  Gene0001
#> 4 Cell004        positive         G0    treat2  Gene0001
#> 5 Cell005        negative          S    treat2  Gene0001
```

``` r
makePerCellDF(mocked_sce, features = FALSE, use_coldata = FALSE)
#> data frame with 0 columns and 200 rows
```

``` r
# ExpressionSet method
makePerCellDF(mocked_es)[1:5, 1:5]
#>         Gene0001 Gene0002 Gene0003 Gene0004 Gene0005
#> Cell001        0        3        0       16        0
#> Cell002        6       26      263       21        0
#> Cell003        0       32        6       23        1
#> Cell004       26       25       95        6        0
#> Cell005        3        0       38      108        2
```

``` r
makePerCellDF(mocked_es, melt = TRUE)[1:5, 1:5]
#>   Mutation_Status Cell_Cycle Treatment .features .assay
#> 1        negative        G2M    treat2  Gene0001      0
#> 2        positive        G2M    treat2  Gene0001      6
#> 3        positive          S    treat2  Gene0001      0
#> 4        positive         G0    treat1  Gene0001     26
#> 5        positive          S    treat2  Gene0001      3
```

``` r
makePerCellDF(mocked_es, melt = TRUE, keep_rownames = TRUE)[1:5, 1:5]
#>       .id Mutation_Status Cell_Cycle Treatment .features
#> 1 Cell001        negative        G2M    treat2  Gene0001
#> 2 Cell002        positive        G2M    treat2  Gene0001
#> 3 Cell003        positive          S    treat2  Gene0001
#> 4 Cell004        positive         G0    treat1  Gene0001
#> 5 Cell005        positive          S    treat2  Gene0001
```

``` r
makePerCellDF(mocked_es, features = FALSE, use_coldata = FALSE)
#> data frame with 0 columns and 200 rows
```

``` r
# Seurat method
makePerCellDF(mocked_seurat, layer = "counts")[1:5, 1:5]
#>         Gene0001 Gene0002 Gene0003 Gene0004 Gene0005
#> Cell001       98      105        7        4      215
#> Cell002       20       14        7       69      513
#> Cell003        0       87        0       58       31
#> Cell004        0       51        7      125      508
#> Cell005       59      109        2      105       85
```

``` r
makePerCellDF(mocked_seurat, layer = "counts", melt = TRUE)[1:5, 1:5]
#>      orig.ident nCount_RNA nFeature_RNA Mutation_Status Cell_Cycle
#> 1 SeuratProject     373573         1487        negative        G2M
#> 2 SeuratProject     353555         1509        negative        G2M
#> 3 SeuratProject     346423         1492        negative         G0
#> 4 SeuratProject     379401         1486        negative         G1
#> 5 SeuratProject     352126         1480        negative          S
```

``` r
makePerCellDF(mocked_seurat,
  layer = "counts", melt = TRUE, keep_rownames = TRUE
)[1:5, 1:5]
#>       .id    orig.ident nCount_RNA nFeature_RNA Mutation_Status
#> 1 Cell001 SeuratProject     373573         1487        negative
#> 2 Cell002 SeuratProject     353555         1509        negative
#> 3 Cell003 SeuratProject     346423         1492        negative
#> 4 Cell004 SeuratProject     379401         1486        negative
#> 5 Cell005 SeuratProject     352126         1480        negative
```

``` r
makePerCellDF(mocked_seurat,
  layer = "counts", features = FALSE, use_coldata =
    FALSE
)
#> data frame with 0 columns and 200 rows
```

# makePerFeatureDF

`makePerFeatureDF` Create a per-feature data.frame (i.e., where each row
represents a feature / gene).

``` r
# SummarizedExperiment method
makePerFeatureDF(mocked_se)[1:5, 1:5]
#>          Cell001 Cell002 Cell003 Cell004 Cell005
#> Gene0001       0       7       2       0       2
#> Gene0002     653      64     814     464     497
#> Gene0003       0       2       0       2       8
#> Gene0004     303     499     299     167     563
#> Gene0005       0       1      23      17      46
```

``` r
makePerFeatureDF(mocked_se, melt = TRUE)[1:5, ]
#>    .cells .assay
#> 1 Cell001      0
#> 2 Cell001    653
#> 3 Cell001      0
#> 4 Cell001    303
#> 5 Cell001      0
```

``` r
makePerFeatureDF(mocked_se, melt = TRUE, keep_rownames = TRUE)[1:5, ]
#>        .id  .cells .assay
#> 1 Gene0001 Cell001      0
#> 2 Gene0002 Cell001    653
#> 3 Gene0003 Cell001      0
#> 4 Gene0004 Cell001    303
#> 5 Gene0005 Cell001      0
```

``` r
makePerFeatureDF(mocked_se, features = FALSE, use_rowdata = FALSE)
#>   [1] Cell001 Cell002 Cell003 Cell004 Cell005 Cell006 Cell007 Cell008 Cell009
#>  [10] Cell010 Cell011 Cell012 Cell013 Cell014 Cell015 Cell016 Cell017 Cell018
#>  [19] Cell019 Cell020 Cell021 Cell022 Cell023 Cell024 Cell025 Cell026 Cell027
#>  [28] Cell028 Cell029 Cell030 Cell031 Cell032 Cell033 Cell034 Cell035 Cell036
#>  [37] Cell037 Cell038 Cell039 Cell040 Cell041 Cell042 Cell043 Cell044 Cell045
#>  [46] Cell046 Cell047 Cell048 Cell049 Cell050 Cell051 Cell052 Cell053 Cell054
#>  [55] Cell055 Cell056 Cell057 Cell058 Cell059 Cell060 Cell061 Cell062 Cell063
#>  [64] Cell064 Cell065 Cell066 Cell067 Cell068 Cell069 Cell070 Cell071 Cell072
#>  [73] Cell073 Cell074 Cell075 Cell076 Cell077 Cell078 Cell079 Cell080 Cell081
#>  [82] Cell082 Cell083 Cell084 Cell085 Cell086 Cell087 Cell088 Cell089 Cell090
#>  [91] Cell091 Cell092 Cell093 Cell094 Cell095 Cell096 Cell097 Cell098 Cell099
#> [100] Cell100 Cell101 Cell102 Cell103 Cell104 Cell105 Cell106 Cell107 Cell108
#> [109] Cell109 Cell110 Cell111 Cell112 Cell113 Cell114 Cell115 Cell116 Cell117
#> [118] Cell118 Cell119 Cell120 Cell121 Cell122 Cell123 Cell124 Cell125 Cell126
#> [127] Cell127 Cell128 Cell129 Cell130 Cell131 Cell132 Cell133 Cell134 Cell135
#> [136] Cell136 Cell137 Cell138 Cell139 Cell140 Cell141 Cell142 Cell143 Cell144
#> [145] Cell145 Cell146 Cell147 Cell148 Cell149 Cell150 Cell151 Cell152 Cell153
#> [154] Cell154 Cell155 Cell156 Cell157 Cell158 Cell159 Cell160 Cell161 Cell162
#> [163] Cell163 Cell164 Cell165 Cell166 Cell167 Cell168 Cell169 Cell170 Cell171
#> [172] Cell172 Cell173 Cell174 Cell175 Cell176 Cell177 Cell178 Cell179 Cell180
#> [181] Cell181 Cell182 Cell183 Cell184 Cell185 Cell186 Cell187 Cell188 Cell189
#> [190] Cell190 Cell191 Cell192 Cell193 Cell194 Cell195 Cell196 Cell197 Cell198
#> [199] Cell199 Cell200
#> <0 rows> (or 0-length row.names)
```

``` r
# SingleCellExperiment method
makePerFeatureDF(mocked_sce)[1:5, 1:5]
#>          Cell001 Cell002 Cell003 Cell004 Cell005
#> Gene0001       0       0       1      15       1
#> Gene0002       5       1       0      57      23
#> Gene0003      29      18      99      36     213
#> Gene0004     360     776     234      69      24
#> Gene0005       0       0       0       0       0
```

``` r
makePerFeatureDF(mocked_sce, melt = TRUE)[1:5, ]
#>    .cells .assay
#> 1 Cell001      0
#> 2 Cell001      5
#> 3 Cell001     29
#> 4 Cell001    360
#> 5 Cell001      0
```

``` r
makePerFeatureDF(mocked_sce, melt = TRUE, keep_rownames = TRUE)[1:5, ]
#>        .id  .cells .assay
#> 1 Gene0001 Cell001      0
#> 2 Gene0002 Cell001      5
#> 3 Gene0003 Cell001     29
#> 4 Gene0004 Cell001    360
#> 5 Gene0005 Cell001      0
```

``` r
makePerFeatureDF(mocked_sce, features = FALSE, use_rowdata = FALSE)
#>   [1] Cell001 Cell002 Cell003 Cell004 Cell005 Cell006 Cell007 Cell008 Cell009
#>  [10] Cell010 Cell011 Cell012 Cell013 Cell014 Cell015 Cell016 Cell017 Cell018
#>  [19] Cell019 Cell020 Cell021 Cell022 Cell023 Cell024 Cell025 Cell026 Cell027
#>  [28] Cell028 Cell029 Cell030 Cell031 Cell032 Cell033 Cell034 Cell035 Cell036
#>  [37] Cell037 Cell038 Cell039 Cell040 Cell041 Cell042 Cell043 Cell044 Cell045
#>  [46] Cell046 Cell047 Cell048 Cell049 Cell050 Cell051 Cell052 Cell053 Cell054
#>  [55] Cell055 Cell056 Cell057 Cell058 Cell059 Cell060 Cell061 Cell062 Cell063
#>  [64] Cell064 Cell065 Cell066 Cell067 Cell068 Cell069 Cell070 Cell071 Cell072
#>  [73] Cell073 Cell074 Cell075 Cell076 Cell077 Cell078 Cell079 Cell080 Cell081
#>  [82] Cell082 Cell083 Cell084 Cell085 Cell086 Cell087 Cell088 Cell089 Cell090
#>  [91] Cell091 Cell092 Cell093 Cell094 Cell095 Cell096 Cell097 Cell098 Cell099
#> [100] Cell100 Cell101 Cell102 Cell103 Cell104 Cell105 Cell106 Cell107 Cell108
#> [109] Cell109 Cell110 Cell111 Cell112 Cell113 Cell114 Cell115 Cell116 Cell117
#> [118] Cell118 Cell119 Cell120 Cell121 Cell122 Cell123 Cell124 Cell125 Cell126
#> [127] Cell127 Cell128 Cell129 Cell130 Cell131 Cell132 Cell133 Cell134 Cell135
#> [136] Cell136 Cell137 Cell138 Cell139 Cell140 Cell141 Cell142 Cell143 Cell144
#> [145] Cell145 Cell146 Cell147 Cell148 Cell149 Cell150 Cell151 Cell152 Cell153
#> [154] Cell154 Cell155 Cell156 Cell157 Cell158 Cell159 Cell160 Cell161 Cell162
#> [163] Cell163 Cell164 Cell165 Cell166 Cell167 Cell168 Cell169 Cell170 Cell171
#> [172] Cell172 Cell173 Cell174 Cell175 Cell176 Cell177 Cell178 Cell179 Cell180
#> [181] Cell181 Cell182 Cell183 Cell184 Cell185 Cell186 Cell187 Cell188 Cell189
#> [190] Cell190 Cell191 Cell192 Cell193 Cell194 Cell195 Cell196 Cell197 Cell198
#> [199] Cell199 Cell200
#> <0 rows> (or 0-length row.names)
```

``` r
# ExpressionSet method
makePerFeatureDF(mocked_es)[1:5, 1:5]
#>          Cell001 Cell002 Cell003 Cell004 Cell005
#> Gene0001       0       6       0      26       3
#> Gene0002       3      26      32      25       0
#> Gene0003       0     263       6      95      38
#> Gene0004      16      21      23       6     108
#> Gene0005       0       0       1       0       2
```

``` r
makePerFeatureDF(mocked_es, melt = TRUE)[1:5, ]
#>    .cells .assay
#> 1 Cell001      0
#> 2 Cell001      3
#> 3 Cell001      0
#> 4 Cell001     16
#> 5 Cell001      0
```

``` r
makePerFeatureDF(mocked_es, melt = TRUE, keep_rownames = TRUE)[1:5, ]
#>        .id  .cells .assay
#> 1 Gene0001 Cell001      0
#> 2 Gene0002 Cell001      3
#> 3 Gene0003 Cell001      0
#> 4 Gene0004 Cell001     16
#> 5 Gene0005 Cell001      0
```

``` r
makePerFeatureDF(mocked_es, features = FALSE, use_rowdata = FALSE)
#>   [1] Cell001 Cell002 Cell003 Cell004 Cell005 Cell006 Cell007 Cell008 Cell009
#>  [10] Cell010 Cell011 Cell012 Cell013 Cell014 Cell015 Cell016 Cell017 Cell018
#>  [19] Cell019 Cell020 Cell021 Cell022 Cell023 Cell024 Cell025 Cell026 Cell027
#>  [28] Cell028 Cell029 Cell030 Cell031 Cell032 Cell033 Cell034 Cell035 Cell036
#>  [37] Cell037 Cell038 Cell039 Cell040 Cell041 Cell042 Cell043 Cell044 Cell045
#>  [46] Cell046 Cell047 Cell048 Cell049 Cell050 Cell051 Cell052 Cell053 Cell054
#>  [55] Cell055 Cell056 Cell057 Cell058 Cell059 Cell060 Cell061 Cell062 Cell063
#>  [64] Cell064 Cell065 Cell066 Cell067 Cell068 Cell069 Cell070 Cell071 Cell072
#>  [73] Cell073 Cell074 Cell075 Cell076 Cell077 Cell078 Cell079 Cell080 Cell081
#>  [82] Cell082 Cell083 Cell084 Cell085 Cell086 Cell087 Cell088 Cell089 Cell090
#>  [91] Cell091 Cell092 Cell093 Cell094 Cell095 Cell096 Cell097 Cell098 Cell099
#> [100] Cell100 Cell101 Cell102 Cell103 Cell104 Cell105 Cell106 Cell107 Cell108
#> [109] Cell109 Cell110 Cell111 Cell112 Cell113 Cell114 Cell115 Cell116 Cell117
#> [118] Cell118 Cell119 Cell120 Cell121 Cell122 Cell123 Cell124 Cell125 Cell126
#> [127] Cell127 Cell128 Cell129 Cell130 Cell131 Cell132 Cell133 Cell134 Cell135
#> [136] Cell136 Cell137 Cell138 Cell139 Cell140 Cell141 Cell142 Cell143 Cell144
#> [145] Cell145 Cell146 Cell147 Cell148 Cell149 Cell150 Cell151 Cell152 Cell153
#> [154] Cell154 Cell155 Cell156 Cell157 Cell158 Cell159 Cell160 Cell161 Cell162
#> [163] Cell163 Cell164 Cell165 Cell166 Cell167 Cell168 Cell169 Cell170 Cell171
#> [172] Cell172 Cell173 Cell174 Cell175 Cell176 Cell177 Cell178 Cell179 Cell180
#> [181] Cell181 Cell182 Cell183 Cell184 Cell185 Cell186 Cell187 Cell188 Cell189
#> [190] Cell190 Cell191 Cell192 Cell193 Cell194 Cell195 Cell196 Cell197 Cell198
#> [199] Cell199 Cell200
#> <0 rows> (or 0-length row.names)
```

``` r
# Seurat method
makePerFeatureDF(mocked_seurat, layer = "counts")[1:5, 1:5]
#>          Cell001 Cell002 Cell003 Cell004 Cell005
#> Gene0001      98      20       0       0      59
#> Gene0002     105      14      87      51     109
#> Gene0003       7       7       0       7       2
#> Gene0004       4      69      58     125     105
#> Gene0005     215     513      31     508      85
```

``` r
makePerFeatureDF(mocked_seurat, layer = "counts", melt = TRUE)[1:5, ]
#>    .cells .assay
#> 1 Cell001     98
#> 2 Cell001    105
#> 3 Cell001      7
#> 4 Cell001      4
#> 5 Cell001    215
```

``` r
makePerFeatureDF(mocked_seurat,
  layer = "counts", melt = TRUE, keep_rownames = TRUE
)[1:5, ]
#>        .id  .cells .assay
#> 1 Gene0001 Cell001     98
#> 2 Gene0002 Cell001    105
#> 3 Gene0003 Cell001      7
#> 4 Gene0004 Cell001      4
#> 5 Gene0005 Cell001    215
```

``` r
makePerFeatureDF(mocked_seurat,
  layer = "counts", features = FALSE,
  use_rowdata = FALSE
)
#>   [1] Cell001 Cell002 Cell003 Cell004 Cell005 Cell006 Cell007 Cell008 Cell009
#>  [10] Cell010 Cell011 Cell012 Cell013 Cell014 Cell015 Cell016 Cell017 Cell018
#>  [19] Cell019 Cell020 Cell021 Cell022 Cell023 Cell024 Cell025 Cell026 Cell027
#>  [28] Cell028 Cell029 Cell030 Cell031 Cell032 Cell033 Cell034 Cell035 Cell036
#>  [37] Cell037 Cell038 Cell039 Cell040 Cell041 Cell042 Cell043 Cell044 Cell045
#>  [46] Cell046 Cell047 Cell048 Cell049 Cell050 Cell051 Cell052 Cell053 Cell054
#>  [55] Cell055 Cell056 Cell057 Cell058 Cell059 Cell060 Cell061 Cell062 Cell063
#>  [64] Cell064 Cell065 Cell066 Cell067 Cell068 Cell069 Cell070 Cell071 Cell072
#>  [73] Cell073 Cell074 Cell075 Cell076 Cell077 Cell078 Cell079 Cell080 Cell081
#>  [82] Cell082 Cell083 Cell084 Cell085 Cell086 Cell087 Cell088 Cell089 Cell090
#>  [91] Cell091 Cell092 Cell093 Cell094 Cell095 Cell096 Cell097 Cell098 Cell099
#> [100] Cell100 Cell101 Cell102 Cell103 Cell104 Cell105 Cell106 Cell107 Cell108
#> [109] Cell109 Cell110 Cell111 Cell112 Cell113 Cell114 Cell115 Cell116 Cell117
#> [118] Cell118 Cell119 Cell120 Cell121 Cell122 Cell123 Cell124 Cell125 Cell126
#> [127] Cell127 Cell128 Cell129 Cell130 Cell131 Cell132 Cell133 Cell134 Cell135
#> [136] Cell136 Cell137 Cell138 Cell139 Cell140 Cell141 Cell142 Cell143 Cell144
#> [145] Cell145 Cell146 Cell147 Cell148 Cell149 Cell150 Cell151 Cell152 Cell153
#> [154] Cell154 Cell155 Cell156 Cell157 Cell158 Cell159 Cell160 Cell161 Cell162
#> [163] Cell163 Cell164 Cell165 Cell166 Cell167 Cell168 Cell169 Cell170 Cell171
#> [172] Cell172 Cell173 Cell174 Cell175 Cell176 Cell177 Cell178 Cell179 Cell180
#> [181] Cell181 Cell182 Cell183 Cell184 Cell185 Cell186 Cell187 Cell188 Cell189
#> [190] Cell190 Cell191 Cell192 Cell193 Cell194 Cell195 Cell196 Cell197 Cell198
#> [199] Cell199 Cell200
#> <0 rows> (or 0-length row.names)
```

# Session information

``` r
sessionInfo()
#> R version 4.4.0 (2024-04-24)
#> Platform: x86_64-pc-linux-gnu
#> Running under: Ubuntu 24.04 LTS
#> 
#> Matrix products: default
#> BLAS/LAPACK: /usr/lib/x86_64-linux-gnu/libmkl_rt.so;  LAPACK version 3.8.0
#> 
#> locale:
#>  [1] LC_CTYPE=C.UTF-8       LC_NUMERIC=C           LC_TIME=C.UTF-8       
#>  [4] LC_COLLATE=C.UTF-8     LC_MONETARY=C.UTF-8    LC_MESSAGES=C.UTF-8   
#>  [7] LC_PAPER=C.UTF-8       LC_NAME=C              LC_ADDRESS=C          
#> [10] LC_TELEPHONE=C         LC_MEASUREMENT=C.UTF-8 LC_IDENTIFICATION=C   
#> 
#> time zone: Asia/Shanghai
#> tzcode source: system (glibc)
#> 
#> attached base packages:
#> [1] stats     graphics  grDevices utils     datasets  methods   base     
#> 
#> other attached packages:
#> [1] biotidy_0.99.0
#> 
#> loaded via a namespace (and not attached):
#>  [1] Matrix_1.7-0                future.apply_1.11.2        
#>  [3] jsonlite_1.8.8              compiler_4.4.0             
#>  [5] crayon_1.5.3                Rcpp_1.0.12                
#>  [7] SeuratObject_5.0.2          SummarizedExperiment_1.34.0
#>  [9] Biobase_2.64.0              GenomicRanges_1.56.1       
#> [11] parallel_4.4.0              globals_0.16.3             
#> [13] IRanges_2.38.0              yaml_2.3.8                 
#> [15] fastmap_1.2.0               lattice_0.22-6             
#> [17] R6_2.5.1                    XVector_0.44.0             
#> [19] generics_0.1.3              S4Arrays_1.4.1             
#> [21] GenomeInfoDb_1.40.1         knitr_1.47                 
#> [23] BiocGenerics_0.50.0         dotCall64_1.1-1            
#> [25] future_1.33.2               DelayedArray_0.30.1        
#> [27] MatrixGenerics_1.16.0       GenomeInfoDbData_1.2.12    
#> [29] rlang_1.1.4                 sp_2.1-4                   
#> [31] xfun_0.45                   SparseArray_1.4.8          
#> [33] cli_3.6.3                   progressr_0.14.0           
#> [35] zlibbioc_1.50.0             digest_0.6.36              
#> [37] grid_4.4.0                  spam_2.10-0                
#> [39] lifecycle_1.0.4             S4Vectors_0.42.0           
#> [41] SingleCellExperiment_1.26.0 data.table_1.15.4          
#> [43] evaluate_0.24.0             listenv_0.9.1              
#> [45] codetools_0.2-20            abind_1.4-5                
#> [47] stats4_4.4.0                parallelly_1.37.1          
#> [49] rmarkdown_2.27              httr_1.4.7                 
#> [51] matrixStats_1.3.0           tools_4.4.0                
#> [53] htmltools_0.5.8.1           UCSC.utils_1.0.0
```
