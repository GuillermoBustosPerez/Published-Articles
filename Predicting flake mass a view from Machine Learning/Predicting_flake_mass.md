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
Length
</th>
<th style="text-align:right;">
Width
</th>
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
51.3
</td>
<td style="text-align:right;">
29.8
</td>
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
49.1
</td>
<td style="text-align:right;">
30.0
</td>
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
30.8
</td>
<td style="text-align:right;">
43.8
</td>
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
30.2
</td>
<td style="text-align:right;">
19.6
</td>
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
57.1
</td>
<td style="text-align:right;">
37.8
</td>
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

    ##  [1] "Length"       "Width"        "Mean_Thick"   "Max_Thick"    "SD_Thick"    
    ##  [6] "Weight"       "Surf_Plat"    "Surf_Plat_II" "Plat_Depth"   "Cortex"      
    ## [11] "Cortex_Loc"   "No_Scars"     "Long_Ridges"  "EPA"          "IPA"

## Descriptive statistics of experimental assemblage

The sample consisted of 300 freehand experimental flint flakes knapped
using a hard hammer. Flakes belonged to nearly 20 knapping sequences
wherein a wide variety of knapping methods were employed – hierarchical
(Levallois and hierarchical discoid), bifacial (discoid), and unipolar –
to generate the experimental sample, ensuring a wide range of
morphologies. All selected flakes from the knapping sequences were
complete, and all presented feather terminations. Since a key aspect of
the experimentation was to estimate flake mass and independently of
exterior factors, hammerstones included a wide selection of limestone,
sandstone, and quartzite pebbles, allowing for a diverse range of
morphologies and potential active percussion areas.

Summary statistics of the experimental assemblage. Note that **“Platform
size1”** refers to platform size measured following Muller and Clarkson
(2016); while **“Platform size2”** refers to measures following
Andrefsky (2005).

``` r
# Make data frame of descriptive statistics of assemblage
Summary_Assem <- data.frame(
    rbind(
      data.frame(data.matrix(summary(Reg_Data$Length))) %>% t(),
      data.frame(data.matrix(summary(Reg_Data$Width))) %>% t(),
      data.frame(data.matrix(summary(Reg_Data$Mean_Thick))) %>% t(),
      data.frame(data.matrix(summary(Reg_Data$Surf_Plat))) %>% t(),
      data.frame(data.matrix(summary(Reg_Data$Surf_Plat_II))) %>% t(),
      data.frame(data.matrix(summary(Reg_Data$Weight))) %>% t()
      ))

# Place column names for each measure
Measure <- c("Length", "Width", "Mean Thickness", "Platform Surface1",
             "Platform Surface2", "Weight")

Summary_Assem <- cbind(Measure, Summary_Assem)

rownames(Summary_Assem) <- 1:nrow(Summary_Assem)
```

<table>
<thead>
<tr>
<th style="text-align:left;">
Measure
</th>
<th style="text-align:right;">
Min.
</th>
<th style="text-align:right;">
X1st.Qu.
</th>
<th style="text-align:right;">
Median
</th>
<th style="text-align:right;">
Mean
</th>
<th style="text-align:right;">
X3rd.Qu.
</th>
<th style="text-align:right;">
Max.
</th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align:left;">
Length
</td>
<td style="text-align:right;">
15.40
</td>
<td style="text-align:right;">
31.675000
</td>
<td style="text-align:right;">
38.450
</td>
<td style="text-align:right;">
40.858000
</td>
<td style="text-align:right;">
48.425000
</td>
<td style="text-align:right;">
86.80
</td>
</tr>
<tr>
<td style="text-align:left;">
Width
</td>
<td style="text-align:right;">
11.90
</td>
<td style="text-align:right;">
26.500000
</td>
<td style="text-align:right;">
34.150
</td>
<td style="text-align:right;">
35.390333
</td>
<td style="text-align:right;">
41.500000
</td>
<td style="text-align:right;">
79.50
</td>
</tr>
<tr>
<td style="text-align:left;">
Mean Thickness
</td>
<td style="text-align:right;">
1.80
</td>
<td style="text-align:right;">
5.233333
</td>
<td style="text-align:right;">
7.000
</td>
<td style="text-align:right;">
7.629111
</td>
<td style="text-align:right;">
9.441667
</td>
<td style="text-align:right;">
20.20
</td>
</tr>
<tr>
<td style="text-align:left;">
Platform Surface1
</td>
<td style="text-align:right;">
3.96
</td>
<td style="text-align:right;">
23.105000
</td>
<td style="text-align:right;">
45.560
</td>
<td style="text-align:right;">
63.814490
</td>
<td style="text-align:right;">
85.187500
</td>
<td style="text-align:right;">
477.95
</td>
</tr>
<tr>
<td style="text-align:left;">
Platform Surface2
</td>
<td style="text-align:right;">
5.40
</td>
<td style="text-align:right;">
35.737500
</td>
<td style="text-align:right;">
77.135
</td>
<td style="text-align:right;">
102.703833
</td>
<td style="text-align:right;">
136.612500
</td>
<td style="text-align:right;">
821.50
</td>
</tr>
<tr>
<td style="text-align:left;">
Weight
</td>
<td style="text-align:right;">
0.50
</td>
<td style="text-align:right;">
4.145000
</td>
<td style="text-align:right;">
7.395
</td>
<td style="text-align:right;">
11.282867
</td>
<td style="text-align:right;">
14.017500
</td>
<td style="text-align:right;">
96.20
</td>
</tr>
</tbody>
</table>

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
