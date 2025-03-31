# Data-wrangling-assignment-
Data wrangling assignment of Sakshi S Bhat- 2345045




import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns
from sklearn.preprocessing import MinMaxScaler


sns.set(style="whitegrid")

# Load the dataset
file_path = '/mnt/data/orders.xlsx'
df = pd.read_csv('D:\\hahahaha//.ORDERS.csv)


print('First 5 Rows:')
print(df.head())

print('\nData Types:')
print(df.dtypes)

num_rows, num_columns = df.shape
print(f'\nNumber of Rows: {num_rows}, Number of Columns: {num_columns}')

missing_values = df.isnull().sum()
print('\nMissing Values:')
print(missing_values)

print('\nDescriptive Statistics:')
print(df.describe())


df['Order Date'] = pd.to_datetime(df['Order Date'], errors='coerce')
df['Ship Date'] = pd.to_datetime(df['Ship Date'], errors='coerce')

print('\nData Types After Conversion:')
print(df.dtypes)


print('\nNumber of Duplicate Rows Before Dropping:')
print(df.duplicated().sum())

df.drop_duplicates(inplace=True)

print('\nNumber of Duplicate Rows After Dropping:')
print(df.duplicated().sum())



df['Customer Name'] = df['Customer Name'].str.title()
df['City'] = df['City'].str.title()
df['State'] = df['State'].str.title()
df['Category'] = df['Category'].str.title()
df['Sub-Category'] = df['Sub-Category'].str.title()


q_upper = df['Quantity'].quantile(0.99)
df['Quantity'] = np.where(df['Quantity'] > q_upper, q_upper, df['Quantity'])

print('\nOutliers in Quantity column capped at 99th percentile.')

df['Order Year'] = df['Order Date'].dt.year
df['Order Month'] = df['Order Date'].dt.month

df['Profit Margin'] = (df['Profit'] / df['Sales']) * 100
df['Profit Margin'] = df['Profit Margin'].round(2)

=sales_by_customer = df.groupby('Customer ID')['Sales'].sum().reset_index()

profit_by_category = df.groupby('Category')['Profit'].sum().reset_index()

scaler = MinMaxScaler()
df['Sales_Normalized'] = scaler.fit_transform(df[['Sales']])


sales_bins = [0, 500, 2000, df['Sales'].max()]
sales_labels = ['Low', 'Medium', 'High']
df['Sales_Bin'] = pd.cut(df['Sales'], bins=sales_bins, labels=sales_labels)

print('\nData Transformation Completed Successfully.')
