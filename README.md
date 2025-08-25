# Customer-Engagement-Offer-Effectiveness-Analysis
## Problem Statement
- The Cafe Rewards program runs marketing campaigns that send promotional offers to customers over a 30-day period.
- Offers can be of three types: informational, discount, or buy one, get one (BOGO).
- Customers receive different mixes of offers and have a limited time to redeem them.
- Customer engagement is recorded through events — offers received, viewed, completed — and purchases (transactions).
- The company wants to understand which offers and customer segments generate the highest engagement and sales to improve targeting in future campaigns.
## Project Objectives
- Determine which offer types (BOGO, discount, informational) are most effective in driving completions.
- how many informational offers were followed by transactions.
- Measure the impact of offers on purchase behavior.
- Identify patterns in offer completions based on demographic factors.
- Evaluate which communication channels (email, web, mobile, social) lead to higher completion rates.
- Measure average time taken from offer receipt to completion.
- Provide data-driven recommendations to improve future marketing campaigns.
```python
Importing File & Required Liabraries
import pandas as pd 
import numpy as np
import seaborn as sns 
import matplotlib.pyplot as plt
import warnings 
warnings.filterwarnings('ignore')
```
```python
Customers=pd.read_csv(r'C:\Users\Aarav Computer\Downloads\Caffe Rewards\customers.csv')
Offers=pd.read_csv(r'C:\Users\Aarav Computer\Downloads\Caffe Rewards\offers.csv')
```
## Exploratory Data Analysis
Checking Data Types,Missing Values
```python
Customers.head(10)
Customers.dtypes
Customers.isnull().sum()
```
Adding columns, Changing data types ,Renaming columns,Filling Missing/Null values
```python
def Age_Category(age):
        if age <= 20:
            return '0-20'
        elif age <= 40:
             return '21-40'
        elif age <= 60:
            return '41-60'
        else:
            return 'Greater than 60'
        
Customers['Age_Category']=Customers['age'].apply(Age_Category)
def Income_Category(income):
        if income <= 40000:
            return '0-40000'
        elif income <= 80000:
             return '41000-80000'
        elif income <= 100000:
            return '81000-100000'
        else:
            return 'higher than 100000'
````
```python
        
