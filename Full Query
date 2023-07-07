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

ALTER TABLE [PortfolioProject2].[dbo].[NashvilleHousing]
ADD Sale_Date2 Date;

UPDATE [PortfolioProject2].[dbo].[NashvilleHousing]
SET Sale_Date2 = TRY_CONVERT (date, Sale_Date)

SELECT Sale_Date, Sale_Date2
FROM [PortfolioProject2].[dbo].[NashvilleHousing]


---Populate Null Property_Address Data

SELECT Property_Address
FROM [PortfolioProject2].[dbo].[NashvilleHousing]
WHERE Property_Address IS NULL


--Count Property_Address Null Data

SELECT COUNT(*) AS [Property_Address Null Values]
FROM [PortfolioProject2].[dbo].[NashvilleHousing]
WHERE Property_Address IS NULL;

----

SELECT Parcel_ID, Property_Address
FROM [PortfolioProject2].[dbo].[NashvilleHousing]
ORDER BY Parcel_ID



SELECT a.Parcel_ID, a.Property_Address, b.Parcel_ID, b.Property_Address, ISNULL(a.Property_Address, b.Property_Address)
FROM [PortfolioProject2].[dbo].[NashvilleHousing] AS a
JOIN [PortfolioProject2].[dbo].[NashvilleHousing] AS b
	ON a.Parcel_ID = b.Parcel_ID
	AND a.Unique_ID <> b.Unique_ID
WHERE a.Property_Address IS NULL



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

SELECT COUNT(*) AS [Property_City Null Values]
FROM [PortfolioProject2].[dbo].[NashvilleHousing]
WHERE Property_City IS NULL;




SELECT Parcel_ID, Property_City
FROM [PortfolioProject2].[dbo].[NashvilleHousing]
ORDER BY Parcel_ID


SELECT a.Parcel_ID, a.Property_City, b.Parcel_ID, b.Property_City, ISNULL(a.Property_City, b.Property_City)
FROM [PortfolioProject2].[dbo].[NashvilleHousing] AS a
JOIN [PortfolioProject2].[dbo].[NashvilleHousing] AS b
	ON a.Parcel_ID = b.Parcel_ID
	AND a.Unique_ID <> b.Unique_ID
WHERE a.Property_City IS NULL



UPDATE a
SET Property_City = ISNULL(a.Property_City, b.Property_City)
FROM [PortfolioProject2].[dbo].[NashvilleHousing] AS a
JOIN [PortfolioProject2].[dbo].[NashvilleHousing] AS b
	ON a.Parcel_ID = b.Parcel_ID
	AND a.Unique_ID <> b.Unique_ID
WHERE a.Property_City IS NULL




---Change abbreviations and correct misspellings in Column Land Use


SELECT DISTINCT (Land_Use)
FROM [PortfolioProject2].[dbo].[NashvilleHousing]
GROUP BY Land_Use
ORDER BY Land_Use DESC



SELECT DISTINCT (Land_Use), COUNT (Land_Use) AS Land_Use_Count
FROM [PortfolioProject2].[dbo].[NashvilleHousing]
GROUP BY Land_Use
ORDER BY Land_Use DESC



SELECT Land_Use,
CASE
	WHEN Land_Use = 'VACANT RESIENTIAL LAND' THEN 'VACANT RESIDENTIAL LAND'
	WHEN Land_Use = 'VACANT RES LAND' THEN 'VACANT RESIDENTIAL LAND'
	ELSE Land_Use
END
FROM [PortfolioProject2].[dbo].[NashvilleHousing]
WHERE Land_Use = 'VACANT RES LAND' OR Land_Use = 'VACANT RESIENTIAL LAND'
ORDER BY Land_Use DESC



UPDATE [PortfolioProject2].[dbo].[NashvilleHousing]
SET Land_Use = CASE
	WHEN Land_Use = 'VACANT RESIENTIAL LAND' THEN 'VACANT RESIDENTIAL LAND'
	WHEN Land_Use = 'VACANT RES LAND' THEN 'VACANT RESIDENTIAL LAND'
	ELSE Land_Use
END




---Remove Duplicates

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
ORDER BY Parcel_ID




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
--ORDER BY Parcel_ID
)
SELECT*
FROM RowNumCTE
WHERE RowNum >1
ORDER BY Property_Address


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
