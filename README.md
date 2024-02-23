# Data-science-data-clearing

## Content 
- [Overview](#overview)
- [Data Preparation](#data-preparation)
- [Data Exploration](#data-exploration)
  
## Overview

This project aims to provide basic knowledge of data cleaning and various methods for reading and exploring data. In Task 1, several problems in the dataset were identified and resolved. Furthermore, Task 2 involved exploring the cleaned dataset using basic methods.


## Task 1 Data Preparation

Task 1 aims to find several problems in given dataset. 5 problems were found in the given dataset which were duplicated rows,incontient,spelling mistake,irregular spacing and missing value.

### Way to fix the duplicated rows
```
data.columns = data.columns.str.strip()
data.drop_duplicates(inplace=True)
```
### Way to fix the incontient
```
data = data.applymap(lambda x: x.replace("%%", "%") if isinstance(x, str) else x)
data.loc[8, 'Time period'] = data.loc[8, 'Time period'].replace('-12', '')
```
### Way to fix the spelling mistake
```
data['Colum name'] = data['colum name'].str.replace('miss spelled data', 'target speleed data', regex=True)
```
### Way to fix the irregular spacing
```
data = data.applymap(lambda x: x.lstrip() if isinstance(x, str) else x)
```
### Way to fix the missing value

Find missing values
```
missing_values = data.isnull().sum()
```
Calculate mean value
```
Column_G_Median = data['Column_G'].median()
```
Change missing values to mean value
```
data['Rural (Residence)'].fillna(Column_G_Median, inplace=True)
```

## Task 2 Data Exploration

### Side by side box plot

A box plot is generated based on the quantitative variable, including Q1, Q2, Q3, and Q4, to help analyze the extent of the data distribution. This box plot compares four different columns: 'High income group,' 'Low income group,' 'Lower middle income group,' and 'Upper middle income group.

<img width="586" alt="image" src="https://github.com/hyeonBin7201/Data-science-data-clearing/assets/152157238/2e019712-5fb4-4670-8a67-5d56e99f0671">

The key code to genrate box plot is provided below
```
data['Total'] = data['Total'].str.rstrip('%').astype(int)
data.dropna().boxplot(column='Total',by='Income Group')
plt.show()
```
### Compare mean and median value

This part is comparing mean and median values of the four different categories' frequency values.

<img width="624" alt="image" src="https://github.com/hyeonBin7201/Data-science-data-clearing/assets/152157238/cda3db82-6d99-42fe-b6d6-efebba5cbe2f">

The key code to compare the mean values is provided below.
```
data.groupby('Income Group')['Total'].mean()
```

The key code to compare the median values is provided below.
```
copy_data['Rural (Residence)'].median()
copy_data['Urban (Residence)'].median()
```

### Displaying top 10 countries

Based on the median values, the list of top 10 countries are displayed. Two different countries are displayed based on the rural area and urban area.

<img width="559" alt="image" src="https://github.com/hyeonBin7201/Data-science-data-clearing/assets/152157238/a66cac1e-c560-4d65-a8f1-07bdf63665cc">

The key code to generate the list of the top 10 countries is provided below.

```
top_rural_countries = copy_data.nlargest(10, 'Rural (Residence)')[['Countries and areas', 'Rural (Residence)']]

top_urban_countries = copy_data.nlargest(10, 'Urban (Residence)')[['Countries and areas', 'Urban (Residence)']]
```


### Compare the percentage of the different categories

In this part the poorest group and richest group were comapred based on the three different statistic which are the mean, median, and standard deviation
The key code to generate these statistic measures is provided below
```
um_data = copy_data[copy_data['Income Group'] == 'Upper middle income (UM)']
percentage_columns = ['Poorest (Wealth quintile)', 'Richest (Wealth quintile)']
statistics = um_data[percentage_columns].describe()

print("Poorest (Wealth quintile) statistics:")
print(statistics.loc[['mean', '50%', 'std'], 'Poorest (Wealth quintile)'])

print("\n'Richest (Wealth quintile) statistics:")
print(statistics.loc[['mean', '50%', 'std'], 'Richest (Wealth quintile)'])
```


