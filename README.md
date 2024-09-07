# Predicting Federally Qualified Health Center Program Funding Levels

## Project Overview
Federally qualified health centers (FQHCs) are one of the most essential healthcare providers in the United States, providing care to over [30 million people](https://bphc.hrsa.gov/about-health-center-program/impact-health-center-program) each year, regardless of their ability to pay. As mission-driven organizations, FQHCs provide high-quality healthcare to uninsured and underserved individuals and families in every state. The Health Resource and Service Administration (HRSA), is that certifies eligibility and compliance for entities to become and maintain FQHC status by awarding Health Center Program funding several times a year. Becoming an FQHC is a long and complex process that requires certain procedures, organizational structures, and government funding experience as they receive a significant federal grant to become an FQHC. 

My aim with this project is to predict the estimated Health Center Program funding a new entity could secure given certain information about their patients, community, and organization structure. Health organizations could use this estimate to determine if it is worth going through the process to become a federally-qualified health center, especially to help offset any costs they incur by providing uncompensated care. 

## High Level Overview of Results


## Repo Organization


---
## Data
Currently funded FQHCs are required to provide details about their organizations on an annual basis to HRSA through the Uniform Data System (UDS) report. Data is collected through an online portal and goes through several review processes to ensure no data errors are present. I used the most recently published data, which is from calendar year 2022. 

To view the data source, data reporting requirements and definitions, or other available information about FQHCs, visit this links below:

* [Health Center Program Annual UDS Data](https://www.hrsa.gov/foia/electronic-reading)
* [2022 UDS Manual](https://bphc.hrsa.gov/sites/default/files/bphc/data-reporting/2022-uds-manual.pdf)
* [National Awardee Data](https://data.hrsa.gov/tools/data-reporting/program-data/national)

The UDS report collects thousands of measures about an organization's operations, including patient demographic details, staffing and utilization, costs of care, operating hours and locations, clinincal outcomes and diagnosis details, financial information (funding, insurance, revenue, expenses, etc.).

In order to reduce the size of the dataset for this project, I met with Eric Battis, a former Chief Financial Officer of an FQHC in Arizona, to determine what data is most critical to understand a health center's operations. The list of table I included for the scope of this project include the following:
* *Health Center Info:* Health center name, address, grant number, and project director.
* *Health Center Site Info:* Health center operating and administrative locations.
* *Table 9E:* Non-patient generate revenue including grant and other revenue.
* *Health Center Zip Codes:* Health center operational locations and number of patients served.
* *Table 8A:* Costs of providing care. 
* *Table 5:* Personnel, staffing utilization, and total visits by area.
* *Tables 3A and 3B:* Patient demographic details
* *Table 4:* Other patient characteristics including insurance and special populations.
* *Table 9D:* Revenue generated from patient services.

I imported each of these tables as a seperate dataframe. Each was processed with various cleaning and wrangling steps before consolidating data into one dataframe for visualization, preprocessing and modeling. 

---
## Data Cleaning and Wrangling
**Notebook Links:s**
* [Data Cleaning](https://github.com/dezertdweller/capstone-project-fqhc-model/blob/main/notebooks/importing-data.ipynb)
* [Data Wrangling](https://github.com/dezertdweller/capstone-project-fqhc-model/blob/main/notebooks/data-wrangling.ipynb)

### 1. Cleaning Column Names & Subsetting Data
After importing each dataset as a dataframe, I created a function to iterate over a dataframe dictionary to print out the shape of dataframe and the column names. I needed to review these to determine which columns would be relevant based on my meeting with Eric. 

The nine tables had over 650 columns combined. Additionally, several column names were exceedingly long. For example, one column was called `Medicare (Inclusive of dually eligible and other Title XVIII beneficiaries)-0-17 years old (a)` and I renamed it `Medicare 0-17`. I subsetted each dataframe manually by determining which columns to keep for further analysis. I then renamed columns to keep their details but not be as long. 

### 2. Missing Values
The UDS report contains 3 different types of missing values that are Missing Not At Random (MNAR). Tables could have one or more of these MNAR types represented by the data entry options below:
1. "-" represents no data entry by health center
2. "--" represents suppressed patient counts between 1-15 to protect patient privacy
3. "---" represents suppressed health center confidential data

I created a function examine impact of each of these null types in each dataframe including the number of instances of each MNAR type and percent of the data missing in each column. I then dealt with the missing types differently based on the reason they were missing. Below is an example output from the function `find_missing_values()`.

![finding-missing-values](https://github.com/user-attachments/assets/a4b2b66e-fe0c-4419-8a18-61fc4a3e1007)

My strategy for dealing with each missing type was as follows:
1. For the "-" missing type, I replaced these with '0' since there was no data entry by health center. No entry would mean 0 for that field since it is possible to not have data to enter. For examle, the health center funding table had many instance of '-' for `ph_amount` which would be where an entity reports the amount of public housing health center funding they receive. Most health centers do not receive this subtype of health center funding, so they would leave it blank. The health_center site dataframe had this type of missing value for site geographic details, and I dropped these rows later because it could mean the site was not yet operational.
2. The "--" missing type was initially replaced with np.nan values. I decided to replace these values with a random number between 1 and 15 since the "--" was to suppress patient counts between 1-15 to protect patient privacy.
3. The "---" was also replaced with np.nan values. I kept these dataframes separate from the 3 dataframes with the "--" missing types so I could differentiate between the different missing types and impute them approporiately. Details about how I dealt with these missing values will be covered during the preprocessing section.

After dealing with the missing values, I consolidated the initial nine dataframes into 4 dataframes for furhter wrangling.

### 3. Creating Summary Data for Consolidation
Two dataframes consisted of several rows of data for each health center. The `Service Area` dataframe consisted of nearly 15,000 rows of data and the `Service Sites` dataframe had approximately 97,100 rows of data. I decided to create aggregate information from these tables to add to the main `Health Centers` dataframe which had details about each entity including the target variable `total_hc_funding`. The new summary data that was added to the `Health Centers` dataframe included zip code counts for each entity representing their total service area, the count of health center sites each entity manages, the total weekly hours of operation across sites for each entity, and the count of cities each entity operated in. I then merged this new aggregated dataframe to the `Health Centers` data.

After imputing random numbers raning from 1-15 for the MNAR type "--", I consolidated the `Health Center Ops Finance` dataframe with `Health Centers`. 

---
## Exploratory Data Analysis
**Notebook Link:**
* [Exploratory Data Analysis](https://github.com/dezertdweller/capstone-project-fqhc-model/blob/main/notebooks/eda-visualization.ipynb)

During EDA, my primary goal was to look at the distribution of the target variable, `total_hc_funding` and how it interacted with other features. The target variable has a wide distribution, ranging from $275,778 to  $22,382,349. Most health centers between receive 1.8M and 4.6M of health center funding annually, with the mean funding level just over 3.6 million.  These esepcially large outliers were not due to errors, some entities do in fact receive nearly 80x the amount of funding as other entities. There are similar distributions with the total number of patients served.  

![hcp-funding-distribution](https://github.com/dezertdweller/capstone-project-fqhc-model/blob/main/assets/hcp-funding-distribution.png)

Total patients has a strong positive correlation with total HCP funding, demonstrated with the Pearson's correlation coefficient of 0.730.

I wanted to test several hypotheses with how different features affect funding. 

**Rural vs. Urban**: Urban providers represent 58.7% of the total number of entities and they receive 64.1% of the total available health center funding. Rural providers represent 41.3% of the total number of providers and receive 35.9% of the funding. 

*Do urban providers receive more funding than rural providers after controlling for patient volume?* I conducted an ANCOVA statistical test to isolate the effect of the categorical variable (Urban or Rural) on the dependent variable (total health center funding). I want to control for patient volume because the total number of patients is highly correlated with the total health center funding. After controlling for patient counts, there is not a statistically significant difference in the total health center funding Urban providers receive compared to Rural providers. 

**Other Non-patient Revenue**:
The larger health centers become, the more diversified their revenue streams appear. As some health centers increase in patient size, they seem to be more likely to have larger amounts of funding coming from other non-patient and non-grant revenue streams. However, we did see in the table earlier that this is not always the case. Some health centers that have massive amounts of total other revenue, like East Boston Neighborhood Health Center only serve a small number of patients, just over 80,000 in their case. 

*Do health centers with more total other revenue receive more HCP funding compared to health centers with less other revenue?* I conducted an ANCOVA statistical test and controlled for patient size. I also conducted a Logit Regression test to compare HCP funding between entities broken up into 4 quantiles. Both tests demonstrate that health centers that have higher other non-patient non-grant revenue have more total HCP funding. Of note however is that other revenue only explains 6% of the variance in HCP funding.

![other-rev-categry](https://github.com/dezertdweller/capstone-project-fqhc-model/blob/main/assets/other-rev-category.png)


**SDOH Proportions**:

---
## Pre-Processing


---
## Modeling


---
## Results


---
## Future Improvements
Including other details about entities (type (gov, hospital, nonprofit), region, distance to US-Mex border, )
Including state-level data (% uninusred, total population, % low-income)

---
## Credits & Thanks 🤗


