# Predicting Federally Qualified Health Center Program Funding Levels

## Project Overview
Federally qualified health centers (FQHCs) are one of the most essential healthcare providers in the United States, providing care to over [30 million people](https://bphc.hrsa.gov/about-health-center-program/impact-health-center-program) each year, regardless of their ability to pay. As mission-driven organizations, FQHCs provide high-quality healthcare to uninsured and underserved individuals and families in every state. The Health Resource and Service Administration (HRSA), is that certifies eligibility and compliance for entities to become and maintain FQHC status by awarding Health Center Program funding several times a year. Becoming an FQHC is a long and complex process that requires certain procedures, organizational structures, and government funding experience as they receive a significant federal grant to become an FQHC. 

My aim with this project is to predict the estimated Health Center Program funding a new entity could secure given certain information about their patients, community, and organization structure. Health organizations could use this estimate to determine if it is worth going through the process to become a federally-qualified health center, especially to help offset any costs they incur by providing uncompensated care. 

## High Level Overview of Results
This project presents the culmination of an extensive data analysis and modeling project focused on predicting total health center funding based on various operational and demographic factors. The best-performing model, a Random Forest Regressor, achieved an R2 score of 0.648, demonstrating a strong ability to predict funding levels from complex and diverse inputs. This model notably outperformed others by effectively handling outliers and leveraging important features like total patients, operation hours, and uninsured ratios, which were pivotal in improving the prediction accuracy. The insights derived from this analysis are provide a data-driven foundation for decision-making that could inform strategic planning fo health organizations interested in applying for Health Center Program funding. The final report includes detailed evaluations of model performance, feature importance analyses, and residual examinations, all of which underscore the robustness and reliability of the predictive models developed.

## Reports
📝 [Technical Report](https://github.com/dezertdweller/capstone-project-fqhc-model/blob/main/reports/Predicting%20Federally%20Qualified%20Health%20Center%20Program%20Funding%20Levels.pdf)

📊 [Executive Presentation](https://github.com/dezertdweller/capstone-project-fqhc-model/blob/main/reports/Predicting%20Health%20Center%20Program%20Funding.pdf)

## Repo Organization
```
├── LICENSE
├── README.md          <- The top-level README for developers using this project.
├── data
│   ├── modeling       <- Final data for modeling.
│   ├── preprocessing  <- Intermediate data for preprocessing.
│   ├── eda            <- Intermediate data that has been cleaned.
│   ├── interim        <- Intermediate data that has been transformed.
│   └── raw            <- The original, immutable data dump.
│
├── assets             <- Exported images for reporting.
│
├── notebooks          <- Jupyter notebooks. 
│   ├── importing-data.ipynb
│   ├── data-wrangling.ipynb
│   ├── eda-visualization.ipynb
│   ├── pre-processing.ipynb
│   └── modeling.ipynb 
│
├── references          <- Data dictionaries, manuals, and all other explanatory materials.
│
├── reports
│   ├── Final_Rep_FQHC_Funding_Prediction.pdf
│   └── FQHC_Pred_Prez.pptx 
│                      
├── requirements.txt   <- The requirements file for reproducing the analysis environment, e.g.
│                         generated with `pip freeze > requirements.txt`
```
