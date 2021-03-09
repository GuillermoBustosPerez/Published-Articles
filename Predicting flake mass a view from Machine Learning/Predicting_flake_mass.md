Predicting Flake Mass: A View from Machine Learning. Lithic Technology
================
Guillermo Bustos-Pérez
7/3/2021

## Contents

1.  Load packages, read and check data

2.  Descriptive statistics of experimental assemblage  
    2.1) Descriptive statistics  
    2.2) Bagolini scatter plot  
    2.3) Calculation of new variables

3.  Multiple linear regression and best subset selection  
    3.1) Best subset selection  
    3.2) Number of variables  
    3.3) Variable selection

## 1) Load packages, read and check data

The data is available in this repository as a **.csv** file. Please note
that original data uses **“,”** as decimal marker instead of using
**“.”**. Thus, it is required to use the function **read\_csv2**. Data
manipulation and visualization are performed using package **tidyverse**
(Wickham et al., 2019).

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

## 2) Descriptive statistics of experimental assemblage

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

### 2.1) Descriptive statistics

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

 

### 2.2) Bagolini scatter plot

A Bagolini (1968) scatter plot can be a helpful way to visualize the
data.

``` r
# Bagolini scatter plot
Reg_Data %>% 
  ggplot(aes(Width, Length)) +
  geom_segment(x = 40, y = 0, xend = 0, yend = 40, color = "gray48") +
  geom_segment(x = 60, y = 0, xend = 0, yend = 60, color = "gray48") +
  geom_segment(x = 80, y = 0, xend = 0, yend = 80, color = "gray48") +
  
  geom_segment(x = 0, y = 0, xend = 90, yend = 90, color = "gray48") +
  
  geom_segment(x = 0, y = 0, xend = (90/6), yend = 90, color = "gray48") +
  geom_segment(x = 0, y = 0, xend = (90/3), yend = 90, color = "gray48") +
  geom_segment(x = 0, y = 0, xend = (90/2), yend = 90, color = "gray48") +
  geom_segment(x = 0, y = 0, xend = (90/1.5), yend = 90, color = "gray48") +
  geom_segment(x = 0, y = 0, xend = (90/0.75), yend = 90, color = "gray48") +
  geom_segment(x = 0, y = 0, xend = (90/0.5), yend = 90, color = "gray48") +
  geom_segment(x = 0, y = 0, xend = 90, yend = (90/2), color = "gray48") +
  
  annotate("text", x = 0, y = 89, adj = 0, 
           label = "Very thin blade", size = 2.5) +
  annotate("text", x = 17, y = 89, adj = 0, 
           label = "Thin blade", size = 2.5) +
  annotate("text", x = 33, y = 89, adj = 0, 
           label = "Blade", size = 2.5) +
  annotate("text", x = 44, y = 89, adj = 0, 
           label = "Elongated flake", size = 2.5) +
  annotate("text", x = 72, y = 89, adj = 0, 
           label = "Flake", size = 2.5) +
  annotate("text", x = 88, y = 80, adj = 0, 
           label = "Wide\nflake", size = 2.5) +
  annotate("text", x = 88, y = 53, adj = 0, 
           label = "Very\nwide\nflake", size = 2.5) +
  annotate("text", x = 88, y = 25, adj = 0, 
           label = "Wider\nflake", size = 2.5) +
  
  annotate("text", x = 20, y = 1, adj = 0, 
           label = "Micro", size = 2.5) +
  annotate("text", x = 47, y = 1, adj = 0, 
           label = "Small", size = 2.5) +
  annotate("text", x = 65, y = 1, adj = 0, 
           label = "Normal", size = 2.5) +
  annotate("text", x = 85, y = 1, adj = 0, 
           label = "Big", size = 2.5) +
  
  geom_point(size = 2) +
  scale_x_continuous(breaks = seq(0, 90, 5), lim = c(0, 90)) +
  scale_y_continuous(breaks = seq(0, 90, 5), lim = c(0, 90)) +
  ylab("Length (mm)") +
  xlab("Width (mm)") +
  theme_light() +
  labs(color = "") +
  theme(axis.title = element_text(size = 9, color = "black", face = "bold"),
        axis.text = element_text(size = 8, color = "black"),
        legend.position = "bottom") +
  coord_fixed() 
```

![](Predicting_flake_mass_files/figure-gfm/unnamed-chunk-8-1.png)<!-- -->

### 2.3) Calculation of new variables

