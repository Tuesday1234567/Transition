# First lifetime transition from suicidal ideation to attempt after university entrance: Findings from the World Mental Health International College Student Initiative: main analysis

`main_analysis.Rmd` contains the main regression analyses for transition from lifetime suicidal ideation at baseline to a first suicide attempt during 12-month follow-up.

 Data loading and cleaning, local file paths, and spreadsheet/table-generation sections have been removed. Model formulas, 
 sample selection, survey design, imputation pooling and BH calculations are retained. 


## Analysis overview

1. Within each imputation, restrict the follow-up dataset to `T0_LT_suic_idea == 1` and `T0_LT_suic_attempt == 0`. 
2. Fit survey-weighted quasipoisson generalized linear models with a log link for outcome `T1_suic_attempt_12M`.
3. Specify the survey design with `ids = ~strata`, `strata = ~country`, `weights = ~followup1_norm_weight` and `nest = TRUE`. Use `survey.lonely.psu = "adjust"`.
4. Pool log risk ratios and their covariance matrices using Rubin's rules, then exponentiate to obtain risk ratios and ordinary 95% confidence intervals.
The source implementation uses Rubin's original degrees-of-freedom formula and retains raw pooled p-values at full precision for BH correction.
5. Apply the four separate Benjamini-Hochberg (BH) procedures described below.

All models include COVID-19 period, assessment timing and country. The four sociodemographic predictors are entered together in their base model. 
Each adjusted individual disorder/STB model includes one additional focal predictor alongside all four sociodemographics and the common controls. 
Disorder count is entered as one categorical predictor, producing three contrasts against zero disorders.

## tested models and code objects

| Analysis | Code object | Predictors in addition to the common controls |
| --- | --- | --- |
| Sociodemographic base model | `m1` | Four sociodemographic predictors |
| multivariable Model 1 | `m3` | Four sociodemographics and eight STB characteristics |
| multivariable Model 2 | `biv_results[["md_cat"]]` | Four sociodemographics and categorical disorder count |
| multivariable Model 3 | `m5_num` | Four sociodemographics, eight STB characteristics and categorical disorder count |
| type-only candidate | `m5_type` | Four sociodemographics, eight STB characteristics and eight disorder types |
| type-and-number candidate | `m5` | Four sociodemographics, eight STB characteristics, eight disorder types, `md_2` and `md_3plus` |
| Additional model | `m4` | Four sociodemographics, eight disorder types and `md_cat_ref1` |
| Adjusted bivariate individual disorder/STB models | `biv_results[[variable_name]]` | Four sociodemographics and one focal disorder/STB predictor |


## Multiple-testing correction


COVID-19 period, assessment timing and country coefficients are excluded from the correction families. 
In the adjusted individual disorder/STB models, only the focal coefficient contributes; 



