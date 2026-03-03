# Predicting Mortgage Prepayment

With new Federal Reserve Board members onboarding, the US is likely entering a period of a dove-ish approach to monetary policy and lower interest rates. Declining mortgage rates will likely lead to an increase in rate-refinances for mortgages. Predicting mortgage refinance rates will have important implications for the multi-trillion MBS market. 
Given the very low interest rates during 2020 and 2021 and higher rates since, there has been less rate-driven refinances than the historic average. Less refinance activity has contributed to lower volumes overall, as shown below. Figure 1 shows the average interest rate on 30 year mortgages and the quarterly mortgage origination volume in billions of dollars. 
![App Performance Chart](./images/chart.png) #change file name

## Data
This folder contains a model for mortgage prepayment calibrated with monthly single-family loan-level data from 2014 Q1. I pick this period because it was one of the most recent vintages to experience several periods of declining mortgage interest rates. In addition, it offers the ability to examine several years of mortgage performance previous to the extraordinary economic environment during the COVID crisis. 
The model features both static variables known at origination and dynamic variables which change monthly. Table 1 shows the dynamic variables.

|Variable Name| Description|
|Refi Benefit Now| The magnitude of the financial benefit from refinancing, expressed as the Net Present Value (NPV) assuming the loan term stays the same and there are no transaction costs. I use the lowest mortgage interest rate in the last three months to calculate the NPV.
|Ref Benefit Past| There is a phenomenon known as 'woodheaded borrowers' in mortgage finance.See Deng and Quigley, 2004) Such borrowers have not refinanced in the past when it would have been financially beneficial. When borrowers have missed opportunities in the past, they are more likely to do so in the future. This variable is NPV from refinancing in the period before the last three months, using the lowest interest rate observed since five months after origination. |
|HPI growth| When the collateral appreciates in value, it becomes easier to refinance. (See Mattey and Wallace, 2001). This variable measures the magnitude of home price appreciation for the area (MSA where available, otherwise state) using the FHFA home price index. 

Table 2 shows the static variables.

|Variable Name| Description|
|Loan Age| Months since loan origination|
|OLTV| Loan-to-value ratio at origination|
|CSCORE_MAX| The maximum of the borrower and, where applicable, co-borrower credit score|
|coop_condo_dummy| An indicator variable for whether the collateral is a coop or cooperative unit|
|SATO| Spread at time of origination, the difference between the average mortgage rate and the loan's contract rate. High SATO's often are a sign of borrowers who are relatively uneducated about mortgage markets, inattentive to financial decisionmaking, or have some other characteristic which makes them a less appealing borrower|

## Methodology

I use two methods for estimating this model. The first is a complementary log-log regression, which is known as the discrete-time version of a Cox proportional hazard model. It is similar to logistic regression but controls for the right-censoring of data from when loans prepay. The second is XGBoost, with a grid search to find optimal hyperparameters. 

