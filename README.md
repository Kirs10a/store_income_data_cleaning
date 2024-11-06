# store_income_data_cleaning
The primary objectives are to standardize country names, parse date values, and calculate additional features like the number of days since a particular measurement date. This project demonstrates data cleaning, string manipulation, and date calculations in Python.
Files:

store_income_task.ipynb: Jupyter Notebook containing code and explanations for data cleaning and transformations.
store_income_data_task.csv: The dataset used in this analysis, containing columns such as id, store_name, store_email, department, income, date_measured, and country.

Requirements:
pandas: For data manipulation and analysis.
fuzzywuzzy: For fuzzy matching and standardizing country names.
chardet: To handle character encoding detection (if necessary).
datetime: For date calculations.
Notebook Outline and Documentation
Loading Libraries and Dataset

Import necessary libraries: pandas, fuzzywuzzy, chardet, and datetime.
Load the dataset (store_income_data_task.csv) into a DataFrame.

python
import pandas as pd
from fuzzywuzzy import process
import chardet
from datetime import datetime

# Load the dataset
income_df = pd.read_csv('store_income_data_task.csv')
Data Inspection

Inspect unique values in the country column to understand the variations that need standardization.
Convert country values to lowercase and remove trailing whitespace for uniformity.
python
# Inspect unique country values
print(income_df['country'].unique())

# Clean up country column
income_df['country'] = income_df['country'].str.lower().str.strip()
Standardize Country Names

Use fuzzywuzzy for fuzzy matching to group similar country names under three distinct categories: united kingdom, united states, and south africa.
python

# Define a function to standardize country names using fuzzy matching
def standardize_country(name):
    country_choices = ["united kingdom", "united states", "south africa"]
    match, score = process.extractOne(name, country_choices)
    return match if score > 80 else name  # Only replace if confidence is high

# Apply the function
income_df['country'] = income_df['country'].apply(standardize_country)
Calculate days_ago Column

Parse the date_measured column as a datetime object.
Calculate the number of days from the measurement date to today, storing this value in a new column days_ago.
python
# Parse date_measured column as datetime
income_df['date_measured'] = pd.to_datetime(income_df['date_measured'], errors='coerce')

# Calculate days ago
today = datetime.now().date()
income_df['days_ago'] = (today - income_df['date_measured'].dt.date).dt.days
Analysis and Conclusion

Summarize findings and document any challenges encountered during data cleaning, such as missing values or encoding issues.
Use markdown cells in the notebook to answer questions about the transformations applied.
Example Markdown Cells
Purpose of the Project:
"This project focuses on cleaning and transforming a dataset related to store incomes. The data preprocessing involves cleaning up country names, handling date values, and calculating the number of days since each measurement. This prepares the dataset for further analysis."

Observations on Country Standardization:
"After applying fuzzy matching, we standardized country names to only three categories: united kingdom, united states, and south africa. This step reduces ambiguity in country data and ensures consistency."

Conclusion on Date Calculations:
"The days_ago column provides insights into how recent each measurement is, which could be useful in trend analysis or time-based reporting."
