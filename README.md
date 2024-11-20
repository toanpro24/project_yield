# CMSE 202 Final Project
### Group Members: Brady, Toan, Sophie, Sultan, and Andy
November 19th, 2024

## How Treasury Yields Predict Economic Recessions
### Abstract
This project examines whether U.S. Treasury yield spreads can predict economic recessions, providing valuable insights for investors and policymakers. Motivated by our interest in financial markets and personal finance, we explore if the spread between long-term and short-term Treasury yields offers early signals of impending recessions. Historically, such yield spreads have been considered recession indicators, and we aim to uncover patterns that enhance economic forecasting in today's unpredictable financial climate.

We collected U.S. Treasury yield data across various maturities—from 1-month to 30-year bonds—covering the period from 2002 to 2024, along with quarterly GDP data as an economic benchmark since recessions are typically defined by two consecutive quarters of GDP contraction. Due to inconsistencies in start dates and missing values, the data required thorough cleaning and standardization to ensure accurate analysis.

Our methodology involved calculating yield spreads to assess their potential as recession indicators, focusing on the 10-year minus 3-month (10Y-3MO) spread, which prior studies have identified as significant. We also examined other spreads like the 10Y-1Y and 20Y-10Y to explore their predictive capabilities. The yield data was resampled to quarterly averages to align with GDP data frequency. GDP growth was categorized into positive and negative periods, assigning values of 2 for positive growth and -1 for negative growth to facilitate correlation analysis.

Data visualization played a crucial role in our analysis. By plotting the yield spreads over time alongside GDP growth rates, we observed that the 10Y-3MO yield spread turned negative before the recessions of 2007–2008 and 2019–2020, consistent with an inverted yield curve scenario. An inverted yield curve, where short-term interest rates exceed long-term rates, often signals investor pessimism about the near-term economy and expectations of declining interest rates due to anticipated monetary policy easing.

To further explore the predictive relationship, we analyzed the correlation between yield spreads and GDP growth. Applying the Efficient Market Hypothesis—which posits that markets are forward-looking and current asset prices reflect all available information—we shifted the yield spread data forward by two quarters. This adjustment improved the correlation between the inverted yield spread and subsequent GDP contraction, reinforcing the validity of the 10Y-3MO spread as a leading recession indicator.

Our findings show that the inversion of the 10Y-3MO yield spread consistently preceded economic downturns, suggesting it can serve as an early warning signal. Moreover, when the inverted yield curve begins to revert to normal (uninvert), it often signals an approaching recession as investors adjust their expectations based on changing economic conditions.

To extend our analysis into practical forecasting, we incorporated forward-looking data using Secured Overnight Financing Rate (SOFR) future rates, reflecting traders' expectations of future interest rates. The projected data indicates that the currently inverted yield curve may uninvert in the coming months. Historically, such a pattern has been associated with imminent recessions, suggesting the economy may face a downturn soon. This prospective analysis underscores the practical applicability of our findings for strategic financial planning.

In conclusion, our project demonstrates that Treasury yield spreads, especially the 10Y-3MO spread, can reliably predict economic recessions. The consistent inversion pattern prior to recessions and the improved correlation after adjusting for market expectations provide robust evidence of this relationship. These insights are valuable for investors, policymakers, and individuals making informed decisions based on economic trends. Understanding and leveraging such predictive indicators is crucial for economic stability and growth.

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
