# CMSE 202 Final Project
### Group Members: Brady, Toan, Sophie, Sultan, and Andy
November 19th, 2024

## How Treasury Yields Predict Economic Recessions
### Background & Motivations
This project explores the potential of Treasury yields as indicators of economic downturns. With a focus on the spread between long-term and short-term yields, we investigate whether these financial metrics can signal impending recessions. Our aim is to provide valuable insights for investors and individuals interested in economic forecasting, especially in today's unpredictable financial climate.

### Data Collection and Cleaning
We collected data primarily from US Government sources, focusing on Treasury yield trends across various maturity dates, alongside US GDP data. The data was obtained in CSV format, and the following datasets were used:

Treasury Yields:
1-Year Yield: DGS1.csv
10-Year Yield: DGS10.csv
1-Month Yield: DGS1MO.csv
20-Year Yield: DGS20.csv
30-Year Yield: DGS30.csv
3-Month Yield: DGS3MO.csv
6-Month Yield: DGS6MO.csv

### Code Implementation
The following Python packages were imported for the project:

import pandas as pd
import matplotlib.pyplot as plt
import numpy as np

We noticed that the yield data had varying start years. To ensure proper analysis, we filtered the data to align timelines. Notably, the one-month yield data started from 2002. We also addressed missing values during the cleaning process:


# Function to clean datasets
def clean_dataset(df, target_column):
    # Filter by date
    df = df[df['DATE'] >= '2002-01-01']
    
    # Convert the 'DATE' column to DateTime format 
    df['DATE'] = pd.to_datetime(df['DATE'])
    
    # Replace '.' with NaN
    df[target_column] = df[target_column].replace('.', np.nan)
    
    # Convert the column to numeric
    df[target_column] = pd.to_numeric(df[target_column], errors='coerce')
    
    # Drop NaN values
    df.dropna(subset=[target_column], inplace=True)
    
    return df
    
Each dataset was cleaned and prepared for analysis. Below is the example code used to clean the datasets for different yield types:

yr1 = clean_dataset(pd.read_csv('data/DGS1.csv'), 'DGS1')
yr10 = clean_dataset(pd.read_csv('data/DGS10.csv'), 'DGS10')
mo1 = clean_dataset(pd.read_csv('data/DGS1MO.csv'), 'DGS1MO')
yr20 = clean_dataset(pd.read_csv('data/DGS20.csv'), 'DGS20')
yr30 = clean_dataset(pd.read_csv('data/DGS30.csv'), 'DGS30')
mo3 = clean_dataset(pd.read_csv('data/DGS3MO.csv'), 'DGS3MO')
mo6 = clean_dataset(pd.read_csv('data/DGS6MO.csv'), 'DGS6MO')

### Data Visualization
To visualize the monthly yield averages starting from 2002, we resampled the data and plotted it. This visualization effectively illustrates trends over time:

# Resample to monthly frequency, taking the mean of each month
yr1_monthly = yr1.resample('ME', on='DATE').mean()
yr10_monthly = yr10.resample('ME', on='DATE').mean()

# Additional monthly resampling...

# Plotting
plt.figure(figsize=(12, 6))
plt.plot(yr1_monthly.index, yr1_monthly['DGS1'], label='1-Year Yield')
plt.plot(yr10_monthly.index, yr10_monthly['DGS10'], label='10-Year Yield')

# Additional yield plots...

plt.title('Treasury Yields Over Time (2002 - Present)')
plt.xlabel('Date')
plt.ylabel('Yield (%)')
plt.legend()
plt.show()

Additional Data
We also collected quarterly GDP data as it serves as a key indicator of economic recessions:

gdp = pd.read_csv('data/GDP.csv')
