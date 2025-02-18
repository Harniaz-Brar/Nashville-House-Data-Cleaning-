# Nashville Housing Data Cleaning Project
## Overview
This project involves cleaning and transforming a dataset related to housing in Nashville. The primary goal is to ensure the dataset is in a consistent and usable format for analysis.

## Data Cleaning Steps
Standardize Date Format: Convert the SaleDate column to a standard date format.
Populate Property Address Data: Fill in missing PropertyAddress values using matching ParcelID.
Break Out Address into Individual Columns: Split the PropertyAddress and OwnerAddress into separate columns for address, city, and state.
Update "Sold as Vacant" Field: Convert Y and N values to Yes and No in the SoldAsVacant column.
Remove Duplicates: Identify and remove duplicate records based on key fields.
Delete Unused Columns: Remove columns that are no longer needed in the dataset.

## Importing Data
To import data using OPENROWSET and BULK INSERT, ensure the server is configured properly.

## Usage
1. Clone the repository to your local machine using `git clone`
2. Open the `.sql` file using SQL Server Management Studio or any SQL IDE of your choice.
3. Run the script in an environment connected to a SQL database.
