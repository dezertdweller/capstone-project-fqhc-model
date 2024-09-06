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

---
## Data Cleaning and Wrangling
