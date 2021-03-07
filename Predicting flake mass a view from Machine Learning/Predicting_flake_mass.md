Predicting Flake Mass: A View from Machine Learning. Lithic Technology
================
Guillermo Bustos-Pérez
7/3/2021

## 1) Load packages and Read in data

The data is available in this repository as a .csv file.

``` r
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
Reg_Data <- read.csv2("Data.csv")
```

``` r
Reg_Data_2 <- Reg_Data_2 %>% 
  mutate(Log_Weight = log10(Weight),
         Log_Thick = log10(MeanThick),
         Log_SD_Thick = log10(SDThick),
         Log_Plat = log10(Surface.Plat),
         Log_Plat_2 = log10(Surf_Plat_II),
         Log_Plat_De = log10(`Platform Deepnes`),
         Log_EPA = log10(EPA),
         Log_IPA = log10(IPA)) 
```
