# World-happiness-score-
# 1] Import libraries
import pandas as pd
import numpy as np
import seaborn as sns
import matplotlib.pyplot as plt

# 2] Load the Dataset
df =
pd.read_csv('/content/2015_.csv')

# 3] Initial Data Exploration
print("First few rows :")
print(df.head())   # First few rows

print("Last few rows :")
print(df.tail())   # Last few rows

print("Shape of the Data :")
print(df.shape)  # Shape of the data

# 4] Summary of Data
print("Column names, Data types, and not-nul counts :")
print(df.info())

print("Numeric columns summary :")
print(df.describe())

# 5]Check for missing value
missing_values = df.isnull().sum()
print("Missing values in each column :")
print(missing_values)

# 6] Handle Missing Values
numeric_cols = df.select_dtypes(include=np.number).columns # Select only numeric columns
df[numeric_cols] = df[numeric_cols].fillna(df[numeric_cols].mean()) # Fill NaNs in numeric cols with their mean

# 7] Check for duplicates
print("Number of Duplicate rows :")
print(df.duplicated().sum())

# 8] Rename Columns
df.rename(columns = {"Happiness Score": "Happiness_Score"}, inplace=True)
print(df.head())

# 9] Identify Outliers (using box_plot)
plt.figure(figsize=(10, 6))
sns.boxplot(data=df[['Happiness_Score']])
plt.title('Box Plot of Happiness Score')
plt.show()

# 10] Data Transformation
# Example: Log transformation on a skewed column
df['Happiness_Score'] = np.log(df['Happiness_Score'])
print(df.head())

# 11] Data Visualization
plt.figure(figsize=(10, 6))
sns.histplot(df['Happiness_Score'], bins=30, kde=True)  # Histogram of Happiness Score
plt.title('Distribution of Happiness Score')
plt.show()

# 12] Correlation Analysis
numerical_df = df.select_dtypes(include=np.number)  # Select numerical columns only

correlation_matrix = numerical_df.corr()

plt.figure(figsize=(12, 8))
sns.heatmap(correlation_matrix, annot=True, fmt=".2f", cmap='coolwarm')  # Correlation heatmap
plt.title('Correlation Matrix')
plt.show()

# 13] Pair Plot
sns.pairplot(df)  # Pair plot to visualize relationships
plt.show()

# 14] Grouping and Aggregation
grouped_data = df.groupby('Region')['Happiness_Score'].mean().reset_index()
print(grouped_data)

# Sort by Happiness Score
top10 = df.sort_values('Happiness_Score', ascending=False).head(10)

# Bar plot
plt.figure(figsize=(10,6))
sns.barplot(data=top10, x='Happiness_Score', y='Country', palette='viridis')
plt.title('Top 10 Happiest Countries')
plt.xlabel('Happiness Score')
plt.ylabel('Country')
plt.show()
