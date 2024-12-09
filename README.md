---
Title: "Wesley Life Analysis - Predicting Food Insecurity"
Author: "Aria Fisher"
Date: "12/09/2024"
Output: html_document
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
- CPS is individual level data with household information included. It covers food insecurity measures for a limited scope of counties in Iowa.
- ACS is Census data from the individual level with household information. It doesn't contain food insecurity measures but covers every county in Iowa.
Some .csv files and geographic shapefiles were used to visualize predictions by PUMA.

All of these source data files are already in the data subdirectory of the project, except for spm_pu_2022.sas7bdat, which needs to be externally added to your individual instance of the data subdirectory due to Git processing issues with the file size.

```r
list.files("data")
```

```
 [1] "cps_00005.csv"                  "cps_00006.csv"                 
 [3] "fsfoods_prediction.csv"         "fsstmp_prediction.csv"         
 [5] "fswrouty_prediction.csv"        "interim"                       
 [7] "iowa_seniors_by_puma.csv"       "single_senior_household.csv"   
 [9] "spm_pu_2022.sas7bdat"           "tl_2023_19_puma20"             
[11] "total_iowa_seniors_by_puma.csv"
```
To clean the data and obtain the files that are called in the model code, run clean_acs.R and clean_cps.R.
Each next file you run to reproduce the models and visualizations will contain this code:

```r
source("./code/clean_cps.R")
source("./code/clean_acs.R")
```
Those other files can then directly reference cps_data and acs_data, both of which have now been cleaned.

## Methods Explaination
The underlying structure of this project is based on exploring the application of a broad spectrum of models to a real-world dataset. For this reason, each food insecurity measure predicted in this project is approached with an MLE logistic regression, a logistic regression adjusted with Firth's Penalized Likelihood, a Random Forest and Random Tree, Lasso regression, and Ridge regression. Models are trained and tested on the CPS data set. The model with the highest AUC score is then selected for making predictions on the ACS data. Interaction terms and clustering are explored as a possible way to improve the models, but are not implemented in the final models as they fail to meaningfully improve the AUC score.

## Reproduce
1. Open the code directory and run `clean_cps.R` and `clean_acs.R`
   ```r
   list.files("code")
   ```
   
```
  [1] "clean_acs.R"                          
  [2] "clean_cps.R"                          
  [3] "cluster_model_testing.R"              
  [4] "combining_predictions.R"              
  [5] "fsfoods_analysis.R"                   
  [6] "fsstamp_analysis.R"                   
  [7] "FSWROUTY_variable.R"                  
  [8] "visualizations_and_general_analysis.R"
```
   
2. Run the following files in the code directory to conduct analysis and produce plots based on predicting each specified variable.
  * `FSWROUTY_variable.R` and corresponding `cluster_model_testing.R`
  * `fsfoods_analysis.R`
  * `fsstamp_analysis.R`
    
3.  Run the following files in the code directory to conduct aggregate analysis and produce further aggregate visualizations.
    Specifically, see the relationship between each predicted variable with each other and the senior (60+) population.
  * `visualizations_and_general_analysis.R`
  * `combining_predictions.R`

4. Visualizations for specific models are output by the specified variable files. Aggregate visualizations are saved for reference in the following directory:
   ```r
   list.files('figures')
   ```
  ```
   [1] "acs_elderly_population.png"            
   [2] "acs_elderly_proportion.png"            
   [3] "average_rank_of_insecurity_seniors.png"
   [4] "fsfoods_household_elderly.png"         
   [5] "FSSTMP_FSFOODS_ANALYSIS.png"           
   [6] "FSSTMP_FSWROUTY_ANALYSIS.png"          
   [7] "fsstmp_household_elderly.png"          
   [8] "FSWROUTY_FSFOODS_ANALYSIS.png"         
   [9] "number_of_seniors_by_puma.png"         
  [10] "number_on_seniors_fsstmp.png"          
  [11] "propotion_of_seniors_predicted.png"    
  [12] "test.png"                              
  [13] "test2.png"
  ```