Customers['Income_Category']=Customers['income'].apply(Income_Category)
Customers.rename(columns={'became_member_on':'Membership'},inplace = True)
Customers['income'].isna().mean()*100
Customers['income'].interpolate(method='linear',inplace=True)
Customers['gender']=Customers['gender'].fillna('unknown')
Customers['Membership']=pd.to_datetime(Customers['Membership'],format='%Y%m%d')
```

## Adding Columns
import datetime as dt
```python
Customers['Year']=Customers['Membership'].dt.year
```
## Data cleaning
```
Events=pd.read_csv(r'C:\Users\Aarav Computer\Downloads\Caffe Rewards\events.csv')
Events.head(10)
Events['day']=(Events['time']//24)+1
Events.dtypes
Events.isnull().sum()
Events.rename(columns={'value':'offer_id'},inplace=True)
Events['offer_id']=Events['offer_id'].str.replace(r"[{'offer_id':']","",regex=True)
Events['offer_id']=Events['offer_id'].str.replace(r"[{'offer_id':']","",regex=True)
Events['offer_id']=Events['offer_id'].str.replace(r"[amunt]","",regex=True)
Events['offer_id']=Events['offer_id'].str.replace(r"[}]","",regex=True)
Events['offer_id']=Events['offer_id'].replace({"9b98b8c733c4b65b9b679969":"9b98b8c7a33c4b65b9aebfe6a799e6d9",
                                                   "0b115392cc45b7b97c272217":"0b1e1539f2cc45b7b9fa7c272da2e1d7",
                                                   "2906b810c74411798c6938c95":"2906b810c7d4411798c6938adc9daaa5",
                                                   "c6683743c1bb461111cc24":"fafdcd668e3743c1bb461111dcafc2a4",
                                                   "3207678b1433c631608b":"3f207df678b143eea3cee63160fa8bed",
                                                   "45c5796940891539b80":"4d5c57ea9a6940dd891ad53e9dbe8da0",
                                                   "22986c3696443797061b8c2":"2298d6c36e964ae4a3e7e9706d1fb8c2",
                                                   "19421c1440978bb69c19b020":"f19421c1d4aa40978ebb69ca19b0e20d",
                                                   "26436372046b9bb56bc8210":"ae264e3637204a6fb9bb56bc8210ddfd",
                                         "58bc65990b2455138643c4b9837":"5a8bc65990b245e5a138643cd4eb9837"},regex=True)
```
```python
Events.head(10)
Offerscompleted=Events[Events['event']=='offer completed']
Correcting Values
Offerscompleted
Offers['offer_id'].unique()
Offerscompleted['offer_id'].unique()
Offerscompleted['offer_id']=Offerscompleted['offer_id'].str.split(',').str[0]
Offerscompleted['offer_id']=Offerscompleted['offer_id'].str.strip()
Offerscompleted
```
## Customer Insights :
## 1.Gender_Wise Overall Customers VS Offer Completed Customers Distribution :
```python
plt.figure(figsize=(12,4))
plt.subplot(1,2,1)
col=['red','orange','pink']
Gender=['Female','Male','Other']
plt.pie(GenderWiseCustomers['customer_id'],autopct='%1.1f%%',colors=col,labels=GenderWiseCustomers['gender'],shadow=True)
plt.title('Total Customers Gender_wise Distribution',fontweight='bold',fontsize=10)
plt.subplot(1,2,2)
col=['pink','orange','coral']
Gender=['Female','Male','Other']
plt.pie(OfferscompletedCustomers['customer_id'],autopct='%1.1f%%',colors=col,labels=OfferscompletedCustomers['gender'],shadow=True)
plt.title('Offers Completed Customers Gender_wise Distribution',fontweight='bold',fontsize=10)
plt.show()
```
![Customers](Customers%20Distribution.png)
- The business has received the male customers more.
- both in terms of event and offers completed that means where the other events refers to offer receive,offer viewed,and transactions happened the male customers count is more than the female count.
- and even in terms of completing the offers also,male has contributed the 49% where as female customes have contributed to the 46.1%.
- Other category is contributing generally low.
- Unknown category showing the data of customes where 1.3% customers are unknown because gender information is not provided of this customers and it can lead to generate wrong customer insights.
- The business employees are responsible to take the proper information of the customers for the future marketing and promotional planning in the different customer segments.
## 2.Age Wise Customers Distribution:
```
python
plt.figure(figsize=(10,4))
sns.barplot(data=Data,x='Age_Category',y='customercount',palette='YlOrBr')
plt.title('Age Category Wise Customer Distribution')
plt.show()
```
- Customers below 20 years (kids) form the smallest group, with fewer than 15,000 customers.

- The largest customer group belongs to the 60+ age category (old age customers).

- A significant variation is observed in customer counts across the range of 20,000 to 120,000, showing notable differences between age groups.

- The 41–60 age group contributes a substantial customer base of more than 100,000 customers.

- The 21–40 age group (adults) accounts for only around 60,000 customers, which is half the size of the old age category.

- This wide variation in age-based customer distribution highlights the need for stronger marketing strategies to improve awareness and engagement with products and offers across all age categories.
```python
Income=Customer_Details['income'].reset_index()
IncomeDistribute=Customers.groupby('Income_Category')['customer_id'].count().reset_index()
plt.pie(IncomeDistribute['customer_id'],labels=IncomeDistribute['Income_Category'],autopct='%1.1f%%')
plt.title('Customers Disribution by the Income')
plt.show()
```
```python
AgeandIncome=Customer_Details[['age','income']]
plt.figure(figsize=(8,4))
sns.scatterplot(data=AgeandIncome,x='age',y='income')
plt.title('Age & Incomes Correlation',fontsize=14)
plt.show()
```
```python
AgeandIncome=Customer_Details[['age','income']].corr()
AgeandIncome
print('Correlation coefficient\n------------')
print('AgeIncomeCorr:',round(AgeandIncome.values[0,1],2))
```
- The 0.2 is showing a positive correlation but the correlation is is very week.
- That means as age increases ,income tends to increase slightly,but the relationship is not strong.
- That means there is no as such connection is there between the earning of the customers with the age,so if the business focusing on high paying customers the business should check the income of the person individually age priority will not be the good option in that case.
## Events Insights:
## How was the events distribution in the business?
```python
col=['skyblue','pink','orange','gold']
plt.figure(figsize=(4,8))
plt.pie(EventsCount['count'],autopct='%1.1f%%',colors=col,labels=EventsCount['event'],shadow=True)
plt.title('Events Distribution Percentage',fontsize =14)
plt.show()
```
## Conclusion :
- Male customers are more active than female customers in events and offer completions.

- A small percentage (1.3%) of customers have unknown gender, which may distort customer insights.

- The old age (60+) group is the largest customer base, while under 20 group is the smallest.

- A major portion of customers (40–50%) fall in the income range of $40,000–80,000.

- The correlation between age and income is weak (0.2), meaning income cannot be predicted reliably by age.

- Most offers were targeted at old age customers, leading to higher completion in that segment.

- Regular transactions (138,000) are much higher than offers completed (11%), showing many customers buy without offers.

- Daily transactions range between 3,000–6,000, with fluctuations and unstable sales patterns.

- Customer membership peaked until 2017 but dropped sharply in 2018, showing reduced loyalty.

- Discount offers were the most popular, with two specific offers completing 5,000+ times each, while offers with high difficulty + short duration performed poorly.

## Recommendations:
- Collect complete and accurate gender data to avoid bias in analysis.

- Improve targeting by ensuring equal marketing across all age groups, not just older customers.

- Focus marketing campaigns on the 21–40 age group, which is underrepresented compared to older customers.

- Provide loyalty programs and consistent discounts to stabilize sales and increase membership.

- Design offers with a balanced difficulty and duration (not too hard, not too short) for better completion.

- Track low-performing days and apply dynamic offers or promotions to maintain continuity in sales.
  # Download the project file
  [project](Coffee%20Project%20file.pdf)

  
# SHAHISTA SHAIKH
# Contact me:
shaikhshahi326@gmail.com