Previous studies have shown that it is easier to predict log of flake
mass using log of platform size (Braun et al., 2008; Clarkson & Hiscock,
2011; Shott et al., 2000). Following this line of approach, logarithmic
transformations of all variables were included in the dataset, and the
target variable was the logarithmic transformation of flake weight. In
the present study, all logarithmic transformations refer to the common
logarithm (base 10). Log transformations of variables are common, since
they avoid negative results (necessary in the case of predicting flake
weight), reduce skewed distributions, and can approximate parametric
distributions (which favors the inferential power of models).

Log10 transformation of variables and original variables are placed into
a new data frame. Variables of length and width are removed since they
would be altered by retouch.

``` r
# Calculate log10 transformations of variables and 
# place into new dataset
Reg_Data_2 <- Reg_Data %>% 
  mutate(Log_Weight = log10(Weight),
         Log_Max_Thick = log10(Max_Thick),
         Log_Thick = log10(Mean_Thick),
         Log_SD_Thick = log10(SD_Thick),
         Log_Plat = log10(Surf_Plat),
         Log_Plat_2 = log10(Surf_Plat_II),
         Log_Plat_De = log10(Plat_Depth),
         Log_EPA = log10(EPA),
         Log_IPA = log10(IPA)) %>% 
  select(-c(Length, Width, Weight,
            Surf_Plat, Surf_Plat_II))
```

## 3) Multiple linear regression and best subset selection

Best subset selection of variables (Furnival & Wilson, 1974; Hastie et
al., 2009; Hocking & Leslie, 1967) fits separate regression models for
each of the possible combination of variables, the total number of
models being equal to 2<sup>*p*</sup> (p being the number of predictors)

``` r
# number of models fitted
2^(ncol(Reg_Data_2)-1)
```

    ## [1] 262144

### 3.1) Best subset selection

Best subset selection is performed using the package **“leaps”** (Lumley
based on Fortran code by Alan Miller, 2020), “caret” (Kuhn, 2008), and
“broom” (Robinson, 2014).

``` r
# Load package
library(leaps)

# Perform best subset
regfit_full <- regsubsets(Log_Weight ~., 
                          data = Reg_Data_2,
                          nvmax = 18)
reg_summary <- summary(regfit_full)
```

### 3.2 Number of variables

Selection of number of variables is evaluated using two parameters:
Mallows’s Cp and adjusted *r*<sup>2</sup>. Mallows’s Cp (Mallows, 1973)
accounts for model fit in the process of model selection – a low value
indicates a good model. The addition of predictors results in an
increasing *r*<sup>2</sup> (proportion of variance of the predicted
variable explained by the independent variables) irrespective of
predictor contribution to the model and making it impossible to compare
models with a different number of predictors.

``` r
# Plot cp and adjusted rsquared according to n predictors
data.frame(reg_summary[4], 
           reg_summary[5],
           Predictors  = seq(1, 18, 1)) %>% 
  pivot_longer(c(adjr2, cp),
               names_to = "Parameters",
               values_to = "Estimation") %>% 
  ggplot(aes(Predictors, Estimation, color = Parameters)) +
    scale_x_continuous(breaks = seq(0, 18, 2), lim = c(1, 18)) +
  geom_line() +
  geom_point() +
  ggsci::scale_color_aaas() +
  theme_light() +
  facet_wrap(~Parameters, scales = "free") +
  theme(legend.position = "none")
```

![](Predicting_flake_mass_files/figure-gfm/unnamed-chunk-12-1.png)<!-- -->
Therefore, combined data from adjusted *r*<sup>2</sup> and
*C*<sub>*p*</sub> indicate that (given the employed variables) the
optimal model should have between six and eight explanatory variables.

### 3.3 Variable selection

Evaluation of predictor selection and stability following the
calculation of Cp and adjusted r2 values allows for refinement of the
model (Figure 3).

``` r
# Get variable stability using adjusted r2 and Cp ####
par(mfrow  = c(1,2))
plot(regfit_full, scale = "adjr2")
plot(regfit_full, scale = "Cp")
```

![](Predicting_flake_mass_files/figure-gfm/unnamed-chunk-13-1.png)<!-- -->

Function to get best variables:

``` r
# Function to get formula

get_model_formula <- function(id, object, outcome){
  # get models data
  models <- summary(object)$which[id,-1]
  # Get outcome variable
  #form <- as.formula(object$call[[2]])
  #outcome <- all.vars(form)[1]
  # Get model predictors
  predictors <- names(which(models == TRUE))
  predictors <- paste(predictors, collapse = "+")
  # Build model formula
  as.formula(paste0(outcome, "~", predictors))
}

# Get best variables
get_model_formula(7, regfit_full, "Log_Weight")
```

    ## Log_Weight ~ Mean_Thick + Cortex + No_Scars + EPA + Log_Max_Thick + 
    ##     Log_Plat + Log_Plat_De
    ## <environment: 0x0000000015729028>

## References
