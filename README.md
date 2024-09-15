# 2020 Pennsylvania General Election Mail Ballot Data Analysis
### Table of Contents
- [Overview](#Overview)
  - [Summary](#Summary)
  - [Data Source](#Data-Source)
  - [Tools](#Tools)
  - [Data Dictionary](#Data-Dictionary)
  - [Objectives](#Objectives)
  - [References](#References)
- [Data Cleaning and Manipulation](#data-cleaning-and-manipulation)
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
### Tools
- DB Browser (SQLite) - Data Cleaning, Exploration, Analysis and Manipulation
  - Utilized to process the data and enhance it so it is in a better format for visualization and further analysis.
- Excel - Data Preparation
  - Utitlized to prepare the data in supplementary tables to be used for analyzing the dataset.
- Tableau - Data Visualization
  - Utilized to create visualizations and interactive dashboards to help others have a firmer understanding of the data.
- Python - Data Visualization and Analysis
  - Utilized to create more complex visualizations that Tableau is less able to create, and to perform statistical and complex data analyses.
### References
- 
## Data Source
2020 General Election Mail Ballot Requests: The primary dataset being analyzed can be found in the '.csv' file. It contains data on the mail ballot requester's county residence, their party designation, date of birth, the type of mail ballot their request is for, the day it is submitted, processed, along with when the ballot is sent and received, and which legislative districts they reside in. These results allow for an analysis that can focus on the age of the voter requesting a mail ballot, their party, the time it takes for it to be processed, and their geographical location.

The dataset can be found in Pennsylvania's Open Data Portal, [here](https://data.pa.gov/Government-Efficiency-Citizen-Engagement/2020-General-Election-Mail-Ballot-Requests-Departm/mcba-yywm/about_data).

County, Congressional, State Senate, State House Supplementary: A group of supplementary tables I made for getting further insights on how geographical results from the 2020 General Election could have an effect on the amount of mail ballot requests each party made. 

### Data Dictionary
#### 2020 General Election Mail Ballot Requests
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



## Data Cleaning and Manipulation
### Renaming the Dataset
For the sake of simplifying our SQL queries, we should rename the dataset into something shorter instead.
```sql
-- Renaming the dataset to a shorter name
ALTER TABLE [2020_General_Election_Mail_Ballot_Requests_Department_of_State_20240914] RENAME TO GeneralBallots;
```



</br>

### Date Parsing
In order to better analyze the dates in SQL, we must first change the format of the dataes from MM/DD/YYYY into YYYY-MM-DD. Using the Subscript function in SQL, we can rearrange the numbers so that this is the case.

```sql
-- Modfiying all the date columns from MM/DD/YYYY to YYYY-MM-DD.
UPDATE GeneralBallots
SET 
    DateofBirth = SUBSTR(DateofBirth, 7, 4) || '-' || SUBSTR(DateofBirth, 1, 2) || '-' || SUBSTR(DateofBirth, 4, 2),
    ApplicationApprovedDate = SUBSTR(ApplicationApprovedDate, 7, 4) || '-' || SUBSTR(ApplicationApprovedDate, 1, 2) || '-' || SUBSTR(ApplicationApprovedDate, 4, 2),
    ApplicationReturnDate = SUBSTR(ApplicationReturnDate, 7, 4) || '-' || SUBSTR(ApplicationReturnDate, 1, 2) || '-' || SUBSTR(ApplicationReturnDate, 4, 2),
    BallotMailedDate = SUBSTR(BallotMailedDate, 7, 4) || '-' || SUBSTR(BallotMailedDate, 1, 2) || '-' || SUBSTR(BallotMailedDate, 4, 2),
    BallotReturnedDate = SUBSTR(BallotReturnedDate, 7, 4) || '-' || SUBSTR(BallotReturnedDate, 1, 2) || '-' || SUBSTR(BallotReturnedDate, 4, 2);

```

</br>

Now we should check to make sure that it worked.

```sql
-- Sampling the date columns to see that the columns were changed correctly.
Select DateofBirth, ApplicationApprovedDate, ApplicationReturnDate, BallotMailedDate, BallotReturnedDate 
from GeneralBallots
limit 5;
```

</br>


![Changed Columns](https://github.com/user-attachments/assets/6c5e7051-24cc-415a-ba3f-6475b584f320)

</br>
As we can see the format has been correctly altered.

### Handling Date Outliers
#### Date of Birth
For analysis it is best that we check the date columns to find if there any outliers that would not be explained by some other factor, such as applicants whose date of birth being in the 1800s or very early 1900s being Pennsylvanians whose ages are specifically kept confidential from the public. First we should make sure that all applicants would be eligible to vote on election day (November 3, 2020), due to being at eighteen years of age on the day.

```sql
-- Checking the youngest applicants.
Select DateofBirth from GeneralBallots
order by DateofBirth desc
limit 50;
```

</br>

![Youngest Applicants](https://github.com/user-attachments/assets/589086fa-85fa-4647-87df-c88d90cab3d0)

</br>

There only seems to be one person who is too young to vote, being born on 2006. Therefore, we should remove this error from the dataset

```sql
-- Deleting the outlier
DELETE FROM GeneralBallots WHERE DateofBirth = '2006-07-14';
```

#### Application Return/Processing
Similar to the date of birth, we should make sure that all of the applications were approved and returned in a date is in a plausible range. Dates outside of this range may indicate an error or anomaly in the data entry. Such outliers might distort the analysis and effect its overall accuracy.


To maintain accuracy and data integrity, we must records with approval or return dates that fall outside of 2019 or 2020. By doing this we keep our data integrity high, and the insights from our analysis more accurate. Especially for such a large dataset. 

```sql
-- Deleting large outliers from the application dates
Delete from GeneralBallots 
where (ApplicationApprovedDate not like '2020%' AND ApplicationApprovedDate not like '2019%') 
and (ApplicationReturnDate not like '2020%' AND ApplicationReturnDate not like '2019%');
```

### Calculating Ages
Now that any egregious outlier in the date columns have been cleaned, we have the ability to calculate each applicant's age based on how old they would be on election day.


First we must create an integer column for Age.

</br>

```sql
-- Creating an age column
ALTER TABLE GeneralBallots ADD COLUMN Age INTEGER;
```
</br>


Now that it has been created we must populate the column with the age of each applicant by subtracting the years between their birth and election day.

```sql
-- Populating the age column by updating the column to find the years between election day and date of birth
UPDATE GeneralBallots
SET Age = (
    CAST(
        (JULIANDAY('2020-11-03') - JULIANDAY(DateofBirth)) / 365.25 AS INTEGER
    )
);```


### Generation Assignment
To find the generation that each applicant belongs to, it is best that we make a column for their birth year.
```sql
-- Creates the birth year column
ALTER TABLE Jou ADD COLUMN BirthYear INTEGER;


-- Makes the value of the column the year that the person was born
Update Jou set BirthYear = SUBSTR(DateofBirth, 1, 4);
```




## Exploratory Data Analysis
### Descriptive Statistics


### Univariate Analysis 


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
### Common Insights
### Interactive Tableau Dashboard

## Licenses
