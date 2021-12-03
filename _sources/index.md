# Project propsal

## Aim

The aim of this project is to allow tech employees in Canada to get a reasonable estimation of how much they will potentially earn given their skillset and years of experience. Fresh graduates and seasoned employees would benefit from an analysis tool that allows them to predic their earning potential. While the Human Resources (HR) department of companies have access to this market information, tech employees are mostly clueless about what their market value is. Therefore, a salary predictor tool could assist them in the negatiation process.

Using the [Stack Overflow Annual Developer Survey](https://insights.stackoverflow.com/survey) data, we did Exploratory Data analaysis (EDA) and came up with some features that we think are helpful in predicting the expected yearly compensation of tech employees. We describe the data, selected features, and the modelling process which you should be able to reproduce following the usage steps.

## Data

The data set used in this project is from the [Stack Overflow Annual Developer Survey](https://insights.stackoverflow.com/survey), which is conducted annually. The survey data set has nearly 80,000 responses. There are several useful features that could be extracted from this survey such as education level, location, the language used, job type, all of which are potentially associated with annual compensation.

## Usage

### Dependencies
- R version 3.6.1 and R packages:
  - tidyverse==1.3.1
  - caret==6.0.90
  - docopt==0.7.1
  - testthat=3.0.4

Suggested way to download data:

1. Clone this Github repository
2. Go to [Stack Overflow Annual Developer Survey](https://insights.stackoverflow.com/survey)
3. Copy the link of the csv file, taking 2019 result as an example:
   https://info.stackoverflowsolutions.com/rs/719-EMH-566/images/stack-overflow-developer-survey-2019.zip
4. Run the following command at the terminal from the root directory of this project:
```
# download data
python src/download_data.py --url=https://info.stackoverflowsolutions.com/rs/719-EMH-566/images/stack-overflow-developer-survey-2019.zip --out_dir=data/raw

# pre-process data
Rscript src/preprocessing.R --input=data/raw/survey_results_public.csv --out_dir=data/processed

# run eda report
python src/eda.py --train=data/processed/training.csv --out_dir=results/

# modelling
python src/salary_prediction_model.py --train=data/processed/training.csv --out_dir=results --test=data/processed/test.csv

# render report
jupyter-book build docs
```

## Report
The final report can be found [here](https://github.com/UBC-MDS/tech_salary_predictor_canada_us/tree/main/docs/index.html)

### Exploratory data analysis

After carrying out EDA, we realize that the following features might help us predict the salary of tech workers: years of experience, education level, programming lanaguages used, and their role (full-stack developer, front-end developer, etc.)

Based on the data corresponding to the five features in the survey, we can see that the most frequently used languages in developer's work are JavaScript, HTML/CSS, SQL, Python and Bash/Shell. And the five most frequent coding experience years of those developers who took the survey are 10,5,6,8,4 years respectively.  

### Modelling

We built a multiple linear regression model to see the relationship between these features and the annual compensation.
To simplify our analysis, we focused on Canada. We initially thought of building the model on Canada and USA data. However, we learned from our EDA that there is pay disparity and currency conversion involved. In the future, we plan to include USA in the model and handle those discrepancies.

Additionally, the regression model can also provide other important information such as the weight of each feature or how much each of those features we picked contributes to the overall predicted annual compensation.

The research question is:
> To predict the expected salary of a tech employee in Canada given their number years of experience, education level, programming lanaguages used, and their role

This topic is a predictive question, also extending to following sub-questions:
1. Which features have significantly statistical effect on the response (inference)?
2. Whether the model is robust (model estimation, a predictive problem)
3. The confidence interval of predictions (predictive)

## LICENSE
This database - The Public 2019 Stack Overflow Developer Survey Results - is made available under the Open Database License (ODbL): http://opendatacommons.org/licenses/odbl/1.0/. Any rights in individual contents of the database are licensed under the Database Contents License: http://opendatacommons.org/licenses/dbcl/1.0/