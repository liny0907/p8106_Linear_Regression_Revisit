Linear Regression: A Revisit
================
Lin Yang

``` r
library(RNHANES)
library(tidyverse)
library(summarytools)
library(leaps)
```

# Data

In this example, we assess the association between high density
lipoprotein (HDL) cholesterol and body mass index, blood pressure, and
other demographic factors (age, gender, race) using the NHANES data
(<https://wwwn.cdc.gov/nchs/nhanes/ContinuousNhanes/Default.aspx?BeginYear=2001>).
The data can be downloaded using functions in the package `RNHANES`.

``` r
dat <- nhanes_load_data(file_name = "l13_B", year = "2001-2002")

dat = dat %>% 
  left_join(nhanes_load_data("BMX_B", "2001-2002"), by="SEQN") %>%
  left_join(nhanes_load_data("BPX_B", "2001-2002"), by="SEQN") %>%
  left_join(nhanes_load_data("DEMO_B", "2001-2002"), by="SEQN")

dat = dat %>% 
  select(SEQN, RIAGENDR, RIDRETH1, RIDAGEYR, BMXBMI, BPXSY1, LBDHDL) %>%
  mutate(RIAGENDR = as_factor(RIAGENDR), RIDRETH1 = as_factor(RIDRETH1))

colnames(dat) <- c("ID", "gender", "race", "age", "bmi", "sbp", "hdl")

dat <- na.omit(dat)
```

Summary statistics of the predictors and the response:
