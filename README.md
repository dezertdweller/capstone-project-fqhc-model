# Predicting Federally Qualified Health Center Program Funding Levels

## Project Overview
Federally qualified health centers (FQHCs) are one of the most essential healthcare providers in the United States, providing care to over [30 million people](https://bphc.hrsa.gov/about-health-center-program/impact-health-center-program) each year, regardless of their ability to pay. As mission-driven organizations, FQHCs provide high-quality healthcare to uninsured and underserved individuals and families in every state. The Health Resource and Service Administration (HRSA), is that certifies eligibility and compliance for entities to become and maintain FQHC status by awarding Health Center Program funding several times a year. Becoming an FQHC is a long and complex process that requires certain procedures, organizational structures, and government funding experience as they receive a significant federal grant to become an FQHC. 

My aim with this project is to predict the estimated Health Center Program funding a new entity could secure given certain information about their patients, community, and organization structure. Health organizations could use this estimate to determine if it is worth going through the process to become a federally-qualified health center, especially to help offset any costs they incur by providing uncompensated care. 

---
## Data
Currently funded FQHC are required to provide details about their organizations on an annual basis to HRSA through the Uniform Data System (UDS) report. Data is collected through an online portal and goes through several review processes to ensure no data errors are present. I used the most recently published data, which is from calendar year 2022. 

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
### 1. Cleaning Column Names & Subsetting Data
Talk about shape and size of each table, created a function to iterate through df dictionary to get details. Used this to subset data further into only critical columns.

Talk about renaming columns so that feature names made sense versus referring to table subsections.

### 2. Missing Values
The UDS report contains 3 different types of missing values that are Missing Not At Random (MNAR). Tables could have one or more of these MNAR types represented by the data entry options below:
1. "-" represents no data entry by health center
2. "--" represents suppressed patient counts between 1-15 to protect patient privacy
3. "---" represents suppressed health center confidential data

I created a function examine impact of each of these null types in each dataframe including the number of instances of each MNAR type and percent of the data missing in each column. I then dealt with the missing types differently based on the reason they were missing. My strategy was as follows:
1. For the "-" missing type, I replaced these with '0' since there was no data entry by health center. No entry would mean 0 for that field since it is possible to not have data to enter. For examle, the health center funding table had many instance of '-' for `ph_amount` which would be where an entity reports the amount of public housing health center funding they receive. Most health centers do not receive this subtype of health center funding, so they would leave it blank. The health_center site dataframe had this type of missing value for site geographic details, and I dropped these rows later because it could mean the site was not yet operational.
2. The "--" missing type was initially replaced with np.nan values. I decided to replace these values with a random number between 1 and 15 since the "--" was to suppress patient counts between 1-15 to protect patient privacy.
3. The "---" was also replaced with np.nan values. I kept these dataframes separate from the 3 dataframes with the "--" missing types so I could differentiate between the different missing types and impute them approporiately. Details about how I dealt with these missing values will be covered during the preprocessing section.

### 3. Creating Summary Data for Consolidation


---
## Exploratory Data Analysis
