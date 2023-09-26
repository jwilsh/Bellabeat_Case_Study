# Bellabeat_Case_Study
This project is a documentation of my capstone project. The final case study at the end of the last module from the [Google Data Analytics Certificate](https://www.coursera.org/professional-certificates/google-data-analytics) course. This case study showcases some of the skills that I have inherited during the course across different tools used in data analysis.

# Scenario
Bellabeat is a high-tech manufacturer of health-focused products specifically for women. Bellabeqat is a successful small company, however they have a vision of becoming one of the major players in the global smart device market. The Co-founder of Bellabeat Urška Sršen, beleives that by analysing smart device fitness data can help unlock new growth opportunities for the company. 

We will be looking at data from an existing dataset [FitBit Fitness Tracker Data](https://www.kaggle.com/datasets/arashnic/fitbit) to determine if there are any trends that we can identify. These insights will then be used to give high level recommendations to guide the marketing strategy for the company.

# 1.0 Ask

### 1.1 Business Task:
Use the data provided in the [FitBit Fitness Tracker Data](https://www.kaggle.com/datasets/arashnic/fitbit) dataset to gain insights into how people use non-Bellabeat smart devices. Use these insights to help guide the marketing strategy for the company.

### 1.2 Business Objectives:
- What are some trends in smart device usage?
- How could these trends apply to Bellabeat customers?
- How could these trends help influence Bellabeat marketing strategy?

### 1.3 Deliverables:
- A clear summary of the business task
- A description of all data sources used
- Documentation of any cleaning or manipulation of data
- A summary of your analysis
- Supporting visualizations and key findings
- Your top high-level content recommendations based on your analysis

### 1.4 Key Stakeholders:
- **Urška Sršen:** Bellabeat’s cofounder and Chief Creative Officer
- **Sando Mur:** Mathematician and Bellabeat’s cofounder; key member of the Bellabeat executive team
- **Bellabeat marketing analytics team:** A team of data analysts responsible for collecting, analyzing, and
reporting data that helps guide Bellabeat’s marketing strategy.

### Products:
- **Bellabeat app:** The Bellabeat app provides users with health data related to their activity, sleep, stress,
menstrual cycle, and mindfulness habits. This data can help users better understand their current habits
and make healthy decisions. The Bellabeat app connects to their line of smart wellness products.
- **Leaf:** Bellabeat’s classic wellness tracker can be worn as a bracelet, necklace, or clip. The Leaf tracker connects
to the Bellabeat app to track activity, sleep, and stress.
- **Time:** This wellness watch combines the timeless look of a classic timepiece with smart technology to track user
activity, sleep, and stress. The Time watch connects to the Bellabeat app to provide you with insights into your
daily wellness.
- **Spring:** This is a water bottle that tracks daily water intake using smart technology to ensure that you are
appropriately hydrated throughout the day. The Spring bottle connects to the Bellabeat app to track
your hydration levels.
- **Bellabeat membership:** Bellabeat also offers a subscription-based membership program for users.
Membership gives users 24/7 access to fully personalized guidance on nutrition, activity, sleep, health
and beauty, and mindfulness based on their lifestyle and goals.

# 2.0 Prepare

### 2.1 Information on Data Source:
- Data is publicly available on Kaggle: FitBit Fitness Tracker Data and stored in 18 csv files.
- Generated by respondents from a survey via Amazon Mechanical Turk between 12 March 2016 to 12 May 2016.
- 30 FitBit users consented to the submission of personal tracker data.
- Data collected includes physical activity recorded in minutes, heart rate, sleep monitoring, daily activity and steps.

### 2.2 Limitations of Data Set:
- Data is collected 5 years ago in 2016. Users’ daily activity, fitness and sleeping habits, diet and food consumption may have changed since then. Data may not be timely or relevant.
- Sample size of 30 FitBit users is not representative of the entire fitness population. The central limit theorem general rule of n≥30 applies and we can use the t test for statstic reference. However, a larger sample size is preferred for the analysis.
- As data is collected in a survey, we are unable to ascertain its integrity or accuracy.
- After further investigation with n_distinct() to check for unique user Id, the set has 33 user data from daily activity, 24 from sleep and only 8 from weight. There are 3 extra users and some users did not record their data for tracking daily activity and sleep.
- For the 8 user data for weight, 5 users manually entered their weight and 3 recorded via a connected wifi device (eg: wifi scale).
- Most data is recorded from Tuesday to Thursday, which may not be comprehensive enough to form an accurate analysis.

### 2.3 Is Data ROCCC?
A good data source is ROCCC which stands for Reliable, Original, Comprehensive, Current, and Cited.

Reliable — LOW — Not reliable as it only has 30 respondents

Original — LOW — Third party provider (Amazon Mechanical Turk)

Comprehensive — MED — Parameters match most of Bellabeat products’ parameters

Current — LOW — Data is 5 years old and may not be relevant

Cited — LOW — Data collected from third party, hence unknown

Overall, the dataset is considered bad quality data and it is not recommended to produce business recommendations based on this data.

### 2.4 Data Selection
The following file is selected and copied for analysis.

dailyActivity_merged.csv

### 2.5 Tool
We are using SQL in MySQL for data cleaning and transformation. Tableau is being used for the visualisations.

# 3.0 Process

### 3.1 Setting-Up the Environement
A new schema is created withtin MySQL with a relevent title for the project 'bellabeat-capstone-case-study'. 

```
CREATE SCHEMA 'bellabeat-capstone-case-study'
```


### 3.2 Importing the Dataset
The .cvs files from ealrier were uploaded onto this newly created dataset.

### 3.2 Preview the Data and it's Structure
The .cvs files from ealrier were uploaded onto this newly created dataset. Here you can see a preview of the dataset.

![Data Preview 1]

You can check the structure of the data in the 'schema' tab.

![Data Preview 2]


### 3.3 Data Cleaning and Manipulation
Now that we are familiar with the structure of the data we can clean the data by checking for any errors.

### 3.3.1 Check the number of participants for each data set
Its important to check that the same amount of participants have participated in all of our datasets.
```
SELECT COUNT(distinct Id)
 FROM bellabeat_capstone_case_study.dailyactivity_merged;
```



### 3.3.2 Check Data for null or Missing Values
All columns were checked for Null data or missing values. However none were found.

```
SELECT Id

FROM bellabeat_capstone_case_study.dailyactivity_merged

WHERE Id is null;
```


### 3.3.2 Check If Correct Datatypes are Applied
The 'ActivityDate' column currently has TEXT datatype and willneed to be changed to DATE. Luckily all of the rest of collumns have the correct data type applied so there will be no need to chnage these.

```
ALTER TABLE dailyactivity_merged

MODIFY ActivityDate DATE;
```

### 3.4 Data Manipulation & Transformation
After checking for dirty data its now time for some data manipultion to get it ready for some analysis. Below are the steps I took in order to transform the data ready to create some visualisations with.

- Create new column DayOfTheWeek by generating date in the form of day of the week for further analysis.

- Create new column TotalMinutes being the sum of VeryActiveMinutes, FairlyActiveMinutes, LightlyActiveMinutes and SedentaryMinutes.

- Create new column TotalHours by converting new column TotalMinutes to number of hours.

- Rearrange and rename columns. As well as removing unused columns.


### First I will create the new columns
```
ALTER TABLE dailyactivity_merged ADD COLUMN TotalMinutes INT;

UPDATE dailyactivity_merged SET TotalMinutes = VeryActiveMinutes + FairlyActiveMinutes + LightlyActiveMinutes + SedentaryMinutes;

ALTER TABLE dailyactivity_merged ADD COLUMN TotalHours INT;

UPDATE dailyactivity_merged SET Totalhours = TotalMinutes / 60;

ALTER TABLE dailyactivity_merged ADD COLUMN CaloriesPerHour INT;

UPDATE dailyactivity_merged SET CaloriesPerHour = Calories / TotalHours;
```

### ActivityDate format changed and Weekday column created
```
UPDATE dailyactivity_merged SET ActivityDate
CASE WHEN ActivityDate LIKE '1%' THEN 01
WHEN ActivityDate LIKE '2%' THEN 02
WHEN ActivityDate LIKE '3%' THEN 03
WHEN ActivityDate LIKE '4%' THEN 04
WHEN ActivityDate LIKE '5%' THEN 05
WHEN ActivityDate LIKE '6%' THEN 06
WHEN ActivityDate LIKE '7%' THEN 07
WHEN ActivityDate LIKE '8%' THEN 08
WHEN ActivityDate LIKE '9%' THEN 09
WHEN ActivityDate LIKE '%/1/%' THEN /01/
WHEN ActivityDate LIKE '%/2/%' THEN /02/
WHEN ActivityDate LIKE '%/3/%' THEN /03/
WHEN ActivityDate LIKE '%/4/%' THEN /04/
WHEN ActivityDate LIKE '%/5/%' THEN /05/
WHEN ActivityDate LIKE '%/6/%' THEN /06/
WHEN ActivityDate LIKE '%/7/%' THEN /07/
WHEN ActivityDate LIKE '%/8/%' THEN /08/
WHEN ActivityDate LIKE '%/9/%' THEN /09/
END AS Date;

UPDATE dailyactivity_merged SET ActivityDate = CONCAT(ActivityDate LIKE 

ALTER TABLE dailyactivity_merged ADD COLUMN Weekday VARCHAR(10);

UPDATE dailyactivity_merged SET Weekday = Weekday(Activitydate);
```


### Then I will delete unused columns and round colums to 2 decimal places
```
ALTER TABLE dailyactivity_merged DROP COLUMN LoggedActivitiesDistance;

ALTER TABLE dailyactivity_merged DROP COLUMN SedentaryActiveDistance;

ALTER TABLE dailyactivity_merged DROP COLUMN TrackerDistance;

UPDATE dailyactivity_merged SET TotalDistance = round(TotalDistance,2);

UPDATE dailyactivity_merged SET TrackerDistance = round(TrackerDistance,2);

UPDATE dailyactivity_merged SET VeryActiveDistance = round(VeryActiveDistance,2);

UPDATE dailyactivity_merged SET ModeratelyActiveDistance = round(ModeratelyActiveDistance,2);

UPDATE dailyactivity_merged SET LightActiveDistance = round(LightActiveDistance,2);
```

![Final_table](https://github.com/jwilsh/Bellabeat_Case_Study/assets/98908958/c3bbf6cb-58ba-4e31-bc84-750cb22329b4)

Now that the data has been manipulated and transformed. We are ready to perfom the analysis.

# 4.0 Analyse

### 4.1 Perform Calculations
Firstly I have drawn a bunch of statistics from the important columns in the table. These statistics include,
- Count(no. of nows)
- Mean(average)
- STD(standard Deviation)
- Min & Max


First up is the TotalSteps column.
```
SELECT COUNT(TotalSteps),
AVG(TotalSteps),
STDDEV(TotalSteps),
MIN(TotalSteps),
MAX(TotalSteps)
FROM dailyactivity_merged;
```

![TotalSteps_Statistics](https://github.com/jwilsh/Bellabeat_Case_Study/assets/98908958/5ae54e78-9461-46a2-bf45-a448b55cfa03)


```
SELECT COUNT(TotalDistance),
AVG(TotalDistance),
STDDEV(TotalDistance),
MIN(TotalDistance),
MAX(TotalDistance)
FROM dailyactivity_merged;
```

![TotalDistance_Statistics](https://github.com/jwilsh/Bellabeat_Case_Study/assets/98908958/98517b65-47d1-461b-aaf3-6d040c45d8aa)


```
SELECT COUNT(SedentaryMinutes),
AVG(SedentaryMinutes),
STDDEV(SedentaryMinutes),
MIN(SedentaryMinutes),
MAX(SedentaryMinutes)
FROM dailyactivity_merged;
```

![SedentaryMinutes_Statistics](https://github.com/jwilsh/Bellabeat_Case_Study/assets/98908958/610c4609-7979-42fa-bd14-8c0582dc8001)


```
SELECT COUNT(TotalHours),
AVG(TotalHours),
STDDEV(TotalHours),
MIN(TotalHours),
MAX(TotalHours)
FROM dailyactivity_merged;
```

![TotalHours_Statistics](https://github.com/jwilsh/Bellabeat_Case_Study/assets/98908958/f9f461cd-5697-4d72-bbd5-37e0ba49e3ec)


```
SELECT COUNT(CaloriesPerHour),
AVG(CaloriesPerHour),
STDDEV(CaloriesPerHour),
MIN(CaloriesPerHour),
MAX(CaloriesPerHour)
FROM dailyactivity_merged;
```

![CaloriesPerHour_Statistics](https://github.com/jwilsh/Bellabeat_Case_Study/assets/98908958/12213ec4-471a-4bd7-a8a6-948af0184b38)

### 4.1 Statistical Findings
1. From the final table and the statistics we can see that users had an average of 6507 steps per day which equated to an average of 4.41km per day. The daily recommended amount of steps to be made each day is 7500 which means that on average the participants used in the data are less active than they should be.

2. The amount of SedentaryMinutes tracked was on average 1084.47. This equates to ~18 hours whcih is something that needs to be worked in with an objective of lowering it. The large amount of SedentaryMinutes highlights that the majority of users weren't very active during the time that the information was collected.

3. The average amount of calories burned per hour is 95. This equates to 2280 per day. Its hard to come to any conclusions with this data as the amount of calories burned and the amount needed to be burned depends on too many external factors such as age, gender, profession, body anatomy as well as others. 

# 5.0 Share
Our data is now imported into Tableau where we can create visualisations to explain and communicate the findings within the data based on the analysis.


### Calories vs. TotalSteps
A visualisation has been made to check to see if there is any correlation between the amount of calories burned and the total amount of steps done.

![TotalStepsvs.Calories_Vis]

From the graph you can clearly see a postive correlation betwen the amount of calories burned and the steps taken.  However the data does include a couple of outliers who potenitally didn't participate properly in the study.

### Calories vs. HoursLogged
The next visualisation compares the amount of calories burned for every hour logged.

![Caloriesvs.HoursLogged_Vis]

The graph shows that there is a slight positive corelation between the amount of calroies burned and the total amount of hours logged. This will be mainly due to the large amount of sedentary hours logged by the participants in the study.
Lots of people logged 24 hours and had a large range of calories burned from 1300-3500. 
This chart also had several outliners which could have been from various reasons.

### ActivityType Comparison


![ActivityType_comparison_Vis]

# 6.0 Act


## Contact
* https://github.com/jwilsh
* https://www.kaggle.com/jwilsh
* 
