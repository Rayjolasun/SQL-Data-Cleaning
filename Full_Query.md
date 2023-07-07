---DATA CLEANING USING SQL QUERY

```sql
SELECT *
FROM PortfolioProject2.dbo.NashvilleHousing


---Rename column names (replacing the spacing with an underscore and capitalize the first letters)

SP_RENAME 'dbo.NashvilleHousing.Unnamed: 0', 'Unique_ID', 'COLUMN';

SP_RENAME 'dbo.NashvilleHousing.Parcel ID', 'Parcel_ID', 'COLUMN';

SP_RENAME 'dbo.NashvilleHousing.Land Use', 'Land_Use', 'COLUMN';

SP_RENAME 'dbo.NashvilleHousing.Property Address', 'Property_Address', 'COLUMN';

SP_RENAME 'dbo.NashvilleHousing.Suite/ Condo   #', 'Suite/Condo', 'COLUMN';

SP_RENAME 'dbo.NashvilleHousing.Property City', 'Property_City', 'COLUMN';

SP_RENAME 'dbo.NashvilleHousing.Sale Date', 'Sale_Date', 'COLUMN';

SP_RENAME 'dbo.NashvilleHousing.Sale Price', 'Sale_Price', 'COLUMN';

SP_RENAME 'dbo.NashvilleHousing.Legal Reference', 'Legal_Reference', 'COLUMN';

SP_RENAME 'dbo.NashvilleHousing.Sold As Vacant', 'Sold_As_Vacant', 'COLUMN';

SP_RENAME 'dbo.NashvilleHousing.Multiple Parcels Involved in Sale', 'Multiple_Parcels_Involved_in_Sale', 'COLUMN';

SP_RENAME 'dbo.NashvilleHousing.Owner Name', 'Owner_Name', 'COLUMN';

SP_RENAME 'dbo.NashvilleHousing.Tax District', 'Tax_District', 'COLUMN';

SP_RENAME 'dbo.NashvilleHousing.image', 'Image', 'COLUMN';

SP_RENAME 'dbo.NashvilleHousing.Land Value', 'Land_Value', 'COLUMN';

SP_RENAME 'dbo.NashvilleHousing.Building Value', 'Building_Value', 'COLUMN';

SP_RENAME 'dbo.NashvilleHousing.Total Value', 'Total_Value', 'COLUMN';

SP_RENAME 'dbo.NashvilleHousing.Finished Area', 'Finished_Area', 'COLUMN';

SP_RENAME 'dbo.NashvilleHousing.Foundation Type', 'Foundation_Type', 'COLUMN';

SP_RENAME 'dbo.NashvilleHousing.Year Built', 'Year_Built', 'COLUMN';

SP_RENAME 'dbo.NashvilleHousing.Exterior Wall', 'Exterior_Wall', 'COLUMN';

SP_RENAME 'dbo.NashvilleHousing.Full Bath', 'Full_Bath', 'COLUMN';

SP_RENAME 'dbo.NashvilleHousing.Half Bath', 'Half_Bath', 'COLUMN';


---Standardize the Date format (Eliminating the time constraints from the Sale Date column)

ALTER TABLE [PortfolioProject2].[dbo].[NashvilleHousing]          ---This query was used to add the Sale_Date2 column
ADD Sale_Date2 Date;

UPDATE [PortfolioProject2].[dbo].[NashvilleHousing]                ---The Sale_Date 2 column was converted to Date type column
SET Sale_Date2 = TRY_CONVERT (date, Sale_Date)

-- The below query gives the Datetime column and the added Date column.
SELECT Sale_Date, Sale_Date2
FROM [PortfolioProject2].[dbo].[NashvilleHousing]


---Populate Null Property_Address Data

SELECT Property_Address
FROM [PortfolioProject2].[dbo].[NashvilleHousing]
WHERE Property_Address IS NULL


--Count Property_Address Null Data
SELECT COUNT(*) AS [Property_Address Null Values]                      ---This gives 159 NULL values which will be populated
FROM [PortfolioProject2].[dbo].[NashvilleHousing]
WHERE Property_Address IS NULL;

----

SELECT Parcel_ID, Property_Address            ---This query show that some rows with similar Parcel ID has the same Property_Address
FROM [PortfolioProject2].[dbo].[NashvilleHousing]
ORDER BY Parcel_ID

----- This relationship was used to populate the NULL values for Property_Address. This was done using SELF JOIN of the table with this query
SELECT a.Parcel_ID, a.Property_Address, b.Parcel_ID, b.Property_Address, ISNULL(a.Property_Address, b.Property_Address)
FROM [PortfolioProject2].[dbo].[NashvilleHousing] AS a
JOIN [PortfolioProject2].[dbo].[NashvilleHousing] AS b
	ON a.Parcel_ID = b.Parcel_ID
	AND a.Unique_ID <> b.Unique_ID
WHERE a.Property_Address IS NULL


---I can now update the table with this.
UPDATE a
SET Property_Address = ISNULL(a.Property_Address, b.Property_Address)
FROM [PortfolioProject2].[dbo].[NashvilleHousing] AS a
JOIN [PortfolioProject2].[dbo].[NashvilleHousing] AS b
	ON a.Parcel_ID = b.Parcel_ID
	AND a.Unique_ID <> b.Unique_ID
WHERE a.Property_Address IS NULL


---Populate Null Property_City Data

SELECT Property_City
FROM [PortfolioProject2].[dbo].[NashvilleHousing]
WHERE Property_City IS NULL


--Count Property_City Null Data
SELECT COUNT(*) AS [Property_City Null Values]				---This gives 159 NULL values which will be populated
FROM [PortfolioProject2].[dbo].[NashvilleHousing]
WHERE Property_City IS NULL;


SELECT Parcel_ID, Property_City			---This query show that some rows with similar Parcel ID has the same Property_City
FROM [PortfolioProject2].[dbo].[NashvilleHousing]
ORDER BY Parcel_ID


----- This relationship was used to populate the NULL values for Property_City. This was done using SELF JOIN of the table with this query
SELECT a.Parcel_ID, a.Property_City, b.Parcel_ID, b.Property_City, ISNULL(a.Property_City, b.Property_City)
FROM [PortfolioProject2].[dbo].[NashvilleHousing] AS a
JOIN [PortfolioProject2].[dbo].[NashvilleHousing] AS b
	ON a.Parcel_ID = b.Parcel_ID
	AND a.Unique_ID <> b.Unique_ID
WHERE a.Property_City IS NULL


---I can now update the table with this.
UPDATE a
SET Property_City = ISNULL(a.Property_City, b.Property_City)
FROM [PortfolioProject2].[dbo].[NashvilleHousing] AS a
JOIN [PortfolioProject2].[dbo].[NashvilleHousing] AS b
	ON a.Parcel_ID = b.Parcel_ID
	AND a.Unique_ID <> b.Unique_ID
WHERE a.Property_City IS NULL



---Change abbreviations and correct misspellings in Column Land Use

SELECT DISTINCT (Land_Use)	   ---This was used to know the distinc values in the column so as to confirm the ones with issues
FROM [PortfolioProject2].[dbo].[NashvilleHousing]
GROUP BY Land_Use
ORDER BY Land_Use DESC

---I counted the number of each distinct value so that I can confirm the change after replacing them
SELECT DISTINCT (Land_Use), COUNT (Land_Use) AS Land_Use_Count
FROM [PortfolioProject2].[dbo].[NashvilleHousing]
GROUP BY Land_Use
ORDER BY Land_Use DESC


SELECT Land_Use,			-- CASE statement was used to replace the misspelled and abbreviated values 
CASE
	WHEN Land_Use = 'VACANT RESIENTIAL LAND' THEN 'VACANT RESIDENTIAL LAND'
	WHEN Land_Use = 'VACANT RES LAND' THEN 'VACANT RESIDENTIAL LAND'
	ELSE Land_Use
END
FROM [PortfolioProject2].[dbo].[NashvilleHousing]
WHERE Land_Use = 'VACANT RES LAND' OR Land_Use = 'VACANT RESIENTIAL LAND'
ORDER BY Land_Use DESC


---I can now update the table with this.
UPDATE [PortfolioProject2].[dbo].[NashvilleHousing]
SET Land_Use = CASE
	WHEN Land_Use = 'VACANT RESIENTIAL LAND' THEN 'VACANT RESIDENTIAL LAND'
	WHEN Land_Use = 'VACANT RES LAND' THEN 'VACANT RESIDENTIAL LAND'
	ELSE Land_Use
END



---Remove Duplicates

---I will use CTE and some WINDOWS functions to find and remove where there are duplicated values
SELECT*,				 -- Firstly is to identify the duplicated rows, I used Row Number for this.
ROW_NUMBER() OVER(
PARTITION BY Parcel_ID,
Property_Address,
Sale_Price,
Sale_Date,
Legal_Reference
ORDER BY Unique_ID
) AS RowNum
FROM [PortfolioProject2].[dbo].[NashvilleHousing]
ORDER BY Parcel_ID


-- I will write this into CTE
WITH RowNumCTE AS (
SELECT*,
ROW_NUMBER() OVER(
PARTITION BY Parcel_ID,
Property_Address,
Sale_Price,
Sale_Date,
Legal_Reference
ORDER BY Unique_ID
) AS RowNum
FROM [PortfolioProject2].[dbo].[NashvilleHousing]
--ORDER BY Parcel_ID					--(The ORDER BY clause does not work in CTE)
)
SELECT*
FROM RowNumCTE
WHERE RowNum >1
ORDER BY Property_Address


---This was used to Delete the duplicated rows
WITH RowNuMCTE AS (
SELECT*,
ROW_NUMBER() OVER(
PARTITION BY Parcel_ID,
Property_Address,
Sale_Price,
Sale_Date,
Legal_Reference
ORDER BY Unique_ID
) AS RowNum
FROM [PortfolioProject2].[dbo].[NashvilleHousing]
--ORDER BY Parcel_ID
)
DELETE
FROM RowNumCTE
WHERE RowNum >1



---Delete Unused Columns

SELECT *
FROM [PortfolioProject2].[dbo].[NashvilleHousing]

Alter Table [PortfolioProject2].[dbo].[NashvilleHousing]
DROP COLUMN Sale_Date
