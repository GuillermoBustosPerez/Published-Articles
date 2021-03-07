Predicting Flake Mass: A View from Machine Learning. Lithic Technology
================
Guillermo Bustos-Pérez
7/3/2021

## 1) Load packages, read and check data

The data is available in this repository as a **.csv** file. Please note
that original data uses **“,”** as decimal marker instead of using
**“.”**. Thus, it is required to use the function **read\_csv2**.

``` r
# Load packages  
library(tidyverse)
```

    ## -- Attaching packages ------------------------------------------------- tidyverse 1.3.0 --

    ## v ggplot2 3.3.2     v purrr   0.3.4
    ## v tibble  3.0.3     v dplyr   1.0.2
    ## v tidyr   1.1.2     v stringr 1.4.0
    ## v readr   1.3.1     v forcats 0.5.0

    ## -- Conflicts ---------------------------------------------------- tidyverse_conflicts() --
    ## x dplyr::filter() masks stats::filter()
    ## x dplyr::lag()    masks stats::lag()

``` r
# Read in data
Reg_Data <- read.csv2("Data.csv")
```

Please note that here the function **kable()** from package
**kableExtra** is being employed to visualize imported data. Function
**head()** can also be employed.

<table>
<thead>
<tr>
<th style="text-align:right;">
Mean Thick
</th>
<th style="text-align:right;">
Max Thick
</th>
<th style="text-align:right;">
SD Thick
</th>
<th style="text-align:right;">
Weight
</th>
<th style="text-align:right;">
Surf Plat
</th>
<th style="text-align:right;">
Surf Plat II
</th>
<th style="text-align:right;">
Plat Depth
</th>
<th style="text-align:right;">
Cortex
</th>
<th style="text-align:right;">
Cortex Loc
</th>
<th style="text-align:right;">
No Scars
</th>
<th style="text-align:right;">
Long Ridges
</th>
<th style="text-align:right;">
EPA
</th>
<th style="text-align:right;">
IPA
</th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align:right;">
10.066667
</td>
<td style="text-align:right;">
13.1
</td>
<td style="text-align:right;">
2.720702
</td>
<td style="text-align:right;">
17.83
</td>
<td style="text-align:right;">
83.585
</td>
<td style="text-align:right;">
167.17
</td>
<td style="text-align:right;">
7.3
</td>
<td style="text-align:right;">
5
</td>
<td style="text-align:right;">
5
</td>
<td style="text-align:right;">
4
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
51
</td>
<td style="text-align:right;">
120
</td>
</tr>
<tr>
<td style="text-align:right;">
8.566667
</td>
<td style="text-align:right;">
9.7
</td>
<td style="text-align:right;">
1.265789
</td>
<td style="text-align:right;">
13.33
</td>
<td style="text-align:right;">
90.480
</td>
<td style="text-align:right;">
127.14
</td>
<td style="text-align:right;">
7.8
</td>
<td style="text-align:right;">
5
</td>
<td style="text-align:right;">
5
</td>
<td style="text-align:right;">
3
</td>
<td style="text-align:right;">
0
</td>
<td style="text-align:right;">
70
</td>
<td style="text-align:right;">
105
</td>
</tr>
<tr>
<td style="text-align:right;">
11.566667
</td>
<td style="text-align:right;">
16.8
</td>
<td style="text-align:right;">
4.601691
</td>
<td style="text-align:right;">
20.33
</td>
<td style="text-align:right;">
40.500
</td>
<td style="text-align:right;">
81.00
</td>
<td style="text-align:right;">
3.6
</td>
<td style="text-align:right;">
3
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
0
</td>
<td style="text-align:right;">
35
</td>
<td style="text-align:right;">
115
</td>
</tr>
<tr>
<td style="text-align:right;">
5.500000
</td>
<td style="text-align:right;">
6.7
</td>
<td style="text-align:right;">
1.423610
</td>
<td style="text-align:right;">
3.98
</td>
<td style="text-align:right;">
59.670
</td>
<td style="text-align:right;">
79.05
</td>
<td style="text-align:right;">
5.1
</td>
<td style="text-align:right;">
5
</td>
<td style="text-align:right;">
5
</td>
<td style="text-align:right;">
3
</td>
<td style="text-align:right;">
0
</td>
<td style="text-align:right;">
66
</td>
<td style="text-align:right;">
111
</td>
</tr>
<tr>
<td style="text-align:right;">
11.166667
</td>
<td style="text-align:right;">
13.3
</td>
<td style="text-align:right;">
2.946561
</td>
<td style="text-align:right;">
22.18
</td>
<td style="text-align:right;">
109.800
</td>
<td style="text-align:right;">
219.60
</td>
<td style="text-align:right;">
12.0
</td>
<td style="text-align:right;">
4
</td>
<td style="text-align:right;">
4
</td>
<td style="text-align:right;">
3
</td>
<td style="text-align:right;">
0
</td>
<td style="text-align:right;">
68
</td>
<td style="text-align:right;">
107
</td>
</tr>
</tbody>
</table>

``` r
# Get column names
colnames(Reg_Data)
```

    ##  [1] "Mean_Thick"   "Max_Thick"    "SD_Thick"     "Weight"       "Surf_Plat"   
    ##  [6] "Surf_Plat_II" "Plat_Depth"   "Cortex"       "Cortex_Loc"   "No_Scars"    
    ## [11] "Long_Ridges"  "EPA"          "IPA"

``` r
Reg_Data_2 <- Reg_Data %>% 
  mutate(Log_Weight = log10(Weight),
         Log_Thick = log10(MeanThick),
         Log_SD_Thick = log10(SDThick),
         Log_Plat = log10(Surface.Plat),
         Log_Plat_2 = log10(Surf_Plat_II),
         Log_Plat_De = log10(`Platform Deepnes`),
         Log_EPA = log10(EPA),
         Log_IPA = log10(IPA)) 
```

## References
