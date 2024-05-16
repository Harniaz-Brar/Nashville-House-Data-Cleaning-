
Nashville Housing Data Cleaning Project
Overview
This project involves cleaning and transforming a dataset related to housing in Nashville. The primary goal is to ensure the dataset is in a consistent and usable format for analysis. The tasks performed include standardizing date formats, populating missing property addresses, breaking down addresses into individual columns, updating specific field values, removing duplicates, and deleting unused columns.

Project Structure
SQL Scripts: Contains the SQL queries used for data cleaning.
Data: Original and cleaned datasets (not included in the repository due to size constraints, but instructions for importing the data are provided).
Data Cleaning Steps
1. Standardize Date Format
Convert the SaleDate column to a standard date format.

-- Convert SaleDate to Date format
ALTER TABLE NashvilleHousing
ADD SaleDateConverted DATE;

UPDATE NashvilleHousing
SET SaleDateConverted = CONVERT(DATE, SaleDate);

2. Populate Property Address Data
Fill in missing PropertyAddress values by using matching ParcelID.

-- Populate missing PropertyAddress
UPDATE a
SET PropertyAddress = ISNULL(a.PropertyAddress, b.PropertyAddress)
FROM PortfolioProject.dbo.NashvilleHousing a
JOIN PortfolioProject.dbo.NashvilleHousing b
ON a.ParcelID = b.ParcelID
AND a.[UniqueID] <> b.[UniqueID]
WHERE a.PropertyAddress IS NULL;

3. Break Out Address into Individual Columns
Split the PropertyAddress and OwnerAddress into separate columns for address, city, and state.

-- Break out PropertyAddress into Address and City
ALTER TABLE NashvilleHousing
ADD PropertySplitAddress NVARCHAR(255), PropertySplitCity NVARCHAR(255);

UPDATE NashvilleHousing
SET PropertySplitAddress = SUBSTRING(PropertyAddress, 1, CHARINDEX(',', PropertyAddress) - 1),
    PropertySplitCity = SUBSTRING(PropertyAddress, CHARINDEX(',', PropertyAddress) + 1, LEN(PropertyAddress));


Memory updated
Here's a README file for your Nashville Housing Data Cleaning Project on GitHub:

Nashville Housing Data Cleaning Project
Overview
This project involves cleaning and transforming a dataset related to housing in Nashville. The primary goal is to ensure the dataset is in a consistent and usable format for analysis. The tasks performed include standardizing date formats, populating missing property addresses, breaking down addresses into individual columns, updating specific field values, removing duplicates, and deleting unused columns.

Project Structure
SQL Scripts: Contains the SQL queries used for data cleaning.
Data: Original and cleaned datasets (not included in the repository due to size constraints, but instructions for importing the data are provided).
Data Cleaning Steps
1. Standardize Date Format
Convert the SaleDate column to a standard date format.

sql
Copy code
-- Convert SaleDate to Date format
ALTER TABLE NashvilleHousing
ADD SaleDateConverted DATE;

UPDATE NashvilleHousing
SET SaleDateConverted = CONVERT(DATE, SaleDate);
2. Populate Property Address Data
Fill in missing PropertyAddress values by using matching ParcelID.

sql
Copy code
-- Populate missing PropertyAddress
UPDATE a
SET PropertyAddress = ISNULL(a.PropertyAddress, b.PropertyAddress)
FROM PortfolioProject.dbo.NashvilleHousing a
JOIN PortfolioProject.dbo.NashvilleHousing b
ON a.ParcelID = b.ParcelID
AND a.[UniqueID] <> b.[UniqueID]
WHERE a.PropertyAddress IS NULL;
3. Break Out Address into Individual Columns
Split the PropertyAddress and OwnerAddress into separate columns for address, city, and state.

sql
Copy code
-- Break out PropertyAddress into Address and City
ALTER TABLE NashvilleHousing
ADD PropertySplitAddress NVARCHAR(255), PropertySplitCity NVARCHAR(255);

UPDATE NashvilleHousing
SET PropertySplitAddress = SUBSTRING(PropertyAddress, 1, CHARINDEX(',', PropertyAddress) - 1),
    PropertySplitCity = SUBSTRING(PropertyAddress, CHARINDEX(',', PropertyAddress) + 1, LEN(PropertyAddress));
4. Update "Sold as Vacant" Field
Convert Y and N values to Yes and No in the SoldAsVacant column.

sql
Copy code
-- Convert SoldAsVacant values
UPDATE NashvilleHousing
SET SoldAsVacant = CASE 
    WHEN SoldAsVacant = 'Y' THEN 'Yes'
    WHEN SoldAsVacant = 'N' THEN 'No'
    ELSE SoldAsVacant
END;
5. Remove Duplicates
Identify and remove duplicate records based on key fields.

sql
Copy code
-- Remove duplicates
WITH RowNumCTE AS (
    SELECT *,
        ROW_NUMBER() OVER (
            PARTITION BY ParcelID, PropertyAddress, SalePrice, SaleDate, LegalReference
            ORDER BY UniqueID
        ) AS row_num
    FROM PortfolioProject.dbo.NashvilleHousing
)
DELETE FROM RowNumCTE
WHERE row_num > 1;
6. Delete Unused Columns
Remove columns that are no longer needed in the dataset.

sql
Copy code
-- Drop unused columns
ALTER TABLE PortfolioProject.dbo.NashvilleHousing
DROP COLUMN OwnerAddress, TaxDistrict, PropertyAddress, SaleDate;
Importing Data
To import data using OPENROWSET and BULK INSERT, ensure the server is configured properly.

sql
Copy code
-- Example: BULK INSERT
BULK INSERT NashvilleHousing
FROM 'C:\Path\To\Data\Nashville Housing Data.csv'
WITH (
    FIELDTERMINATOR = ',',
    ROWTERMINATOR = '\n'
);

Usage
Clone the repository.
Open the SQL scripts in your SQL Server Management Studio.
Execute the scripts in the provided order.
