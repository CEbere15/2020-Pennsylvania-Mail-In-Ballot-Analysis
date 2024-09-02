# 2020 Pennsylvania General Election Mail Ballot Data Analysis
### Table of Contents
- [Overview](#Overview)
  - [Summary](#Summary)
  - [Data Source](#Data-Source)
  - [Tools](#Tools)
  - [Data Dictionary](#Data-Dictionary)
  - [Objectives](#Objectives)
  - [References](#References)
- [Data Cleaning and Manipulation](#data-cleaning-and-preparation)
- [Exploratory Data Analysis](#exploratory-data-analysis)
  - [Descriptive Statistics](#Descriptive-Statistics)
- [Data Analysis](#data-analysis)
- [Results](#results)
- [License](#license)


## Overview

### Summary
This data analysis project aims to analyze and draw insights into the Pennsylvanian mail ballot application requests made for the 2020 General Election. Requests that were recorded by the Pennsylvanian Department of State. By doing so, we seek to get a better understanding of the trends brought from these requests, and how they are affected by different aspects of the people making them and where they live. 

### Objectives 
- Find whether party registration of the requester has any affect on how long it takes for their mail ballot to be received
- Find whether the county, congressional or state district that the person who is requesting a mail in ballot affects how long it takes for their request to be processed
- Find whether age and the requester's generation has an affect on whether a person chooses to make their request online, overseas, by mail or on paper
- Find whether a specific party made requests for mail ballots more than others did  

### Data Source
### Tools

### Data Dictionary
| Column       | Data Type       | Description                                                                      |
|-------------------|-------------|-----------------------------------------------------------------------------|
| `County Name`      | Text     | The county in which the applicant resides in when submitting their ballot.        |
| `Applicant Party Designation`    | 	Text | The party that the applicant is associated with.     |
| `Date of Birth`| Datetime| Date of the voter's birth; those with the birthday 1/1/1800 are given that date for confidentiality purposes.|
|`Mail Application Type`| Text | Identifies the type of mail ballot requested.|
| `Application Approved Date`      | Datetime     | The date that the application was submitted by the voter.  |
| `Application Return Date`| Datetime | The date that the application was processed by their county's election office.|
| `Ballot Mailed Date` | Datetime     | The date in which the county confirmed the application to queue a ballot label to mail the ballot materials to the voter.           |
| `Ballot Returned Date`      | Datetime     | The date the county marked the ballot as received after the voter mailed it back to the county.|
| `State House District`      | Text     | The Pennsylvania House of Representatives district in which the voter is located.|
| `State Senate District`      | Text     | The Pennsylvania Senate district in which the voter is located.|
| `Congressional District`      | Text     | The Congressional District district in which the voter is located.|
### Further Definitions



#### Mail Application Types
- **MAILIN**: A mail-in ballot application
- **OLMAILV**: A mail ballot application that was submitted online
- **OLREGV**: A civilian absentee ballot application that was submitted online
- **CVO**: An absentee ballot application for an overseas civilian voter
- **ALT**: An alternative ballot application where voters who are 65 years of age and the polling place may not be fully accessible
- **BV**: Bedridden Veteran
- **C**: An absentee ballot application issued during the emergency absentee period
- **CIV**: A civilian absentee ballot application that was submitted via paper
- **CRI**: An absentee ballot application for an overseas civilian voter in a remote/isolated location
- **F**: An absentee application for an individual who qualifies to vote for federal offices in federal election years
- **M**: An absentee ballot application for a military voter
- **MRI**: An absentee ballot application for a military voter in a remote/isolated location
- **OLMAILNV**: Online Mail-In Ballot Application (Not Verified)
- **OLREGNV**: Online Regular Absentee Ballot Application (Not Verified)
- **PER**: An absentee ballot application where a voter has requested permanent status
- **PMI**: A mail ballot application where the voter has requested permanent status
- **REG**: An absentee ballot application that was submitted via paper
- **V**: Veteran (Not Verified)
- **BVRI**: Bedridden Veteran - Remote/Isolated 

### References
- 

## Data Cleaning and Preparation
### Date Parsing
#### Handling Outliers

### Calculating Ages


### Generation Assignment




## Exploratory Data Analysis
### Descriptive Statistics


### Distribution Analysis



### Time Series Analysis

### Party Prevalence Analysis 
## Data Analysis
### Voter Turnout Analysis


### Application Process Efficiency Analysis

### Time Interval Analysis

#### Partywise Time Interval Analysis
### Age and Voting Behavior Analysis

### Generational Voting Patterns
### Application Type Analysis


### Geospatial Analysis

### Trend Analysis


## Results


## Licenses
