# Data Cleaning in SQL
This is a documented personal project on Data cleaning with SQL and all the process are explained therein.

<br>

<p align="center">
  <img src="Data_Cleaning_Images.jpg">
  <br>
</p>

<br>

## Introduction
Data cleaning is a crucial process in any data analytics project, as it ensures that the data used for analysis is accurate, complete, and consistent. SQL is a powerful query language for managing and manipulating relational databases. The project will involve identifying and addressing errors, inconsistencies, and missing values in the dataset, as well as optimizing the data structure for improved analysis. By the end of this project, I aim to have a clean and reliable dataset ready for further analysis and insights.

In this project, I used the Microsoft SQL Server in cleaning the dataset.
                                                      

The full query for this project can be found [here](Full_Query.md)
                                                      

### Data Sourcing

The dataset that I used in this project is open-source data without any PII (Personally Identifiable Information) that I acquired from [Kaggle](https://www.kaggle.com/datasets/tmthyjames/nashville-housing-data?resource=download). The dataset includes information of real estate data from the hot Nashville housing market.
There are more than 56600 rows on this dataset and 30 columns that are titled: Unnamed, Parcel ID, Land Use, Property Address, Suite/ Condo #, Property City, Sale Date, Sale Price, Legal Reference, Sold As Vacant, Multiple Parcels Involved in Sale, Owner Name, Address, City, State, Acreage, Tax District, Neighborhood, Image, Land Value, Building Value, Total Value, Finished Area, Foundation Type, Year Built, Exterior Wall, Grade, Bedrooms, Full Bath, Half Bath. This dataset has a lot of inconsistencies which makes this dataset perfect to practice data cleaning.

<br>

<p align="center">
  <img src="Excel-Dataset.JPG">
  <br>Excel raw data
</p>

<br>

## Objective of the project

1. Rename Column Names
2. Standardize Sales Date column format
3. Populate Null Property Address data
4. Populate Null Property City data
5. Change abbreviations and correct misspellings in Column Land Use
6. Remove Duplicates
7. Delete Unused Columns

<br>

## Skills / Concepts Demonstrated

The following skills were demonstrated in this data cleaning project using SQL:
1. Proficiency in SQL: The project requires a strong understanding and knowledge of SQL queries and functions to perform various data cleaning tasks such as renaming column names, standardizing column formats, populating null data, changing values, removing duplicates, and deleting columns.

2. Data Analysis: Before making changes, the project necessitates a careful analysis of the dataset to identify areas that need improvement. This includes identifying misspellings, inconsistencies, duplicates, and unused columns. Data analysis skills are crucial for understanding the data and determining the appropriate cleaning actions.

3. Data Cleaning Techniques: The project demonstrates competence in applying various data cleaning techniques, such as renaming columns, standardizing data formats, populating null values, correcting misspellings, removing duplicates, and deleting unused columns. These techniques require attention to detail and a systematic approach to ensure data quality.

4. Data Validation: The project showcases skills in implementing data validation rules to ensure the accuracy and consistency of the data during the cleaning process. This involves validating data formats, ensuring referential integrity, and applying predefined rules to identify and fix data quality issues.

5. Data Governance: The project demonstrates an understanding of data governance principles by following best practices such as establishing data standards, developing data cleaning scripts, defining responsibilities for data cleaning tasks, and implementing ongoing monitoring and maintenance procedures.

6. Communication: The project requires effective communication skills to present findings and recommendations to stakeholders.


Overall, this project showcases a combination of technical skills in SQL, data analysis, data cleaning techniques, data validation, and data governance. These skills are essential for effectively cleaning and maintaining data quality in an MSSQL Server environment.


<br>

## Procedure

Firstly, I pulled the whole dataset to view all the rows and columns. I noticed that some faults were present in the dataset such as the column names that were not properly formated by having spacing between the words. Sale Date column which is in Date-Time format, there were many NULL values in some important parts of the dataset such as the Property Address and Property City, there were also some abreviations and misspellings which might alter the analysis should this be later used for analysis, there were duplicate values present and also some extra columns that are not needed for further analysis.

<br>

### Rename column names (replacing the spacing with an underscore and capitalize the first letters)


  
 <p align="center">
  <img src="Unformated_Column_names_.JPG">
  <br>Unformated Column Names
</p>

I noticed that the column names have spacing between them, this could still be used in a query by opening and closing it with a square bracket but it is not a standard practice of naming a column.

<br>

The spacing was replaced with an underscore and a column was re-named using the below query:

<p align="center">
  <img src="Rename_column_query.JPG">
  <br>Query 1
</p>

<br>

The image below shows the renamed columns to be used for further cleaning as standard practice.

<p align="center">
  <img src="Formated_column_names.JPG">
  <br> Formated Column Names
</p>

<br>

### Standardize the Date format (Eliminating the time constraints from the Sale Date column)

The **Sale_Date** column was formated with **Date-Time** format in this dataset but the time component is not necessary which has to be removed because the dataset is not for Time siries analysis.

The time component was removed using the query below:

<p align="center">
  <img src="Sale_Date_Query1.JPG">
  <br>Query 2
</p>

<br>

This process was achieved by first altering the table adding a **Sale_Date2** column and converted it by setting the date type as **Date**.
The below image show the existing **Dete-Time** column and the created **Date** column

<p align="center">
  <img src="Sale_Date2.JPG">
  <br>Sale_Date2
</p>

<br>

### Populate Null Property Address Data

The **Property_Address** column was noticed to have so many **NULL** values and it is an important column in the dataset which shows the location of a propertty to be analyzed. This has to be corrected because it will sway the outcome of the analysis should the dataset be used for further analysis.

The query below was used to populate the **NULL** values:

<p align="center">
  <img src="Property_Address_Query.JPG">
  <br>Query 3
</p>

<br>

The below image show the **NULL** values in the **Property_Address** column:

<p align="center">
  <img src="Property_Address_Null.JPG">
  <br>Property Address Null
</p>

<br>

The query below was used to populate the Count of **NULL** values in the Property_Address column:

<p align="center">
  <img src="Count_Property_Address_Null.jpg">
  <br>Query 4
</p>

<br>

<p align="center">
  <img src="Property_Address_Null_Count.jpg">
  <br>Property Address Null Count
</p>

From the image above, the **NULL** values are **159** in total which will have to be populatd.

<br>


The query below shows that some rows with similar **Parcel_ID** has the same Property_Address:

<p align="center">
  <img src="Property-ParcelID.JPG">
  <br>Query 5
</p>

<br>

This relationship between the **Parcel_ID** and the **Property_Address** can be used to populate the **NULL** values in the **Property_Address** column using a **SELF JOIN** of the table itself with the query below:

<p align="center">
  <img src="Property-Parcel-Join.JPG">
  <br>Query 6
</p>

For the **JOIN** to function in the query above, I just gave the joined tables the names "a" and "b" at random. *ISNULL(where we want to look for a NULL value, what we want to populate the NULL value with)* is the syntax for this statement. The addresses where the **Parcel_ID** is common and the **Unique_IDs** don't match were filled in by the **ISNULL** query in the **NUL** cells. Due to the possibility that the **Parcel_IDs** for two different rows could be identical, but not the **Unique_ID**, I used the **Unique_ID** to prevent the same **Parcel_IDs** from being compared more than once.

The **ISNULL** has populated the NULL cells with the **Property_Addresses** where it has same **Parcel_ID** with **18 rows**. The remaining **10 rows** does not have **Property_Address** populated.

I can now update the table with the below query:

<p align="center">
  <img src="Property_Address_Populated_Query.jpg">
  <br>Query 7
</p>

If I run the previous Query 6, it populated 10 rows of NULL cells where there are no matching Parcel_Id and the Property_Address..
If I run the query where Property_Address ISNULL it gives 144 rows. This means that 144 rows of the data does not have Property_Address at all which will be reported to the stakeholders.


<br>

### Populate Null Property City data

The **Property_City** column was also noticed to have so many **NULL** values and it is an important column in the dataset which shows the location of a property to be analyzed because knowing the address wthout the city won't give us clue about the location.

The query below was used to populate the **NULL** values of the Property_City:

<p align="center">
  <img src="Property_City_Query.JPG">
  <br>Query 8
</p>

<br>

The below image show the **NULL** values in the **Property_City** column:

<p align="center">
  <img src="Property_City_Null.JPG">
  <br>Property City Null
</p>

<br>

The query below was used to populate the Count of **NULL** values in the Property_City column:

<p align="center">
  <img src="Count_Property_City_Null.jpg">
  <br>Query 9
</p>

<br>

<p align="center">
  <img src="Property_City_Null_Count.jpg">
  <br>Property City Null Count
</p>

From the image above, the **NULL** values are **159** in total which will have to be populatd.

<br>

The query below shows that some rows with similar **Parcel_ID** has the same Property_City:

<p align="center">
  <img src="Property_City-ParcelID.jpg">
  <br>Query 10
</p>

<br>

This relationship between the **Parcel_ID** and the **Property_City** can be used to populate the **NULL** values in the **Property_City** column using a **SELF JOIN** of the table itself with the query below:

<p align="center">
  <img src="Property_City-Parcel-Join.jpg">
  <br>Query 11
</p>

For the **JOIN** to function in the query above, I just gave the joined tables the names "a" and "b" at random. *ISNULL(where we want to look for a NULL value, what we want to populate the NULL value with)* is the syntax for this statement. The city where the **Parcel_ID** is common and the **Unique_IDs** don't match were filled in by the **ISNULL** query in the **NUL** cells. Due to the possibility that the **Parcel_IDs** for two different rows could be identical, but not the **Unique_ID**, I used the **Unique_ID** to prevent the same **Parcel_IDs** from being compared more than once.

The **ISNULL** has populated the NULL cells with the **Property_City** where it has same **Parcel_ID** with **18 rows**. The remaining **10 rows** does not have **Property_City** populated.

I can now update the table with the below query:

<p align="center">
  <img src="Property_City_Populated_Query.jpg">
  <br>Query 12
</p>

If I run the previous Query 11, it populated 10 rows of NULL cells where there are no matching Parcel_Id and the Property_City.
If I run the query where Property_City ISNULL it gives 144 rows. This means that 144 rows of the data does not have Property_City at all which will be reported to the stakeholders.


<br>

### Change abbreviations and correct misspellings in Column Land Use

The Land_Use column in the Nashville Housing dataset contains different types of usage of the land. This could be confirmed with the query below.

<p align="center">
  <img src="LandUse_Distinct_Query.jpg">
  <br>Query 13
</p>

<br>

The query above shows Vacant residential land has 3 distinct values as a result of abbreviations and misspelling which are shown below

<br>

<p align="center">
  <img src="LandUse_Distinct_Image.jpg">
  <br>Distinct Land_Use
</p>

<br>

<p align="center">
  <img src="LandUse_Count_Query.jpg">
  <br>Query 14
</p>

<br>

The count query above shows the count of each of the 3 distinct values which can be seen below.

<br>

<p align="center">
  <img src="LandUse_Count_Image.jpg">
  <br>Land_Use Count
</p>

<br>

In the above image, the value could be seen to be considerably high which could effectively sway the outcome of the analysis if not corrected.

From the above image it could also be seen that;
**‘VACANT RESIENTIAL LAND’** has 3 values
**‘VACANT RESIDENTIAL LAND’** has 3586 values
**‘VACANT RES LAND’** has 1575 values

<br>

I now have to convert the misspelled words to **VACANT RESIDENTIAL LAND** and to do this I wrote the following query using case statement


<p align="center">
  <img src="LandUse_Case_Query.jpg">
  <br>Query 15
</p>

<br>

Looking at the above query shows that the misspelled values have been converted to **VACANT RESIDENTIAL LAND** as seen in the image below.

<br>

<p align="center">
  <img src="LandUse_Case_Image.jpg">
  <br>Land_Use Case Statement
</p>

<br>

The corrected values will now be updated into the table with the query below

<p align="center">
  <img src="LandUse_Updated_Query.jpg">
  <br>Query 16
</p>

<br>

If I run query 14 above again, we see that the 'VACANT RESIENTIAL LAND' and 'VACANT RESIDENTIAL LAND' strings are no longer available and the value of the ‘VACANT RESIDENTIAL LAND’ has increased from **3586** to **5164** as seen below

<p align="center">
  <img src="LandUse_Updated_Image.jpg">
  <br>Land_Use Updated
</p>


<br>

### Remove Duplicates

There are so many duplicated values in the columns of the dataset which has to be removed in other not to have a flawed analysis. And in doing this, I need to identify the duplicate values using the below query.

<p align="center">
  <img src="Duplicate_RowNum_Query.jpg">
  <br>Query 17
</p>

<br>

From the query above, I selected all columns from the NashvilleHousing table and assigns a row number to each row which is either 1 or 2. 2 being for duplicate values. The row number is generated by the ROW_NUMBER() function, which is used in the OVER() clause. The OVER() clause specifies that the row number should be generated within each partition. The partition criteria is a list of columns that are used to group the rows together. In this case, the partition criteria I used is Parcel_ID, Property_Address, Sale_Price, Sale_Date, and Legal_Reference.
The ORDER BY clause specifies that the rows should be sorted by Parcel_ID. This ensures that the rows with the same row number are grouped together.

<br>

In the below query, I created a temporary table called RowNumCTE. The temporary table contains all of the columns from the NashvilleHousing table, as well as the row number generated by the ROW_NUMBER() function. I was able to use this temp table to identify the duplicated values using the WHERE clause.
The ORDER BY clause was also comment out because it could not be used in Common Table Expressions.

<p align="center">
  <img src="Duplicate_RowNumCTE_Query.jpg">
  <br>Query 18
</p>

<br>

From the above query, it could be confirmed that the duplicate values in the table are 104.

<br>

The query below was then used to delete all rows from the RowNumCTE table where the row number is greater than 1. This will leave only the rows that are not duplicates.

<p align="center">
  <img src="Duplicate_Delete_Query.jpg">
  <br>Query 19
</p>

<br>

If I run **Query18** again, it will return empty table which means all the duplicated values have been deleted.


<br>

### Delete Unused Columns

Since the Sale-Date2 column have been created, the Sale_Date column is no longer relevant to us, as you can see from the table's data. This will have to be removed.

Although, eliminating columns from the raw data should not be done in practice, doing so helps save views for later use.
The below query was used to remove the column.

<p align="center">
  <img src="Delete_Column_Query.jpg">
  <br>Query 20
</p>

<br>


### Recommendation:

1. Utilize SQL queries and functions: Utilize SQL queries and functions to rename column names, standardize sales date column format, populate null property address and city data, change abbreviations and correct misspellings in the column land use.

2. Implement data validation rules: Apply data validation rules to ensure consistency and accuracy when making changes. This will help maintain data integrity throughout the cleaning process.

3. Develop data cleaning scripts: Write scripts to identify and remove duplicates from the dataset. This will help reduce data redundancy and improve data quality.

4. Perform a thorough analysis: Before deleting any unused columns, conduct a thorough analysis to confirm their irrelevance. Ensure that removing these columns will not impact any existing or future data analysis requirements.

### Conclusion:

In conclusion, the data cleaning project using SQL Server has successfully achieved the objectives of renaming column names, standardizing the sales date column format, populating null property address and city data, changing abbreviations and correcting misspellings in the column land use, removing duplicates, and deleting unused columns.

By utilizing SQL queries and functions, implementing data validation rules, and developing data cleaning scripts, the project has improved the overall quality and consistency of the data stored.

The cleaned dataset is now more reliable and accurate, enabling better decision-making, smoother analysis, and improved operational efficiency. Regular monitoring and ongoing data governance practices are essential to maintain the quality of the data in SQL Server.
