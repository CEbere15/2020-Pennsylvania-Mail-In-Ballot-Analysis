# 2020 Pennsylvania General Election Mail Ballot Data Analysis
### Table of Contents
- [Overview](#Overview)
  - [Summary](#Summary)
  - [Data Source](#Data-Source)
  - [Tools](#Tools)
  - [Data Dictionary](#Data-Dictionary)
  - [Objectives](#Objectives)
  - [References](#References)
- [Data Cleaning and Preparation](#data-cleaning-and-preparation)
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



## Data Cleaning and Preparation
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
);
```

Now we can test how well the command executed.

```sql
Select CountyName, DateofBirth, Age from GeneralBallots limit 5
```


![Age Test](https://github.com/user-attachments/assets/93f03388-c529-49cd-918b-5a7eb9bbd464)

As we can see the age for applicants was calculated correctly.

### Generation Assignment
To find the generation that each applicant belongs to, it is best that we make a column for their birth year, from taking it from their date of birth.
```sql
-- Creates the birth year column
ALTER TABLE GeneralBallots ADD COLUMN BirthYear INTEGER;


-- Makes the value of the column the year that the person was born
Update GeneralBallots set BirthYear = SUBSTR(DateofBirth, 1, 4);
```


Now to test if it executed correctly.

![Birth Year Test](https://github.com/user-attachments/assets/f83f67b6-ee15-480b-978f-4f71a7d530e1)


</br>

Now that there is a Birth Year column, we can sort each applicant into a genertiion based on that year.


To make sure that we accurately tabulate the amount of members of the Greatest Generation there are, the cutoff year is 1908, which was the year the oldest known living Pennsylvanian was born in the beginning of 2020. Anyone born before then is placed into the Unknown section. 

```sql
-- Create a column for Generation
ALTER TABLE GeneralBallots ADD COLUMN Generation INTEGER;

-- Populating the Generation column by setting a generation by birth year
UPDATE GeneralBallots
SET Generation = 
    CASE WHEN BirthYear BETWEEN '1928' AND '1945' THEN 'Silent Generation'
        WHEN BirthYear BETWEEN '1908' AND '1927' THEN 'Greatest Generation'
        WHEN BirthYear BETWEEN '1946' AND '1964' THEN 'Baby Boomers'
        WHEN BirthYear BETWEEN '1965' AND '1980' THEN 'Generation X'
        WHEN BirthYear BETWEEN '1981' AND '1996' THEN 'Millennials'
        WHEN BirthYear BETWEEN '1997' AND '2012' THEN 'Generation Z'
        WHEN BirthYear >= '2013' THEN 'Generation Alpha'
        ELSE 'Unknown'
    END;
```

</br>

Now that the column has been populated let us make sure that it is worked.

```sql
Select CountyName, DateofBirth, BirthYear, Age, Generation
from GeneralBallots
limit 10;
```


![Generation Test](https://github.com/user-attachments/assets/b6d8f3c7-f0b5-4b85-bab1-9e87bc4473d8)

As we can see the generation column was made correctly


### Party Grouping

To make it easier to analyze different party performances it makes sense to group the amount of parties, in which there are many, into different categories.

First let us get a feel of the multiple parties by making a query that joins with the Party Codes, to give the name of the abbreviated party registration.

```sql
-- Use a join to show the name of the party and its designation and how common it is.
SELECT PoliticalPartyDescription, ApplicantPartyDesignation, COUNT(*) AS count
FROM GeneralBallots
inner join [Party Codes] on [Party Codes].Code = GeneralBallots.ApplicantPartyDesignation
GROUP BY [ApplicantPartyDesignation] order by Count desc;

```

![Des and Name Check](https://github.com/user-attachments/assets/593508f2-c55e-4e32-934e-6370cc465e93)


As we can see, most of the applicants are part of the Top 10 designations. Meaning that it is better to group up the hundreds into just a few. The best categories would most likely be Democratic, Republican, Libertarian, Independent, Green and Minor Party



These queries will create a column for Party Category and group all the parties into the aforementioned categories.

```sql
-- Create a column for Party Category
ALTER TABLE GeneralBallots ADD COLUMN Category INTEGER;

-- Populating the party category by setting them by party designation
UPDATE GeneralBallots
SET Category = 
Case when ApplicantPartyDesignation = 'D' then 'Democratic'
when ApplicantPartyDesignation in ('GOP','R') then 'Republican'
when ApplicantPartyDesignation in ('NF','I','NOP','NO','OTH','INDE','NON') then 'Independent'
when ApplicantPartyDesignation in ('GPUS','GR','GP') then 'Green'
when ApplicantPartyDesignation in ('LN') then 'Libertarian'
else 'Minor Party' end;
```

</br>

Now to test if the new column was made correctly.
```sql
Select CountyName, ApplicantPartyDesignation, Category from GeneralBallots limit 20;
```

![Party Category Test](https://github.com/user-attachments/assets/14bf3eb0-cbec-4d94-bedb-e92da2887fb8)

As we can see, the Category column matches up with the designation. 
### Application Grouping
To better analyze the different methods of mail in voting that were applied for, it is best we group them into categories such as paper, mailed, overseas and online. The following query will create a column for Application Category and populate it by what the Application Type is.

```sql
-- Create a column for Application Category
ALTER TABLE GeneralBallots ADD COLUMN [Application Category] INTEGER;

-- Populating the application category based on application type
UPDATE GeneralBallots
SET [Application Category] = 
Case when MailApplicationType in ('OLREGNV', 'OLMAILNV', 'OLREGV', 'OLMAILV') then 'Online'
when MailApplicationType in ('REG','CIV') then 'Paper'
when MailApplicationType in ('MAILIN','PMI', 'F','BV', 'V', 'PER','ALT','BV','M', 'C', 'ALT') then 'Mailed/Absentee'
when MailApplicationType in ('CVO', 'CRI', 'MRI', 'BVRI') then 'Overseas/Remote or Isolated'
End;

```

</br>

Now to make sure the categorization worked.

```sql
-- Testing the application type categorization
Select CountyName, MailApplicationType, [Application Category] from GeneralBallots limit 10;
```

</br>



![Categorization Application Test](https://github.com/user-attachments/assets/efb5b5a9-6b82-45ab-a382-17e5d1dcafb3)

As we can see the case statement ran correctly.


## Exploratory Data Analysis
### Descriptive Statistics




### Distribution Analysis
#### Age
```py
# Histogram for Ages
plt.subplots(figsize=(75,45))
sns.histplot(data.query("Generation != 'Unknown'")['Age'], bins=20, kde=True) # Data filtered so that anyone in an unknown generation due to having an age that was designated as too high, is not in the histogram
plt.title('Distribution of Age')
plt.show()
```
![Age Distributiion](https://github.com/user-attachments/assets/c11983a8-3869-4769-9872-bebf78277147)

#### Age by County
```py
boxplot = data.query("Generation != 'Unknown'").boxplot(column='Age', by='CountyName', vert= False, figsize=(60,60))
```

![Ages by County](https://github.com/user-attachments/assets/3c0b6ce7-8bb4-4032-870e-8c9dbe38ac7e)


### Univariate Analysis
#### Generation
```py
# Creation of an order for Generation
GenOrd = data['Generation'].value_counts().index

# Creating a figure size for the count plot
plt.figure(figsize=(20, 10))

# Creating the Countplot
sns.countplot(y='Generation', data=data, order=GenOrd, palette='crest')

# Labeling the countplot
plt.title('Generation Count')
plt.xlabel('Count')
plt.ylabel('Generation')
plt.show()
```

It's clear from the plot that the most common Generation to apply for mail in ballots are Baby Boomers. Beating out Millenials by about 400,000 applications. Behind them are Generation X, the Silent Generation, Generation Z and the Greatest Generation. The fact that older generations are so readily voting by mail could have to do with COVID making it the preferred options for those generations. 


![Generation Count](https://github.com/user-attachments/assets/07074db9-c156-4068-bded-2b0051ac999a)

#### Congressional Districts

```py
# Creation of an order for Congreessional Districts by Applications
ConOrd = data['CongressionalDistrict'].value_counts().index

# Creating a figure size for the count plot
plt.figure(figsize=(20, 16))

# Creating the Countplot
sns.countplot(y='CongressionalDistrict', data=data, order=ConOrd, palette='crest')

# Labeling the countplot
plt.title('Applications by Congressional Districts')
plt.xlabel('Count')
plt.ylabel('Congressional District')
plt.show()
```
The 3rd Congressional District has the most applicants from there than any other district, followed not far behind by the 18th and 4th respectively. However, the top three districts with the least applicants from there, 15th, 12th and 13th have less than half of the applications as the Top 3. This is despite the fact that Congressional Districts have populations that are virtually identical to one another. Meaning for what ever reason some districts have more people apply for mail in ballots than others. Which could mean that there are some regional factors at work here. Population density could mean that more rural or exurban districts have less sophisticated mailing systems. Which means people are less likely to choose to vote by mail and instead just vote in person due to being less at risk for COVID, not having a sophisticated system for mail in voting, and it being cheaper for them to do.  


![Congressional Count Plot](https://github.com/user-attachments/assets/f4cb8417-7a76-43b6-af41-d911ea8429d4)


#### Application Category
```py

# Creation of an order for Application Categories
AppOrd = data['Application Category'].value_counts().index

# Creating a figure size for the count plot
plt.figure(figsize=(20, 10))

# Creating the Countplot
sns.countplot(y='Application Category', data=data, order=AppOrd, palette='crest')

# Labeling the countplot
plt.title('Application Category Count')
plt.xlabel('Count')
plt.ylabel('Application Category')
plt.show()

```

</br>


![Application Category Count](https://github.com/user-attachments/assets/068679f9-7bc9-4d38-821c-421e303aac17)


</br>

From this count plot, it looks the most common method of applying is through online methods by a rather wide margin. The second most common method is Mailed applications. The distant third and fourth are paper applications and overseas. Which would make sense, seeing as most voters would likely be in-state and not overseas during voting season. The fact that online and mail are so common, likely has to do with COVID making these the most convenient options for many voters. 
#### Party Category

```py



# Creation of an order for Party Categories
PartOrd = data['Category'].value_counts().index

# Creating a figure size for the count plot
plt.figure(figsize=(20, 10))

# Creating the Countplot
sns.countplot(y='Category', data=data, order=PartOrd, palette='crest')

# Labeling the countplot
plt.title('Party Category Count')
plt.xlabel('Count')
plt.ylabel('Application Category')
plt.show()



```

![Party Count Plot](https://github.com/user-attachments/assets/3ede6f46-7e51-4729-9692-73f7db7e03f5)

This plot shows that a registered Democrats were a majority of the applicants for mail ballots. With registered Republicans being a very distant, but clear second, followed by Independents. The final three (Minor Parties, Green, Libertarian), were a very small share of the applicants for mail ballots. 
</br>


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
