# Ex-07-Data-Visualization-

## AIM
To Perform Data Visualization on the given dataset and save the data to a file. 

# Explanation
Data visualization is the graphical representation of information and data. By using visual elements like charts, graphs, and maps, data visualization tools provide an accessible way to see and understand trends, outliers, and patterns in data.

# ALGORITHM
### STEP 1
Read the given Data
### STEP 2
Clean the Data Set using Data Cleaning Process
### STEP 3
Apply Feature generation and selection techniques to all the features of the data set
### STEP 4
Apply data visualization techniques to identify the patterns of the data.


# CODE
```
#DEVELOPED BY:SWETHA P 
#REGISTER NUMBER: 212222100053
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns
df=pd.read_csv("Superstore.csv",encoding='unicode_escape')
df
df.head()
df.info()
df.drop('Row ID',axis=1,inplace=True)
df.drop('Order ID',axis=1,inplace=True)
df.drop('Customer ID',axis=1,inplace=True)
df.drop('Customer Name',axis=1,inplace=True)
df.drop('Country',axis=1,inplace=True)
df.drop('Postal Code',axis=1,inplace=True)
df.drop('Product ID',axis=1,inplace=True)
df.drop('Product Name',axis=1,inplace=True)
df.drop('Order Date',axis=1,inplace=True)
df.drop('Ship Date',axis=1,inplace=True)
print("Updated dataset")
df
df.isnull().sum()
#detecting and removing outliers in current numeric data
import matplotlib.pyplot as plt
plt.figure(figsize=(8,8))
plt.title("Data with outliers")
df.boxplot()
plt.show()
plt.figure(figsize=(8,8))
cols = ['Sales','Quantity','Discount','Profit']
Q1 = df[cols].quantile(0.25)
Q3 = df[cols].quantile(0.75)
IQR = Q3 - Q1
df = df[~((df[cols] < (Q1 - 1.5 * IQR)) |(df[cols] > (Q3 + 1.5 * IQR))).any(axis=1)]
plt.title("Dataset after removing outliers")
df.boxplot()
plt.show()
#Which Segment has Highest sales?
sns.lineplot(x="Segment",y="Sales",data=df,marker='o')
plt.title("Segment vs Sales")
plt.xticks(rotation = 90)
plt.show()
sns.barplot(x="Segment",y="Sales",data=df)
plt.xticks(rotation = 90)
plt.show()
#Which City has Highest profit?
df.shape
df1 = df[(df.Profit >= 60)]
df1.shape
plt.figure(figsize=(30,8))
states=df1.loc[:,["City","Profit"]]
states=states.groupby(by=["City"]).sum().sort_values(by="Profit")
sns.barplot(x=states.index,y="Profit",data=states)
plt.xticks(rotation = 90)
plt.xlabel=("City")
plt.ylabel=("Profit")
plt.show()
#Which ship mode is profitable?
sns.barplot(x="Ship Mode",y="Profit",data=df)
plt.show()
sns.lineplot(x="Ship Mode",y="Profit",data=df)
plt.show()
sns.violinplot(x="Profit",y="Ship Mode",data=df)
plt.show()
sns.pointplot(x=df["Profit"],y=df["Ship Mode"])
plt.show()
#Sales of the product based on region
states=df.loc[:,["Region","Sales"]]
states=states.groupby(by=["Region"]).sum().sort_values(by="Sales")
sns.barplot(x=states.index,y="Sales",data=states)
plt.xticks(rotation = 90)
plt.xlabel=("Region")
plt.ylabel=("Sales")
plt.show()
df.groupby(['Region']).sum().plot(kind='pie', y='Sales',figsize=
(6,9),pctdistance=1.7,labeldistance=1.2)
#Find the relation between sales and profit.
df["Sales"].corr(df["Profit"])
df_corr = df.copy()
df_corr = df_corr[["Sales","Profit"]]
df_corr.corr()
sns.pairplot(df_corr, kind="scatter")
plt.show()
#Heatmap
df4=df.copy()
#encoding
from sklearn.preprocessing import LabelEncoder,OrdinalEncoder,OneHotEncoder
le=LabelEncoder()
ohe=OneHotEncoder
oe=OrdinalEncoder()
df4["Ship Mode"]=oe.fit_transform(df[["Ship Mode"]])
df4["Segment"]=oe.fit_transform(df[["Segment"]])
df4["City"]=le.fit_transform(df[["City"]])
df4["State"]=le.fit_transform(df[["State"]])
df4['Region'] = oe.fit_transform(df[['Region']])
df4["Category"]=oe.fit_transform(df[["Category"]])
df4["Sub-Category"]=le.fit_transform(df[["Sub-Category"]])
#scaling
from sklearn.preprocessing import RobustScaler
sc=RobustScaler()
df5=pd.DataFrame(sc.fit_transform(df4),columns=['Ship Mode', 'Segment', 'City','State','Region','Category','Sub-Category','Sales','Quantity','Discount','Profit'])
plt.subplots(figsize=(12,7))
sns.heatmap(df5.corr(),cmap="PuBu",annot=True)
plt.show()
#Find the relation between sales and profit based on the following category.
#Segment
grouped_data = df.groupby('Segment')[['Sales', 'Profit']].mean()
# Create a bar chart of the grouped data
fig, ax = plt.subplots()
ax.bar(grouped_data.index, grouped_data['Sales'], label='Sales')
ax.bar(grouped_data.index, grouped_data['Profit'], bottom=grouped_data['Sales'],
label='Profit')
ax.set_xlabel('Segment')
ax.set_ylabel('Value')
ax.legend()
plt.show()
#City
grouped_data = df.groupby('City')[['Sales', 'Profit']].mean()
# Create a bar chart of the grouped data
fig, ax = plt.subplots()
ax.bar(grouped_data.index, grouped_data['Sales'], label='Sales')
ax.bar(grouped_data.index, grouped_data['Profit'], bottom=grouped_data['Sales'],
label='Profit')
ax.set_xlabel('City')
ax.set_ylabel('Value')
ax.legend()
plt.show()
#States:
grouped_data = df.groupby('State')[['Sales', 'Profit']].mean()
# Create a bar chart of the grouped data
fig, ax = plt.subplots()
ax.bar(grouped_data.index, grouped_data['Sales'], label='Sales')
ax.bar(grouped_data.index, grouped_data['Profit'], bottom=grouped_data['Sales'],
label='Profit')
ax.set_xlabel('State')
ax.set_ylabel('Value')
ax.legend()
plt.show()
#Segment and Ship Mode
grouped_data = df.groupby(['Segment', 'Ship Mode'])[['Sales', 'Profit']].mean()
pivot_data = grouped_data.reset_index().pivot(index='Segment', columns='Ship Mode',
values=['Sales', 'Profit'])
# Create a bar chart of the grouped data
fig, ax = plt.subplots()
pivot_data.plot(kind='bar', ax=ax)
ax.set_xlabel('Segment')
ax.set_ylabel('Value')
plt.legend(title='Ship Mode')
plt.show()
#Segment, Ship mode and Region
grouped_data = df.groupby(['Segment', 'Ship Mode','Region'])[['Sales', 'Profit']].mean()
pivot_data = grouped_data.reset_index().pivot(index=['Segment', 'Ship Mode'],
columns='Region', values=['Sales', 'Profit'])
sns.set_style("whitegrid")
sns.set_palette("Set1")
pivot_data.plot(kind='bar', stacked=True, figsize=(10, 5))
plt.xlabel('Segment - Ship Mode')
plt.ylabel('Value')
plt.legend(title='Region')
plt.show()
```

