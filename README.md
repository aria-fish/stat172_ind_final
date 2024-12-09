---
title: "Wesley Life Analysis - Predicting Food Insecurity"
author: "Aria Fisher"
date: "12/09/2024"
output: html_document
---

## Introduction
This repository contains the code and data used to conduct an examination of senior food insecurity in Iowa at the PUMA (Public Use Microdata Area) level. The examination tests and compares a spectrum of models (General Linear Model, Random Forest, Lasso Regression, etc.) then visualizes the predictions of the best models on PUMA chloropleth maps. Models are trained on an Iowa subset of the IPUMS Current Population Survey (CPS).

## Requirements
To install the required R packages, run the following code in R:


```r
install.packages(c("tidyverse", "knitr", "logistf", "ggthemes", "glmnet",
                   "haven", "pROC", "RColorBrewer", "reshape2", "randomForest", "sf",
                    "rpart", "rpart.plot"))

```

## Data

The data used in for this project is generally split into CPS and ACS.
- CPS is individual level data with household information included for individuals. It covers food insecurity measures for a limited scope of counties in Iowa.
- ACS is Census data from the individual level with household information. It doesn't contain food insecurity measures but covers every county in Iowa.
Some .csv files and geographic shapefiles were used to visualize predictions by PUMA.

All of these source data files are already in the data subdirectory of the project, except for spm_pu_2022.sas7bdat, which needs to be externally added to your individual instance of the data subdirectory due to Git processing issues with the file size.

```r
list.files("data")
```

```
## [1] "cleaning.do"              "hillebrecht.csv"          "hillebrecht.dta"         
## [4] "hillebrecht(missing).csv" "hillebrecht(missing).dta" "variables.csv"
```

```r
list.files("Data/Indonesia/Cleaning/")
```

```
##  [1] "alatas.csv"                               
##  [2] "alatas.dta"                               
##  [3] "alatas(missing).csv"                      
##  [4] "alatas(missing).dta"                      
##  [5] "cleaning.do"                              
##  [6] "FAO Dietary Diversity Guidelines 2011.pdf"
##  [7] "food.dta"                                 
##  [8] "notes.docx"                               
##  [9] "ranks.dta"                                
## [10] "variables.csv"                            
## [11] "xvars.dta"
```

The data files that will be called are "hillebrecht.csv" and "alatas.csv".

## Reproduce
1. Run `run_simulations.R` to reproduce error rate results and coefficient estimate results. 
  *  Indonesia Analysis/all_results.csv
  *  Indonesia Analysis/all_coef.csv
  *  Indonesia Analysis/coef_total_sample.csv
  *  Indonesia Analysis/CB_beta_rank_CI_noelite.csv
  *  Indonesia Analysis/CB_beta_rank_CI.csv
  *  Burkina Faso Analysis/all_results.csv
  *  Burkina Faso Analysis/all_coef.csv
  *  Burkina Faso Analysis/coef_total_sample.csv
  *  Burkina Faso Analysis/CB_beta_rank_CI_noelite.csv
  *  Burkina Faso Analysis/CB_beta_rank_CI.csv
  
The above files can be used to generate plots found in the manuscript:
  
2. Run `Burkina Faso Analysis/make_plots.R` to reproduce error rate plots and coefficient plots for the Burkina Faso data. 
  *  Burkina Faso Analysis/coef_score_EC_hillebrecht.pdf
  *  Burkina Faso Analysis/coef_score_hillebrecht.pdf (Figure 1)
  *  Burkina Faso Analysis/ER_hybrid_AI.pdf (Figure 7 a)
  *  Burkina Faso Analysis/ER_hybrid_DU.pdf (Figure 8)
  *  Burkina Faso Analysis/ER_hybrid.pdf (Figure 3 a)
3. Run `Indonesia Analysis/make_plots.R` to reproduce error rate plots and coefficient plots for the Indonesia data. 
  *  Indonesia Analysis/coef_score_EC_hillebrecht.pdf (Figure 5)
  *  Indonesia Analysis/coef_score_hillebrecht.pdf (Figure 2)
  *  Indonesia Analysis/ER_hybrid_AI.pdf (Figure 7 b)
  *  Indonesia Analysis/ER_hybrid_EC.pdf (Figure 6)
  *  Indonesia Analysis/ER_hybrid.pdf (Figure 3 b)
4. Run `Burkina Faso Analysis/run_mcmc_weights.R` to reproduce heterogeneous ranker results. 
  *  Burkina Faso Analysis/heter_weights_omega.pdf (Figure 4 a)
  *  Burkina Faso Analysis/heter_weights_corr.pdf (Figure 4 b)
## References

Alatas,   V.,   Banerjee,   A.,   Hanna,   R.,   Olken,   B.,   and  Tobias,   J.  (2013).Targeting  the  poor:   Evidence  from  a  field  experiment  in  Indonesia.Harvard  Dataverse,https://doi.org/10.7910/DVN/M7SKQZ, V5.

Hillebrecht,  M.,  Klonner,  S.,  Pacere,  N.  A.,  and  Souares,  A.  (2020b).   Community-basedversus statistical targeting of anti-poverty programs: Evidence from Burkina Faso.Journalof African Economies, 29(3):271–305
