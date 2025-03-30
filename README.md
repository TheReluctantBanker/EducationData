# EducationData
Data Analysis and Visualization on NSW school data

Introduction

This is documentation explaining the work done in creating the Power BI dashboard “Dashboard -Education Data.pbix”. The work encompassed data gathering, exploration, transformation, analysis, visualization and identifying insights and findings based on the visuals.

Objective

The objective of creating the dashboard is to enable the Department of Education, NSW’s senior executives, to obtain an understanding of the trends in schools and enrolments over an extended timeframe. 

Steps followed

•	Gather required data sources

•	Understand & explore the data (using SQL)

•	Import, transform and model data in Power BI

•	Create visualizations in Power BI

•	Interactively analyze the visuals and the underlying data to uncover insights, patterns or trends.

•	Implement report design for the dashboard.

•	Document work done.

Data Gathering 

The data for this work was obtained from The Australian Curriculum Assessment and Reporting Authority. Under their Data Access Program, they made available school location data as of 2024 and profile and enrolment data over 2008-2024 in their website. Data was downloaded in the form of individual excel files. 

Data Understanding

![image](https://github.com/user-attachments/assets/48eb079e-0bbe-42c3-9df7-b4dba7972f67)

A data dictionary provided by ACARA helped with an initial understanding of the data fields. 

Data Exploration using SQL

I found it practical to first explore and initially understand the datasets by using SQL queries. The datasets were imported into a local SQL database instance and then interactive SQL queries were written. 

Some findings based on data exploration are mentioned here. The analysis and work are restricted to NSW state’s records.

[1]  The school location dataset is considered the “dimension table” and the other two datasets (school profile, enrolment data) as fact tables. This enables modelling the data & relationships between them effectively.

[2]  There are some ACARA SML IDs present in the metadata file but not present in the time series data and vice versa. [ for example, about 256 IDs found in school location data, but not found in enrollment data, 212 IDs found in school location data, not found in profile data, 73 IDs found in school profile data, but not in enrollments data] . My understanding for the missing data is that the metadata file contains data as of 2024 and so would not contain data on schools closed prior to 2024.

[3]  The ICSEA index is expected to be in the range of 500-1300. Two records were found for a school (ACARA SML ID =43091) where the value was 100. We would need to investigate whether the value is correct or not.

[4] 	Both the datasets “School Profile 2008-2024.xlsx” and “Enrolments by Grade 2008-2024.xlsx” contain a field called “Total Enrolments”. The second dataset further contains enrolment by Year from Year 1 to Year 12 and ungraded enrolments. It was found during analysis that the “Total Enrolments” did not equal the sum of the individual enrolment fields” for a very small number of School IDs (about 33 records). However, “Total Enrolments” has matched for the School IDs between the two datasets. 

Data Preparation with SQL

I performed the data preparation using SQL consisting of the following steps

1.	Import raw data (excel files) into SQL Server database (local instance for this work task)
2.	Utilize a three way inner join between to determine applicable ACARA SML IDs ( schools operational as of 2024)
3.	Filter the dimension table (school location) for applicable IDs.
4.	Preliminarily join the school profile and school enrolments data to bring together total enrolments and year wise enrolments for the applicable ACARA SML IDs.
5.	From Step 4, identify IDs which have historical data for < 10 years.
6.	Exclude  those IDs from the joined fact tables.
7.	Exclude IDs from the dimension table from Step [3].

Key Assumptions & Rationale

Total Enrolments: To analyze total enrolment figures, I have used the field from the School Profile datasets. The year wise enrolment figures were used to calculate primary school enrolments (Year 1  - Year 6), Secondary School enrolments (Year 7 - Year 10) and Senior Secondary School enrolments (Year 10 - Year 12).

School population based on number of years of data: There are schools which closed prior to 2024 and schools which opened at different years between 2008-2024. 

For this work task, we have considered School IDs which 
[1] Are present in all three datasets 
[2] Schools had data available for enrolments for at least 10 years as of 2024

The above two assumptions have resulted in a population of 3035 schools as of 2024. The intention is to maintain a homogenous base of schools for analysis.

Number of years of data used in analysis: The instruction for the work task  mentioned considering data over the past 5 years, this would have meant 2020-2024, however, I have considered the period 2015-2024 based on the below reasons. 
1.	The years 2020-2022 were extraordinary given the COVID-19 pandemic and therefore can skew the overall picture.
2.	Utilizing a longer time frame provides a better view of evolving trends and patterns.


Data Transformation in Power BI

Two excel files “SQLExport_DimensionSchoolData.xlsx” and “SQLExport_Fact_SchoolData.xlsx” were exported from SQL. These were imported into Power BI.


Since the data is already clean, minimal data transformations were required in Power Query. For facilitating time based analysis, it is good practice to utilize date tables. The fact table contains a numeric column called “Calendar Year”. This was converted to a date field, by suffixing the values with “-12-31” and converting to a date field. 
![image](https://github.com/user-attachments/assets/67f5c38c-5d00-4553-8a31-ed2f04d02847)

![image](https://github.com/user-attachments/assets/3cf85945-8454-44d7-b929-e27c7163f7bb)

The data was filtered to consider the period 2015-2024.
![image](https://github.com/user-attachments/assets/35cc2e45-cb23-4e15-869b-fa5a97a5f8ef)

A date table was created 

![image](https://github.com/user-attachments/assets/83296d12-387c-4596-b878-7c457ced8567)

A relationship was created between the date table and the fact table, and the dimension table and the fact table.

Data Visualization in Power BI

I had a draft of ideas and visuals to create  based on the same. I typically create some rough sketches of visuals on paper to kick-start the process. The data visualizations would focus on
1.	Provide a snapshot of key metrics and compositions as of 2024.
2.	Provide trends by demography and enrolment over the historical period.

Measures were created extensively to help summarize information from the data fields. They are organized under a table called “_Measures” which makes it easier to manage them. Power BI also provides the ability to group measures under folders.

The insights are documented in a PDF document.

Good Practices
Here is a summary of key good practices followed during the dashboard development.
1.	Created a date table to enable time based analysis.
2.	Created measures which could be used across visuals for sums, percentage growths.
3.	Ensured relationships between tables are appropriate.
4.	Enabled  filters cascade across tabs by syncing them.
5.	Used View - Lock Objects to avoid accidental displacement of text box and other objects in the canvas (after visuals are developed and formatted).

Report Design
Report designing involved tasks such as applying consistent formatting for the visualizations (font size, font, axis values, axis labels, positioning of legend, color scheme, size and placement of visuals). I have used tool tips to provide helpful information to users.

I have also provided a “Home” tab and used bookmarks to enable easier navigation to and from the individual tabs.

Please note I have not utilized any logos/wordings to indicate association with the Department of Education, since I do not have permission for re-use of registered trademarks.

Further Improvements

Subject to additional time being available, I would consider areas for further streamlining and optimizing the visuals, for example
1.	Deep dive into year wise enrolment trend/growth rates and their comparison by school sector to uncover any patterns there (  I did create another dataset within Power BI to unpivot the time series data by year of enrolment but ran out of time to work on this!)
2.	Developing a separate process for initial data quality or integrity checks on the data fields from source data
3.	Working out a process for approaching SQL based data preparation & annual update in an organizational setting to ensure reproducibility and repeatability of the work done.
4.	To find out how to refer to the latest available date in the date in page filters, to eliminate hard coding.

Conclusion
This concludes my documentation on the work task of gathering, analyzing and visualizing education data relating to schools and enrolments. Please refer to the document “Quick User Guide.pdf” for some information relating to using the Power BI dashboard file (.pbix)

References
https://www.acara.edu.au/contact-us/acara-data-access
https://app.datacamp.com/learn/career-tracks/data-analyst-in-power-bi