# OUPUT
![Screenshot 2023-05-26 200430](https://github.com/swetha1510/Ex-08-Data-Visualization-/assets/120623583/128fdc78-2b93-41f5-b258-6e622fbfd3fa)
![Screenshot 2023-05-26 201043](https://github.com/swetha1510/Ex-08-Data-Visualization-/assets/120623583/9ef47387-d2a2-47c6-8a16-31cd3a63d507)
![Screenshot 2023-05-26 201154](https://github.com/swetha1510/Ex-08-Data-Visualization-/assets/120623583/9ccf3450-c112-46df-9ca6-1cf59a940ed8)
![Screenshot 2023-05-26 201237](https://github.com/swetha1510/Ex-08-Data-Visualization-/assets/120623583/c9170f1b-a869-4b9b-a8ba-4b5cc965a04d)
![Screenshot 2023-05-26 201349](https://github.com/swetha1510/Ex-08-Data-Visualization-/assets/120623583/c8ea02f1-73b5-4226-af7f-c19f5e1998de)
![Screenshot 2023-05-26 201440](https://github.com/swetha1510/Ex-08-Data-Visualization-/assets/120623583/fb2fdb14-3807-4443-a771-d2d427f074a5)
![Screenshot 2023-05-26 201514](https://github.com/swetha1510/Ex-08-Data-Visualization-/assets/120623583/2bb6b238-413c-4a29-92a9-95d1ec0bb382)
![Screenshot 2023-05-26 201708](https://github.com/swetha1510/Ex-08-Data-Visualization-/assets/120623583/ca06f825-b220-443f-b22b-b974c67c84c4)
![Screenshot 2023-05-26 201823](https://github.com/swetha1510/Ex-08-Data-Visualization-/assets/120623583/911a982f-1ec2-4a9d-8c01-3235f37b7684)
![Screenshot 2023-05-26 201901](https://github.com/swetha1510/Ex-08-Data-Visualization-/assets/120623583/f8500949-6bf9-4944-8fd4-5a01d485d68a)
![Screenshot 2023-05-26 201929](https://github.com/swetha1510/Ex-08-Data-Visualization-/assets/120623583/ca4d4c58-6830-4198-bac9-4543bb0817a3)
![Screenshot 2023-05-26 201955](https://github.com/swetha1510/Ex-08-Data-Visualization-/assets/120623583/76591683-57d9-4ed0-a880-3e6eb6d51bc1)
![Screenshot 2023-05-26 202042](https://github.com/swetha1510/Ex-08-Data-Visualization-/assets/120623583/3526b18f-3b05-4907-9101-e2319480ae64)
![Screenshot 2023-05-26 202139](https://github.com/swetha1510/Ex-08-Data-Visualization-/assets/120623583/5166c818-8120-45cd-beb7-56113636ea5d)
![Screenshot 2023-05-26 202202](https://github.com/swetha1510/Ex-08-Data-Visualization-/assets/120623583/1c4d44bd-4791-4afe-a103-5917ee99518c)
![Screenshot 2023-05-26 202259](https://github.com/swetha1510/Ex-08-Data-Visualization-/assets/120623583/5e4ab593-b45a-4425-8069-fc41ee61ee7e)
![Screenshot 2023-05-26 202336](https://github.com/swetha1510/Ex-08-Data-Visualization-/assets/120623583/c59cc356-7822-498e-b638-111be5fd5567)
![Screenshot 2023-05-26 202419](https://github.com/swetha1510/Ex-08-Data-Visualization-/assets/120623583/d1adedb9-d980-4a92-bf46-9cf4c5563602)
![Screenshot 2023-05-26 202451](https://github.com/swetha1510/Ex-08-Data-Visualization-/assets/120623583/1a431f08-1b2e-4c43-8402-a6a702b57631)
![Screenshot 2023-05-26 202511](https://github.com/swetha1510/Ex-08-Data-Visualization-/assets/120623583/c180053d-62c6-485e-9bc4-13ef85511ef1)
![Screenshot 2023-05-26 202531](https://github.com/swetha1510/Ex-08-Data-Visualization-/assets/120623583/acccb345-eb70-4953-9e45-4fb6339e70ba)
![Screenshot 2023-05-26 202550](https://github.com/swetha1510/Ex-08-Data-Visualization-/assets/120623583/f7d6d206-c079-4fa5-a307-443616f70d39)
![Screenshot 2023-05-26 202622](https://github.com/swetha1510/Ex-08-Data-Visualization-/assets/120623583/bc4f318d-7eb2-4245-9e86-87b7a7fd1c8f)

## RESULT:
Thus, Data Visualization is performed on the given dataset and save the data to a file.












