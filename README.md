# **1. TechStream-Solutions**
### **Context**
The company is called **"TechStream Solutions"**, TechStream Solutions has been operating for several years and has gathered significant data on their costs and revenues.
Their product is a **Software as a Service (SaaS) platform** named **"Streamline Pro"**.

This platform provides comprehensive project management and collaboration tools for businesses of all sizes to manage projects, track progress, and collaborate efficiently.

They are now looking to analyze their unit economics to understand the profitability of Streamline Pro on a per-customer basis.

### **Objective**
My task is to calculate the unit economics for Streamline Pro for the month of March 2023. This will help us assess the profitability and efficiency of our customer acquisition strategies and operational expenses.

This involves analysing key metrics such as Customer Acquisition Cost (CAC), Average Revenue Per User (ARPU), Cost of Goods Sold (COGS), Gross Margin, Customer Lifetime Value (LTV), and the LTV/CAC ratio.

# **2. Data**
### **Data source**

We have a shared folder containing company data:

https://drive.google.com/drive/folders/1qhOW9Y2orRXuzbX-kXEmuJ7TMQiRs2Uv?usp=drive_link

### **Data import**

Import library

```
# import needed library

import pandas as pd
import numpy as np
```
Loading data

```
id_1='1by8tPHwOnq3uKYK2E7sA9VBUYoPM4p1Rnrm_Ss9cyHI'
id_2='1AZOIThOV4P-0eYDge53ZwumVkfkHoYPWxst3k3Bv87c'
id_3='10OGbaywwMIqKgnPGy8VDvpBVtjyqln47iYa2lFhI9Mw'
id_4='1c_WihqTZCQvNgxzmd-OwhR9i5diwtfxXVLyMn8R-Lp4'
id_5='1qayqML1zCKdmtzutkcy9LWvE6xFRm6TGBEVkHHJKIuE'

url_1='https://docs.google.com/spreadsheets/d/' + id_1 + '/export?format=xlsx'
url_2='https://docs.google.com/spreadsheets/d/' + id_2 + '/export?format=xlsx'
url_3='https://docs.google.com/spreadsheets/d/' + id_3 + '/export?format=xlsx'
url_4='https://docs.google.com/spreadsheets/d/' + id_4 + '/export?format=xlsx'
url_5='https://docs.google.com/spreadsheets/d/' + id_5 + '/export?format=xlsx'

df_customer_life=pd.read_excel(url_1, sheet_name='Sheet1')
df_marketing_expenses=pd.read_excel(url_2, sheet_name='Sheet1')
df_monthly_expenses=pd.read_excel(url_3, sheet_name='Sheet1')
df_payroll=pd.read_excel(url_4, sheet_name='Sheet1')
df_payment=pd.read_excel(url_5, sheet_name='Sheet1')
```

# **3. Data Exploration and Calculation**

## **Monthly Expense**

Explore dataset

[In]

```
df_monthly_expenses.head()
df_monthly_expenses.info()
```


[out]

|    |   # | month               | category          | item                 |   amount |
|---:|----:|:--------------------|:------------------|:---------------------|---------:|
|  0 |   1 | 2023-01-01 00:00:00 | Server Costs      | AWS Hosting          |     8000 |
|  1 |   2 | 2023-01-01 00:00:00 | Server Costs      | Google Cloud Storage |     4000 |
|  2 |   3 | 2023-01-01 00:00:00 | Software Licenses | Atlassian Jira       |     1200 |
|  3 |   4 | 2023-01-01 00:00:00 | Software Licenses | Slack                |      800 |
|  4 |   5 | 2023-01-01 00:00:00 | Software Licenses | Salesforce           |     1500 |



![image](https://github.com/user-attachments/assets/1cc6d2d5-3dd3-4f68-8d54-2af4cde8e225)

Calculate sales & marketing expenses

[In]

```
# Create new database for last month expenses:

cond = df_monthly_expenses['month'].dt.strftime('%Y-%m') == '2023-03'

df_last_month_expenses = df_monthly_expenses[cond]
df_last_month_expenses
```
[Out]

|    |   # | month               | category          | item                 |   amount |
|---:|----:|:--------------------|:------------------|:---------------------|---------:|
| 18 |  19 | 2023-03-01 00:00:00 | Server Costs      | AWS Hosting          |     8400 |
| 19 |  20 | 2023-03-01 00:00:00 | Server Costs      | Google Cloud Storage |     4400 |
| 20 |  21 | 2023-03-01 00:00:00 | Software Licenses | Atlassian Jira       |     1400 |
| 21 |  22 | 2023-03-01 00:00:00 | Software Licenses | Slack                |      900 |
| 22 |  23 | 2023-03-01 00:00:00 | Software Licenses | Salesforce           |     1700 |
| 23 |  24 | 2023-03-01 00:00:00 | Software Licenses | Zoom                 |      540 |
| 24 |  25 | 2023-03-01 00:00:00 | Office Rental     | Office Rent          |    10000 |
| 25 |  26 | 2023-03-01 00:00:00 | Other             | Office Supplies      |      600 |
| 26 |  27 | 2023-03-01 00:00:00 | Other             | Travel Expenses      |     3200 |


[In]

```
# Calculate sales & marketing expenses

cond= df_last_month_expenses['item']=='Salesforce'
crm_cost = df_last_month_expenses[cond]['amount'].values[0]
crm_cost

# Add marketing & sales cost to total expenses:

Total_expenses = crm_cost
Total_expenses
```

[Out]

```
1700
```

## **Last month Salary**

Explore dataset

[In]

```
df_payroll.head()
```

[Out]

|    | month               | department   | employee_name   | position          |   paid |
|---:|:--------------------|:-------------|:----------------|:------------------|-------:|
|  0 | 2023-01-01 00:00:00 | Sales        | John Doe        | Sales Manager     |   1500 |
|  1 | 2023-01-01 00:00:00 | Sales        | Jane Smith      | Sales Associate   |    600 |
|  2 | 2023-01-01 00:00:00 | Sales        | Jim Brown       | Sales Associate   |    700 |
|  3 | 2023-01-01 00:00:00 | Sales        | Laura Miller    | Sales Associate   |    800 |
|  4 | 2023-01-01 00:00:00 | Marketing    | Alice Johnson   | Marketing Manager |   1650 |

Add new column [yyyy-mm] to dataset for filter purpose

[In]

```
df_payroll['yyyy-mm']=df_payroll['month'].dt.strftime('%Y-%m')
df_payroll.to_markdown()
```

[Out]

|    | month               | department   | employee_name   | position           |   paid | yyyy-mm   |
|---:|:--------------------|:-------------|:----------------|:-------------------|-------:|:----------|
|  0 | 2023-01-01 00:00:00 | Sales        | John Doe        | Sales Manager      |   1500 | 2023-01   |
|  1 | 2023-01-01 00:00:00 | Sales        | Jane Smith      | Sales Associate    |    600 | 2023-01   |
|  2 | 2023-01-01 00:00:00 | Sales        | Jim Brown       | Sales Associate    |    700 | 2023-01   |
|  3 | 2023-01-01 00:00:00 | Sales        | Laura Miller    | Sales Associate    |    800 | 2023-01   |
|  4 | 2023-01-01 00:00:00 | Marketing    | Alice Johnson   | Marketing Manager  |   1650 | 2023-01   |
|  5 | 2023-01-01 00:00:00 | Marketing    | Bob Davis       | Content Specialist |    700 | 2023-01   |
|  6 | 2023-01-01 00:00:00 | Engineering  | Charlie Wilson  | Senior Developer   |   2500 | 2023-01   |
|  7 | 2023-01-01 00:00:00 | Engineering  | Diana Lee       | Developer          |   1700 | 2023-01   |
|  8 | 2023-01-01 00:00:00 | Engineering  | Eva White       | Junior Developer   |   1000 | 2023-01   |
|  9 | 2023-01-01 00:00:00 | HR           | Frank Green     | HR Manager         |   1300 | 2023-01   |
| 10 | 2023-01-01 00:00:00 | HR           | Gary Black      | HR Specialist      |    700 | 2023-01   |
| 11 | 2023-01-01 00:00:00 | Support      | Hannah Scott    | Support Lead       |    600 | 2023-01   |
| 12 | 2023-01-01 00:00:00 | Support      | Ian Harris      | Support Specialist |    500 | 2023-01   |
| 13 | 2023-01-01 00:00:00 | Support      | Emily Clark     | Support Specialist |    450 | 2023-01   |
| 14 | 2023-01-01 00:00:00 | Analytics    | Michael Brown   | Analytics Manager  |   1700 | 2023-01   |
| 15 | 2023-01-01 00:00:00 | Analytics    | Sarah Wilson    | Data Analyst       |   1200 | 2023-01   |
| 16 | 2023-01-01 00:00:00 | Analytics    | David Lee       | Data Analyst       |   1300 | 2023-01   |
| 17 | 2023-02-01 00:00:00 | Sales        | John Doe        | Sales Manager      |   1500 | 2023-02   |
| 18 | 2023-02-01 00:00:00 | Sales        | Jane Smith      | Sales Associate    |    600 | 2023-02   |
| 19 | 2023-02-01 00:00:00 | Sales        | Jim Brown       | Sales Associate    |    700 | 2023-02   |
| 20 | 2023-02-01 00:00:00 | Sales        | Laura Miller    | Sales Associate    |   5100 | 2023-02   |
| 21 | 2023-02-01 00:00:00 | Marketing    | Alice Johnson   | Marketing Manager  |   1650 | 2023-02   |
| 22 | 2023-02-01 00:00:00 | Marketing    | Bob Davis       | Content Specialist |    700 | 2023-02   |
| 23 | 2023-02-01 00:00:00 | Engineering  | Charlie Wilson  | Senior Developer   |   2500 | 2023-02   |
| 24 | 2023-02-01 00:00:00 | Engineering  | Diana Lee       | Developer          |   1700 | 2023-02   |
| 25 | 2023-02-01 00:00:00 | Engineering  | Eva White       | Junior Developer   |   1000 | 2023-02   |
| 26 | 2023-02-01 00:00:00 | HR           | Frank Green     | HR Manager         |   1300 | 2023-02   |
| 27 | 2023-02-01 00:00:00 | HR           | Gary Black      | HR Specialist      |    700 | 2023-02   |
| 28 | 2023-02-01 00:00:00 | Support      | Hannah Scott    | Support Lead       |    600 | 2023-02   |
| 29 | 2023-02-01 00:00:00 | Support      | Ian Harris      | Support Specialist |    500 | 2023-02   |
| 30 | 2023-02-01 00:00:00 | Support      | Emily Clark     | Support Specialist |    450 | 2023-02   |
| 31 | 2023-02-01 00:00:00 | Analytics    | Michael Brown   | Analytics Manager  |   1700 | 2023-02   |
| 32 | 2023-02-01 00:00:00 | Analytics    | Sarah Wilson    | Data Analyst       |   1200 | 2023-02   |
| 33 | 2023-02-01 00:00:00 | Analytics    | David Lee       | Data Analyst       |   1300 | 2023-02   |
| 34 | 2023-03-01 00:00:00 | Sales        | John Doe        | Sales Manager      |   1500 | 2023-03   |
| 35 | 2023-03-01 00:00:00 | Sales        | Jane Smith      | Sales Associate    |    600 | 2023-03   |
| 36 | 2023-03-01 00:00:00 | Sales        | Jim Brown       | Sales Associate    |    700 | 2023-03   |
| 37 | 2023-03-01 00:00:00 | Sales        | Laura Miller    | Sales Associate    |    800 | 2023-03   |
| 38 | 2023-03-01 00:00:00 | Marketing    | Alice Johnson   | Marketing Manager  |   1650 | 2023-03   |
| 39 | 2023-03-01 00:00:00 | Marketing    | Bob Davis       | Content Specialist |    700 | 2023-03   |
| 40 | 2023-03-01 00:00:00 | Engineering  | Charlie Wilson  | Senior Developer   |   2500 | 2023-03   |
| 41 | 2023-03-01 00:00:00 | Engineering  | Diana Lee       | Developer          |   1700 | 2023-03   |
| 42 | 2023-03-01 00:00:00 | Engineering  | Eva White       | Junior Developer   |   1000 | 2023-03   |
| 43 | 2023-03-01 00:00:00 | HR           | Frank Green     | HR Manager         |   1300 | 2023-03   |
| 44 | 2023-03-01 00:00:00 | HR           | Gary Black      | HR Specialist      |    700 | 2023-03   |
| 45 | 2023-03-01 00:00:00 | Support      | Hannah Scott    | Support Lead       |    600 | 2023-03   |
| 46 | 2023-03-01 00:00:00 | Support      | Ian Harris      | Support Specialist |    500 | 2023-03   |
| 47 | 2023-03-01 00:00:00 | Support      | Emily Clark     | Support Specialist |    450 | 2023-03   |
| 48 | 2023-03-01 00:00:00 | Analytics    | Michael Brown   | Analytics Manager  |   1700 | 2023-03   |
| 49 | 2023-03-01 00:00:00 | Analytics    | Sarah Wilson    | Data Analyst       |   1200 | 2023-03   |
| 50 | 2023-03-01 00:00:00 | Analytics    | David Lee       | Data Analyst       |   1300 | 2023-03   |


Create  function to get last month data

[In]

```
# create function to get last_month data

def get_last_month(df_payroll,month):
  last_month=df_payroll['month'].max().strftime('%Y-%m')

  cond = df_payroll['month'].dt.strftime('%Y-%m') == last_month

  df_last_month_payroll = df_payroll[cond]
  return df_last_month_payroll

df_last_month_payroll = get_last_month(df_payroll,'month')
df_last_month_payroll
```

[Out]

|    | month               | department   | employee_name   | position           |   paid | yyyy-mm   |
|---:|:--------------------|:-------------|:----------------|:-------------------|-------:|:----------|
| 34 | 2023-03-01 00:00:00 | Sales        | John Doe        | Sales Manager      |   1500 | 2023-03   |
| 35 | 2023-03-01 00:00:00 | Sales        | Jane Smith      | Sales Associate    |    600 | 2023-03   |
| 36 | 2023-03-01 00:00:00 | Sales        | Jim Brown       | Sales Associate    |    700 | 2023-03   |
| 37 | 2023-03-01 00:00:00 | Sales        | Laura Miller    | Sales Associate    |    800 | 2023-03   |
| 38 | 2023-03-01 00:00:00 | Marketing    | Alice Johnson   | Marketing Manager  |   1650 | 2023-03   |
| 39 | 2023-03-01 00:00:00 | Marketing    | Bob Davis       | Content Specialist |    700 | 2023-03   |
| 40 | 2023-03-01 00:00:00 | Engineering  | Charlie Wilson  | Senior Developer   |   2500 | 2023-03   |
| 41 | 2023-03-01 00:00:00 | Engineering  | Diana Lee       | Developer          |   1700 | 2023-03   |
| 42 | 2023-03-01 00:00:00 | Engineering  | Eva White       | Junior Developer   |   1000 | 2023-03   |
| 43 | 2023-03-01 00:00:00 | HR           | Frank Green     | HR Manager         |   1300 | 2023-03   |
| 44 | 2023-03-01 00:00:00 | HR           | Gary Black      | HR Specialist      |    700 | 2023-03   |
| 45 | 2023-03-01 00:00:00 | Support      | Hannah Scott    | Support Lead       |    600 | 2023-03   |
| 46 | 2023-03-01 00:00:00 | Support      | Ian Harris      | Support Specialist |    500 | 2023-03   |
| 47 | 2023-03-01 00:00:00 | Support      | Emily Clark     | Support Specialist |    450 | 2023-03   |
| 48 | 2023-03-01 00:00:00 | Analytics    | Michael Brown   | Analytics Manager  |   1700 | 2023-03   |
| 49 | 2023-03-01 00:00:00 | Analytics    | Sarah Wilson    | Data Analyst       |   1200 | 2023-03   |
| 50 | 2023-03-01 00:00:00 | Analytics    | David Lee       | Data Analyst       |   1300 | 2023-03   |

Calculate salary of sales & marketing

[In]


```
# Filter data for sales & marketing of last month salary

last_month_Sales_MKT_payroll=df_last_month_payroll[(df_last_month_payroll['department']=='Sales') | (df_last_month_payroll['department']=='Marketing')]
last_month_Sales_MKT_payroll
```

[Out]

|    | month               | department   | employee_name   | position           |   paid | yyyy-mm   |
|---:|:--------------------|:-------------|:----------------|:-------------------|-------:|:----------|
| 34 | 2023-03-01 00:00:00 | Sales        | John Doe        | Sales Manager      |   1500 | 2023-03   |
| 35 | 2023-03-01 00:00:00 | Sales        | Jane Smith      | Sales Associate    |    600 | 2023-03   |
| 36 | 2023-03-01 00:00:00 | Sales        | Jim Brown       | Sales Associate    |    700 | 2023-03   |
| 37 | 2023-03-01 00:00:00 | Sales        | Laura Miller    | Sales Associate    |    800 | 2023-03   |
| 38 | 2023-03-01 00:00:00 | Marketing    | Alice Johnson   | Marketing Manager  |   1650 | 2023-03   |
| 39 | 2023-03-01 00:00:00 | Marketing    | Bob Davis       | Content Specialist |    700 | 2023-03   |

[In]

```
last_month_salary=last_month_Sales_MKT_payroll['paid'].sum()
last_month_salary
```

[Out]

'''
5950
'''

Add salary of sales & marketing to total expenses

[In]

```
Total_expenses+=last_month_salary
Total_expenses
```

[Out]

```
7650
```

### **Marketing expenses**

Explore dataset

[In]

```
df_marketing_expenses.head()
```

[Out]

|    | date                | channel      |   spending |
|---:|:--------------------|:-------------|-----------:|
|  0 | 2023-01-01 00:00:00 | Google Ads   |        784 |
|  1 | 2023-01-01 00:00:00 | Facebook Ads |        659 |
|  2 | 2023-01-01 00:00:00 | LinkedIn Ads |        729 |
|  3 | 2023-01-01 00:00:00 | Twitter Ads  |        292 |
|  4 | 2023-01-02 00:00:00 | Google Ads   |        935 |

Create function to get last month Marketing expenses

[In]

```
def get_last_month(df, date_column):
    last_month = df[date_column].max().strftime('%Y-%m')
    cond = df[date_column].dt.strftime('%Y-%m') == last_month
    df_last_month = df[cond]
    return df_last_month

last_month_mkt_expense=get_last_month(df_marketing_expenses,'date')
last_month_mkt_expense
````

[Out]

|     | date                | channel      |   spending |
|----:|:--------------------|:-------------|-----------:|
| 236 | 2023-03-01 00:00:00 | Google Ads   |        449 |
| 237 | 2023-03-01 00:00:00 | Facebook Ads |        229 |
| 238 | 2023-03-01 00:00:00 | LinkedIn Ads |        835 |
| 239 | 2023-03-01 00:00:00 | Twitter Ads  |        986 |
| 240 | 2023-03-02 00:00:00 | Google Ads   |        912 |
| 241 | 2023-03-02 00:00:00 | Facebook Ads |        316 |
| 242 | 2023-03-02 00:00:00 | LinkedIn Ads |        481 |
| 243 | 2023-03-02 00:00:00 | Twitter Ads  |        124 |
| 244 | 2023-03-03 00:00:00 | Google Ads   |        167 |
| 245 | 2023-03-03 00:00:00 | Facebook Ads |        871 |
| 246 | 2023-03-03 00:00:00 | LinkedIn Ads |        334 |
| 247 | 2023-03-03 00:00:00 | Twitter Ads  |        816 |
| 248 | 2023-03-04 00:00:00 | Google Ads   |        391 |
| 249 | 2023-03-04 00:00:00 | Facebook Ads |        826 |
| 250 | 2023-03-04 00:00:00 | LinkedIn Ads |        801 |
| 251 | 2023-03-04 00:00:00 | Twitter Ads  |        809 |
| 252 | 2023-03-05 00:00:00 | Google Ads   |        827 |
| 253 | 2023-03-05 00:00:00 | Facebook Ads |        655 |
| 254 | 2023-03-05 00:00:00 | LinkedIn Ads |        132 |
| 255 | 2023-03-05 00:00:00 | Twitter Ads  |        111 |
| 256 | 2023-03-06 00:00:00 | Google Ads   |        716 |
| 257 | 2023-03-06 00:00:00 | Facebook Ads |        312 |
| 258 | 2023-03-06 00:00:00 | LinkedIn Ads |        238 |
| 259 | 2023-03-06 00:00:00 | Twitter Ads  |        794 |
| 260 | 2023-03-07 00:00:00 | Google Ads   |        521 |
| 261 | 2023-03-07 00:00:00 | Facebook Ads |        737 |
| 262 | 2023-03-07 00:00:00 | LinkedIn Ads |        768 |
| 263 | 2023-03-07 00:00:00 | Twitter Ads  |        723 |
| 264 | 2023-03-08 00:00:00 | Google Ads   |        588 |
| 265 | 2023-03-08 00:00:00 | Facebook Ads |        870 |
| 266 | 2023-03-08 00:00:00 | LinkedIn Ads |        639 |
| 267 | 2023-03-08 00:00:00 | Twitter Ads  |        317 |
| 268 | 2023-03-09 00:00:00 | Google Ads   |        763 |
| 269 | 2023-03-09 00:00:00 | Facebook Ads |        921 |
| 270 | 2023-03-09 00:00:00 | LinkedIn Ads |        407 |
| 271 | 2023-03-09 00:00:00 | Twitter Ads  |        274 |
| 272 | 2023-03-10 00:00:00 | Google Ads   |        248 |
| 273 | 2023-03-10 00:00:00 | Facebook Ads |        641 |
| 274 | 2023-03-10 00:00:00 | LinkedIn Ads |        679 |
| 275 | 2023-03-10 00:00:00 | Twitter Ads  |        647 |
| 276 | 2023-03-11 00:00:00 | Google Ads   |        907 |
| 277 | 2023-03-11 00:00:00 | Facebook Ads |        493 |
| 278 | 2023-03-11 00:00:00 | LinkedIn Ads |        173 |
| 279 | 2023-03-11 00:00:00 | Twitter Ads  |        397 |
| 280 | 2023-03-12 00:00:00 | Google Ads   |        999 |
| 281 | 2023-03-12 00:00:00 | Facebook Ads |        914 |
| 282 | 2023-03-12 00:00:00 | LinkedIn Ads |        830 |
| 283 | 2023-03-12 00:00:00 | Twitter Ads  |        976 |
| 284 | 2023-03-13 00:00:00 | Google Ads   |        871 |
| 285 | 2023-03-13 00:00:00 | Facebook Ads |        899 |
| 286 | 2023-03-13 00:00:00 | LinkedIn Ads |        877 |
| 287 | 2023-03-13 00:00:00 | Twitter Ads  |        494 |
| 288 | 2023-03-14 00:00:00 | Google Ads   |        639 |
| 289 | 2023-03-14 00:00:00 | Facebook Ads |        529 |
| 290 | 2023-03-14 00:00:00 | LinkedIn Ads |        299 |
| 291 | 2023-03-14 00:00:00 | Twitter Ads  |        523 |
| 292 | 2023-03-15 00:00:00 | Google Ads   |        161 |
| 293 | 2023-03-15 00:00:00 | Facebook Ads |        441 |
| 294 | 2023-03-15 00:00:00 | LinkedIn Ads |        965 |
| 295 | 2023-03-15 00:00:00 | Twitter Ads  |        144 |
| 296 | 2023-03-16 00:00:00 | Google Ads   |        902 |
| 297 | 2023-03-16 00:00:00 | Facebook Ads |        188 |
| 298 | 2023-03-16 00:00:00 | LinkedIn Ads |        133 |
| 299 | 2023-03-16 00:00:00 | Twitter Ads  |        745 |
| 300 | 2023-03-17 00:00:00 | Google Ads   |        332 |
| 301 | 2023-03-17 00:00:00 | Facebook Ads |        867 |
| 302 | 2023-03-17 00:00:00 | LinkedIn Ads |        136 |
| 303 | 2023-03-17 00:00:00 | Twitter Ads  |        868 |
| 304 | 2023-03-18 00:00:00 | Google Ads   |        559 |
| 305 | 2023-03-18 00:00:00 | Facebook Ads |        390 |
| 306 | 2023-03-18 00:00:00 | LinkedIn Ads |        297 |
| 307 | 2023-03-18 00:00:00 | Twitter Ads  |        994 |
| 308 | 2023-03-19 00:00:00 | Google Ads   |        354 |
| 309 | 2023-03-19 00:00:00 | Facebook Ads |        180 |
| 310 | 2023-03-19 00:00:00 | LinkedIn Ads |        546 |
| 311 | 2023-03-19 00:00:00 | Twitter Ads  |        236 |
| 312 | 2023-03-20 00:00:00 | Google Ads   |        289 |
| 313 | 2023-03-20 00:00:00 | Facebook Ads |        229 |
| 314 | 2023-03-20 00:00:00 | LinkedIn Ads |        309 |
| 315 | 2023-03-20 00:00:00 | Twitter Ads  |        468 |
| 316 | 2023-03-21 00:00:00 | Google Ads   |        391 |
| 317 | 2023-03-21 00:00:00 | Facebook Ads |        476 |
| 318 | 2023-03-21 00:00:00 | LinkedIn Ads |        447 |
| 319 | 2023-03-21 00:00:00 | Twitter Ads  |        268 |
| 320 | 2023-03-22 00:00:00 | Google Ads   |        984 |
| 321 | 2023-03-22 00:00:00 | Facebook Ads |        904 |
| 322 | 2023-03-22 00:00:00 | LinkedIn Ads |        276 |
| 323 | 2023-03-22 00:00:00 | Twitter Ads  |        637 |
| 324 | 2023-03-23 00:00:00 | Google Ads   |        423 |
| 325 | 2023-03-23 00:00:00 | Facebook Ads |        971 |
| 326 | 2023-03-23 00:00:00 | LinkedIn Ads |        608 |
| 327 | 2023-03-23 00:00:00 | Twitter Ads  |        903 |
| 328 | 2023-03-24 00:00:00 | Google Ads   |        214 |
| 329 | 2023-03-24 00:00:00 | Facebook Ads |        898 |
| 330 | 2023-03-24 00:00:00 | LinkedIn Ads |        129 |
| 331 | 2023-03-24 00:00:00 | Twitter Ads  |        853 |
| 332 | 2023-03-25 00:00:00 | Google Ads   |        389 |
| 333 | 2023-03-25 00:00:00 | Facebook Ads |        246 |
| 334 | 2023-03-25 00:00:00 | LinkedIn Ads |        373 |
| 335 | 2023-03-25 00:00:00 | Twitter Ads  |        321 |
| 336 | 2023-03-26 00:00:00 | Google Ads   |        440 |
| 337 | 2023-03-26 00:00:00 | Facebook Ads |        609 |
| 338 | 2023-03-26 00:00:00 | LinkedIn Ads |        614 |
| 339 | 2023-03-26 00:00:00 | Twitter Ads  |        169 |
| 340 | 2023-03-27 00:00:00 | Google Ads   |        457 |
| 341 | 2023-03-27 00:00:00 | Facebook Ads |        656 |
| 342 | 2023-03-27 00:00:00 | LinkedIn Ads |        985 |
| 343 | 2023-03-27 00:00:00 | Twitter Ads  |        865 |
| 344 | 2023-03-28 00:00:00 | Google Ads   |        422 |
| 345 | 2023-03-28 00:00:00 | Facebook Ads |        723 |
| 346 | 2023-03-28 00:00:00 | LinkedIn Ads |        191 |
| 347 | 2023-03-28 00:00:00 | Twitter Ads  |        441 |
| 348 | 2023-03-29 00:00:00 | Google Ads   |        523 |
| 349 | 2023-03-29 00:00:00 | Facebook Ads |        651 |
| 350 | 2023-03-29 00:00:00 | LinkedIn Ads |        762 |
| 351 | 2023-03-29 00:00:00 | Twitter Ads  |        514 |
| 352 | 2023-03-30 00:00:00 | Google Ads   |        757 |
| 353 | 2023-03-30 00:00:00 | Facebook Ads |        810 |
| 354 | 2023-03-30 00:00:00 | LinkedIn Ads |        374 |
| 355 | 2023-03-30 00:00:00 | Twitter Ads  |        960 |
| 356 | 2023-03-31 00:00:00 | Google Ads   |        143 |
| 357 | 2023-03-31 00:00:00 | Facebook Ads |        183 |
| 358 | 2023-03-31 00:00:00 | LinkedIn Ads |        533 |
| 359 | 2023-03-31 00:00:00 | Twitter Ads  |        909 |


Calculate for last month Marketing expenses

[In]

```
total_mkt_expense = last_month_mkt_expense['spending'].sum()
total_mkt_expense
```
[Out]

```
68830
```

Add last month marketing expense to total expense

[In]

```
Total_expenses+=total_mkt_expense
Total_expenses
```

[Out]

```
76480
```
### **Payment**

Explore dataset

[In]

```
df_payment.head()
```

[Out]

|    | date                |   customer_id |   receipt_amount |   new_customer |
|---:|:--------------------|--------------:|-----------------:|---------------:|
|  0 | 2023-01-01 00:00:00 |          2653 |               67 |              1 |
|  1 | 2023-01-01 00:00:00 |          2731 |              271 |              1 |
|  2 | 2023-01-01 00:00:00 |          1277 |              231 |              0 |
|  3 | 2023-01-01 00:00:00 |          2094 |              107 |              0 |
|  4 | 2023-01-01 00:00:00 |          1314 |              416 |              0 |


Get last month payment

[In]

```
last_month_payment = get_last_month(df_payment,'date')
last_month_payment
```

[Out]

|     | date                |   customer_id |   receipt_amount |   new_customer |
|----:|:--------------------|--------------:|-----------------:|---------------:|
| 618 | 2023-03-01 00:00:00 |          1062 |              103 |              0 |
| 619 | 2023-03-01 00:00:00 |          2243 |              157 |              0 |
| 620 | 2023-03-01 00:00:00 |          1166 |              372 |              0 |
| 621 | 2023-03-01 00:00:00 |          2406 |              426 |              1 |
| 622 | 2023-03-01 00:00:00 |          2761 |               41 |              1 |
| 623 | 2023-03-01 00:00:00 |          1371 |              317 |              0 |
| 624 | 2023-03-01 00:00:00 |          1250 |              189 |              0 |
| 625 | 2023-03-01 00:00:00 |          2844 |              252 |              1 |
| 626 | 2023-03-01 00:00:00 |          2577 |              348 |              0 |
| 627 | 2023-03-01 00:00:00 |          2679 |              323 |              1 |
| 628 | 2023-03-01 00:00:00 |          1810 |              114 |              0 |
| 629 | 2023-03-01 00:00:00 |          1902 |              487 |              0 |
| 630 | 2023-03-02 00:00:00 |          1282 |              471 |              0 |
| 631 | 2023-03-02 00:00:00 |          2509 |              253 |              0 |
| 632 | 2023-03-02 00:00:00 |          1893 |              331 |              0 |
| 633 | 2023-03-02 00:00:00 |          1475 |               40 |              1 |
| 634 | 2023-03-02 00:00:00 |          1270 |              496 |              0 |
| 635 | 2023-03-02 00:00:00 |          1373 |              467 |              0 |
| 636 | 2023-03-02 00:00:00 |          2382 |              175 |              0 |
| 637 | 2023-03-02 00:00:00 |          2314 |               43 |              0 |
| 638 | 2023-03-02 00:00:00 |          1789 |              298 |              0 |
| 639 | 2023-03-02 00:00:00 |          2000 |              321 |              0 |
| 640 | 2023-03-02 00:00:00 |          1273 |              362 |              0 |
| 641 | 2023-03-02 00:00:00 |          1773 |              156 |              0 |
| 642 | 2023-03-02 00:00:00 |          2618 |              270 |              0 |
| 643 | 2023-03-02 00:00:00 |          1139 |               49 |              0 |
| 644 | 2023-03-02 00:00:00 |          1990 |               78 |              1 |
| 645 | 2023-03-02 00:00:00 |          1665 |              347 |              0 |
| 646 | 2023-03-02 00:00:00 |          1339 |              138 |              0 |
| 647 | 2023-03-03 00:00:00 |          1884 |              151 |              0 |
| 648 | 2023-03-03 00:00:00 |          1249 |              359 |              0 |
| 649 | 2023-03-03 00:00:00 |          1876 |              464 |              0 |
| 650 | 2023-03-03 00:00:00 |          1165 |              464 |              0 |
| 651 | 2023-03-03 00:00:00 |          1272 |              374 |              0 |
| 652 | 2023-03-03 00:00:00 |          2569 |              187 |              0 |
| 653 | 2023-03-03 00:00:00 |          1087 |              221 |              0 |
| 654 | 2023-03-03 00:00:00 |          2889 |               26 |              0 |
| 655 | 2023-03-03 00:00:00 |          2240 |               80 |              0 |
| 656 | 2023-03-03 00:00:00 |          2527 |              207 |              0 |
| 657 | 2023-03-03 00:00:00 |          2263 |              305 |              1 |
| 658 | 2023-03-04 00:00:00 |          2685 |              486 |              0 |
| 659 | 2023-03-04 00:00:00 |          2655 |              297 |              1 |
| 660 | 2023-03-04 00:00:00 |          1448 |              493 |              0 |
| 661 | 2023-03-04 00:00:00 |          1805 |              329 |              0 |
| 662 | 2023-03-05 00:00:00 |          1651 |              479 |              1 |
| 663 | 2023-03-05 00:00:00 |          2541 |              330 |              0 |
| 664 | 2023-03-05 00:00:00 |          2614 |              401 |              0 |
| 665 | 2023-03-05 00:00:00 |          2257 |              101 |              1 |
| 666 | 2023-03-05 00:00:00 |          2890 |               50 |              0 |
| 667 | 2023-03-05 00:00:00 |          1300 |              369 |              0 |
| 668 | 2023-03-05 00:00:00 |          1627 |              403 |              1 |
| 669 | 2023-03-05 00:00:00 |          2809 |               20 |              0 |
| 670 | 2023-03-05 00:00:00 |          2010 |              438 |              0 |
| 671 | 2023-03-05 00:00:00 |          2045 |              207 |              1 |
| 672 | 2023-03-05 00:00:00 |          1657 |              121 |              0 |
| 673 | 2023-03-05 00:00:00 |          1143 |              405 |              1 |
| 674 | 2023-03-05 00:00:00 |          2553 |              257 |              1 |
| 675 | 2023-03-05 00:00:00 |          2485 |              426 |              0 |
| 676 | 2023-03-05 00:00:00 |          1267 |              214 |              0 |
| 677 | 2023-03-05 00:00:00 |          2974 |              316 |              0 |
| 678 | 2023-03-05 00:00:00 |          2930 |              280 |              1 |
| 679 | 2023-03-06 00:00:00 |          2452 |              394 |              1 |
| 680 | 2023-03-06 00:00:00 |          1492 |              220 |              0 |
| 681 | 2023-03-06 00:00:00 |          1197 |               22 |              0 |
| 682 | 2023-03-06 00:00:00 |          2636 |              249 |              0 |
| 683 | 2023-03-06 00:00:00 |          1657 |              175 |              0 |
| 684 | 2023-03-06 00:00:00 |          2352 |              325 |              0 |
| 685 | 2023-03-06 00:00:00 |          1011 |              132 |              0 |
| 686 | 2023-03-07 00:00:00 |          1131 |              461 |              0 |
| 687 | 2023-03-07 00:00:00 |          2959 |              464 |              0 |
| 688 | 2023-03-07 00:00:00 |          1949 |              177 |              1 |
| 689 | 2023-03-07 00:00:00 |          2399 |              223 |              1 |
| 690 | 2023-03-07 00:00:00 |          1079 |              110 |              0 |
| 691 | 2023-03-07 00:00:00 |          1926 |              153 |              0 |
| 692 | 2023-03-07 00:00:00 |          1544 |              335 |              1 |
| 693 | 2023-03-07 00:00:00 |          2340 |              145 |              0 |
| 694 | 2023-03-07 00:00:00 |          2179 |              270 |              0 |
| 695 | 2023-03-07 00:00:00 |          1126 |               49 |              0 |
| 696 | 2023-03-07 00:00:00 |          2230 |              117 |              0 |
| 697 | 2023-03-07 00:00:00 |          2782 |              478 |              0 |
| 698 | 2023-03-07 00:00:00 |          2064 |              198 |              0 |
| 699 | 2023-03-07 00:00:00 |          2147 |              170 |              0 |
| 700 | 2023-03-07 00:00:00 |          1964 |              488 |              0 |
| 701 | 2023-03-07 00:00:00 |          1884 |              197 |              0 |
| 702 | 2023-03-08 00:00:00 |          1360 |              231 |              0 |
| 703 | 2023-03-08 00:00:00 |          2970 |               93 |              0 |
| 704 | 2023-03-08 00:00:00 |          2735 |              293 |              0 |
| 705 | 2023-03-08 00:00:00 |          1201 |              311 |              1 |
| 706 | 2023-03-08 00:00:00 |          1375 |               68 |              0 |
| 707 | 2023-03-08 00:00:00 |          2659 |               87 |              0 |
| 708 | 2023-03-08 00:00:00 |          1293 |              476 |              0 |
| 709 | 2023-03-08 00:00:00 |          2500 |               75 |              0 |
| 710 | 2023-03-08 00:00:00 |          1264 |               69 |              1 |
| 711 | 2023-03-08 00:00:00 |          2086 |              342 |              0 |
| 712 | 2023-03-08 00:00:00 |          2291 |              214 |              1 |
| 713 | 2023-03-08 00:00:00 |          2124 |               40 |              0 |
| 714 | 2023-03-08 00:00:00 |          1455 |               72 |              0 |
| 715 | 2023-03-08 00:00:00 |          2650 |              290 |              0 |
| 716 | 2023-03-08 00:00:00 |          2056 |              327 |              0 |
| 717 | 2023-03-08 00:00:00 |          1774 |              367 |              0 |
| 718 | 2023-03-09 00:00:00 |          1855 |               95 |              0 |
| 719 | 2023-03-09 00:00:00 |          2724 |              316 |              0 |
| 720 | 2023-03-10 00:00:00 |          2477 |               82 |              1 |
| 721 | 2023-03-10 00:00:00 |          2554 |               96 |              0 |
| 722 | 2023-03-10 00:00:00 |          1717 |              232 |              1 |
| 723 | 2023-03-10 00:00:00 |          2173 |              127 |              1 |
| 724 | 2023-03-11 00:00:00 |          2897 |               87 |              0 |
| 725 | 2023-03-11 00:00:00 |          1703 |               46 |              1 |
| 726 | 2023-03-11 00:00:00 |          2961 |              257 |              1 |
| 727 | 2023-03-11 00:00:00 |          1568 |              452 |              0 |
| 728 | 2023-03-11 00:00:00 |          1863 |              234 |              0 |
| 729 | 2023-03-11 00:00:00 |          1391 |              225 |              0 |
| 730 | 2023-03-11 00:00:00 |          2579 |              216 |              0 |
| 731 | 2023-03-11 00:00:00 |          2372 |              145 |              0 |
| 732 | 2023-03-11 00:00:00 |          2819 |              206 |              0 |
| 733 | 2023-03-12 00:00:00 |          2117 |              313 |              0 |
| 734 | 2023-03-12 00:00:00 |          1887 |              497 |              1 |
| 735 | 2023-03-12 00:00:00 |          1732 |              459 |              0 |
| 736 | 2023-03-12 00:00:00 |          1640 |              157 |              0 |
| 737 | 2023-03-12 00:00:00 |          2074 |              475 |              0 |
| 738 | 2023-03-12 00:00:00 |          2438 |              272 |              0 |
| 739 | 2023-03-12 00:00:00 |          1645 |              402 |              0 |
| 740 | 2023-03-12 00:00:00 |          1445 |              494 |              0 |
| 741 | 2023-03-12 00:00:00 |          2227 |              365 |              0 |
| 742 | 2023-03-12 00:00:00 |          1084 |              179 |              0 |
| 743 | 2023-03-12 00:00:00 |          1178 |              265 |              0 |
| 744 | 2023-03-12 00:00:00 |          1379 |               88 |              0 |
| 745 | 2023-03-13 00:00:00 |          2187 |               33 |              0 |
| 746 | 2023-03-13 00:00:00 |          1359 |              415 |              0 |
| 747 | 2023-03-13 00:00:00 |          2444 |              330 |              0 |
| 748 | 2023-03-13 00:00:00 |          1162 |              199 |              0 |
| 749 | 2023-03-13 00:00:00 |          1393 |               48 |              0 |
| 750 | 2023-03-13 00:00:00 |          2776 |              165 |              0 |
| 751 | 2023-03-13 00:00:00 |          2374 |              173 |              0 |
| 752 | 2023-03-13 00:00:00 |          2403 |              207 |              0 |
| 753 | 2023-03-13 00:00:00 |          2210 |               79 |              0 |
| 754 | 2023-03-13 00:00:00 |          2307 |              405 |              0 |
| 755 | 2023-03-13 00:00:00 |          1784 |              465 |              0 |
| 756 | 2023-03-13 00:00:00 |          1512 |              282 |              0 |
| 757 | 2023-03-13 00:00:00 |          2851 |               87 |              1 |
| 758 | 2023-03-13 00:00:00 |          1428 |              440 |              0 |
| 759 | 2023-03-13 00:00:00 |          1511 |              266 |              0 |
| 760 | 2023-03-13 00:00:00 |          2962 |              186 |              0 |
| 761 | 2023-03-14 00:00:00 |          1781 |              111 |              0 |
| 762 | 2023-03-14 00:00:00 |          1646 |              348 |              0 |
| 763 | 2023-03-14 00:00:00 |          2923 |              118 |              0 |
| 764 | 2023-03-14 00:00:00 |          2576 |              281 |              0 |
| 765 | 2023-03-14 00:00:00 |          1278 |              201 |              0 |
| 766 | 2023-03-15 00:00:00 |          1187 |              255 |              0 |
| 767 | 2023-03-15 00:00:00 |          1344 |              410 |              1 |
| 768 | 2023-03-15 00:00:00 |          2584 |              115 |              1 |
| 769 | 2023-03-15 00:00:00 |          1625 |              478 |              0 |
| 770 | 2023-03-15 00:00:00 |          1956 |              115 |              0 |
| 771 | 2023-03-16 00:00:00 |          2264 |              381 |              0 |
| 772 | 2023-03-16 00:00:00 |          2228 |               61 |              0 |
| 773 | 2023-03-17 00:00:00 |          2528 |              164 |              1 |
| 774 | 2023-03-17 00:00:00 |          2550 |              441 |              0 |
| 775 | 2023-03-17 00:00:00 |          1303 |              152 |              0 |
| 776 | 2023-03-17 00:00:00 |          1254 |              441 |              0 |
| 777 | 2023-03-17 00:00:00 |          1360 |               89 |              1 |
| 778 | 2023-03-17 00:00:00 |          2802 |              292 |              0 |
| 779 | 2023-03-17 00:00:00 |          2603 |              451 |              0 |
| 780 | 2023-03-17 00:00:00 |          1094 |              294 |              0 |
| 781 | 2023-03-17 00:00:00 |          1047 |              446 |              1 |
| 782 | 2023-03-17 00:00:00 |          2354 |               75 |              0 |
| 783 | 2023-03-17 00:00:00 |          2090 |              103 |              0 |
| 784 | 2023-03-17 00:00:00 |          1787 |              390 |              0 |
| 785 | 2023-03-18 00:00:00 |          1244 |              448 |              0 |
| 786 | 2023-03-18 00:00:00 |          2146 |              242 |              0 |
| 787 | 2023-03-19 00:00:00 |          1080 |              420 |              0 |
| 788 | 2023-03-19 00:00:00 |          1813 |              472 |              0 |
| 789 | 2023-03-19 00:00:00 |          1588 |              178 |              1 |
| 790 | 2023-03-19 00:00:00 |          1602 |              437 |              0 |
| 791 | 2023-03-19 00:00:00 |          2154 |              140 |              0 |
| 792 | 2023-03-19 00:00:00 |          1525 |              301 |              0 |
| 793 | 2023-03-19 00:00:00 |          2081 |               82 |              0 |
| 794 | 2023-03-19 00:00:00 |          1107 |              425 |              0 |
| 795 | 2023-03-19 00:00:00 |          2049 |              325 |              0 |
| 796 | 2023-03-19 00:00:00 |          1943 |              346 |              0 |
| 797 | 2023-03-19 00:00:00 |          1584 |              414 |              1 |
| 798 | 2023-03-19 00:00:00 |          2886 |              371 |              1 |
| 799 | 2023-03-19 00:00:00 |          2200 |              314 |              0 |
| 800 | 2023-03-20 00:00:00 |          1631 |              474 |              0 |
| 801 | 2023-03-20 00:00:00 |          2217 |              211 |              0 |
| 802 | 2023-03-20 00:00:00 |          2792 |              322 |              0 |
| 803 | 2023-03-20 00:00:00 |          2684 |              174 |              0 |
| 804 | 2023-03-20 00:00:00 |          1241 |               54 |              0 |
| 805 | 2023-03-20 00:00:00 |          1667 |              434 |              1 |
| 806 | 2023-03-20 00:00:00 |          2588 |              348 |              1 |
| 807 | 2023-03-20 00:00:00 |          1048 |              338 |              0 |
| 808 | 2023-03-20 00:00:00 |          2962 |              319 |              0 |
| 809 | 2023-03-20 00:00:00 |          2017 |              387 |              0 |
| 810 | 2023-03-20 00:00:00 |          1997 |              110 |              0 |
| 811 | 2023-03-20 00:00:00 |          2061 |              445 |              0 |
| 812 | 2023-03-20 00:00:00 |          2027 |              105 |              1 |
| 813 | 2023-03-20 00:00:00 |          1413 |              287 |              0 |
| 814 | 2023-03-20 00:00:00 |          1363 |              302 |              0 |
| 815 | 2023-03-20 00:00:00 |          2588 |              183 |              0 |
| 816 | 2023-03-20 00:00:00 |          2212 |              414 |              0 |
| 817 | 2023-03-21 00:00:00 |          1132 |              303 |              0 |
| 818 | 2023-03-21 00:00:00 |          1241 |              123 |              0 |
| 819 | 2023-03-21 00:00:00 |          1619 |              383 |              0 |
| 820 | 2023-03-21 00:00:00 |          2030 |               63 |              0 |
| 821 | 2023-03-21 00:00:00 |          2448 |              165 |              0 |
| 822 | 2023-03-21 00:00:00 |          2810 |              472 |              0 |
| 823 | 2023-03-21 00:00:00 |          1392 |               47 |              0 |
| 824 | 2023-03-21 00:00:00 |          1215 |               71 |              0 |
| 825 | 2023-03-21 00:00:00 |          1525 |              178 |              0 |
| 826 | 2023-03-21 00:00:00 |          2778 |              418 |              0 |
| 827 | 2023-03-21 00:00:00 |          1260 |              117 |              0 |
| 828 | 2023-03-21 00:00:00 |          1063 |              392 |              0 |
| 829 | 2023-03-21 00:00:00 |          1119 |              220 |              0 |
| 830 | 2023-03-21 00:00:00 |          2421 |              258 |              0 |
| 831 | 2023-03-21 00:00:00 |          1360 |               98 |              0 |
| 832 | 2023-03-21 00:00:00 |          1700 |              450 |              0 |
| 833 | 2023-03-22 00:00:00 |          2516 |               95 |              0 |
| 834 | 2023-03-22 00:00:00 |          2851 |              428 |              0 |
| 835 | 2023-03-22 00:00:00 |          1672 |              458 |              0 |
| 836 | 2023-03-22 00:00:00 |          2010 |              228 |              0 |
| 837 | 2023-03-22 00:00:00 |          2393 |              134 |              0 |
| 838 | 2023-03-22 00:00:00 |          1789 |              301 |              0 |
| 839 | 2023-03-22 00:00:00 |          1762 |              496 |              0 |
| 840 | 2023-03-22 00:00:00 |          1859 |              476 |              0 |
| 841 | 2023-03-22 00:00:00 |          1103 |              127 |              0 |
| 842 | 2023-03-22 00:00:00 |          2961 |              264 |              0 |
| 843 | 2023-03-22 00:00:00 |          1820 |              394 |              0 |
| 844 | 2023-03-22 00:00:00 |          2424 |              454 |              0 |
| 845 | 2023-03-22 00:00:00 |          2462 |              155 |              1 |
| 846 | 2023-03-22 00:00:00 |          1313 |              351 |              1 |
| 847 | 2023-03-23 00:00:00 |          1232 |              476 |              0 |
| 848 | 2023-03-23 00:00:00 |          2324 |              190 |              0 |
| 849 | 2023-03-23 00:00:00 |          1838 |              363 |              0 |
| 850 | 2023-03-23 00:00:00 |          2714 |               74 |              0 |
| 851 | 2023-03-23 00:00:00 |          1357 |              482 |              0 |
| 852 | 2023-03-23 00:00:00 |          2196 |              195 |              0 |
| 853 | 2023-03-23 00:00:00 |          1123 |              413 |              1 |
| 854 | 2023-03-23 00:00:00 |          2502 |               54 |              0 |
| 855 | 2023-03-23 00:00:00 |          1862 |              226 |              1 |
| 856 | 2023-03-23 00:00:00 |          1046 |              362 |              1 |
| 857 | 2023-03-23 00:00:00 |          2885 |              313 |              0 |
| 858 | 2023-03-23 00:00:00 |          1082 |              366 |              0 |
| 859 | 2023-03-23 00:00:00 |          2451 |               94 |              0 |
| 860 | 2023-03-23 00:00:00 |          1176 |              494 |              1 |
| 861 | 2023-03-24 00:00:00 |          2764 |              431 |              0 |
| 862 | 2023-03-24 00:00:00 |          1013 |              253 |              0 |
| 863 | 2023-03-25 00:00:00 |          2323 |              141 |              1 |
| 864 | 2023-03-25 00:00:00 |          2268 |              188 |              0 |
| 865 | 2023-03-25 00:00:00 |          1000 |              489 |              0 |
| 866 | 2023-03-25 00:00:00 |          2798 |              418 |              0 |
| 867 | 2023-03-25 00:00:00 |          2348 |               23 |              1 |
| 868 | 2023-03-25 00:00:00 |          2947 |              371 |              0 |
| 869 | 2023-03-25 00:00:00 |          2826 |               87 |              0 |
| 870 | 2023-03-25 00:00:00 |          2137 |              403 |              0 |
| 871 | 2023-03-25 00:00:00 |          2517 |              188 |              0 |
| 872 | 2023-03-25 00:00:00 |          1323 |               62 |              0 |
| 873 | 2023-03-25 00:00:00 |          1754 |              163 |              0 |
| 874 | 2023-03-26 00:00:00 |          2714 |              123 |              0 |
| 875 | 2023-03-26 00:00:00 |          2945 |               72 |              0 |
| 876 | 2023-03-26 00:00:00 |          1645 |              101 |              1 |
| 877 | 2023-03-26 00:00:00 |          2860 |              275 |              0 |
| 878 | 2023-03-26 00:00:00 |          1754 |              126 |              0 |
| 879 | 2023-03-26 00:00:00 |          2234 |              445 |              0 |
| 880 | 2023-03-26 00:00:00 |          1649 |              384 |              0 |
| 881 | 2023-03-26 00:00:00 |          1161 |               45 |              0 |
| 882 | 2023-03-26 00:00:00 |          2552 |              451 |              0 |
| 883 | 2023-03-26 00:00:00 |          2886 |              347 |              0 |
| 884 | 2023-03-26 00:00:00 |          1507 |              106 |              0 |
| 885 | 2023-03-27 00:00:00 |          2112 |              160 |              1 |
| 886 | 2023-03-27 00:00:00 |          2208 |              426 |              1 |
| 887 | 2023-03-27 00:00:00 |          1702 |              309 |              1 |
| 888 | 2023-03-27 00:00:00 |          2100 |              145 |              0 |
| 889 | 2023-03-27 00:00:00 |          2733 |              276 |              0 |
| 890 | 2023-03-27 00:00:00 |          1689 |              373 |              0 |
| 891 | 2023-03-27 00:00:00 |          1198 |              414 |              0 |
| 892 | 2023-03-27 00:00:00 |          1411 |              219 |              0 |
| 893 | 2023-03-27 00:00:00 |          2449 |              104 |              0 |
| 894 | 2023-03-27 00:00:00 |          1934 |              474 |              0 |
| 895 | 2023-03-27 00:00:00 |          2791 |              222 |              1 |
| 896 | 2023-03-27 00:00:00 |          2947 |               60 |              0 |
| 897 | 2023-03-27 00:00:00 |          1098 |              184 |              0 |
| 898 | 2023-03-27 00:00:00 |          1696 |              431 |              1 |
| 899 | 2023-03-27 00:00:00 |          1816 |              307 |              1 |
| 900 | 2023-03-28 00:00:00 |          2573 |              340 |              0 |
| 901 | 2023-03-28 00:00:00 |          1852 |               32 |              0 |
| 902 | 2023-03-28 00:00:00 |          1370 |              140 |              0 |
| 903 | 2023-03-28 00:00:00 |          1149 |              333 |              1 |
| 904 | 2023-03-29 00:00:00 |          1423 |              165 |              0 |
| 905 | 2023-03-29 00:00:00 |          1439 |              177 |              0 |
| 906 | 2023-03-29 00:00:00 |          1791 |               20 |              0 |
| 907 | 2023-03-29 00:00:00 |          1053 |              488 |              0 |
| 908 | 2023-03-29 00:00:00 |          1392 |              289 |              1 |
| 909 | 2023-03-29 00:00:00 |          1419 |              185 |              0 |
| 910 | 2023-03-29 00:00:00 |          1972 |              370 |              0 |
| 911 | 2023-03-29 00:00:00 |          2338 |              356 |              1 |
| 912 | 2023-03-29 00:00:00 |          1746 |              262 |              1 |
| 913 | 2023-03-29 00:00:00 |          2269 |              484 |              1 |
| 914 | 2023-03-29 00:00:00 |          2935 |              478 |              1 |
| 915 | 2023-03-29 00:00:00 |          2775 |              364 |              0 |
| 916 | 2023-03-29 00:00:00 |          1916 |              494 |              0 |
| 917 | 2023-03-29 00:00:00 |          1353 |              466 |              0 |
| 918 | 2023-03-29 00:00:00 |          1137 |              316 |              0 |
| 919 | 2023-03-29 00:00:00 |          1297 |              496 |              1 |
| 920 | 2023-03-30 00:00:00 |          2151 |              482 |              1 |
| 921 | 2023-03-30 00:00:00 |          1235 |              235 |              0 |
| 922 | 2023-03-30 00:00:00 |          2914 |              183 |              0 |
| 923 | 2023-03-30 00:00:00 |          2578 |              310 |              0 |
| 924 | 2023-03-31 00:00:00 |          2079 |              358 |              0 |
| 925 | 2023-03-31 00:00:00 |          2089 |              375 |              0 |
| 926 | 2023-03-31 00:00:00 |          1355 |              249 |              0 |
| 927 | 2023-03-31 00:00:00 |          1062 |              111 |              0 |
| 928 | 2023-03-31 00:00:00 |          1590 |              271 |              0 |


Calculate total customers in the last month

[In]

```
total_customers=last_month_payment['customer_id'].nunique()
total_customers
```

[Out]

```
292
```

Get last month data for new customers

[In]

```
cond=df_payment['new_customer']==1
last_month_payments_from_new_customers=last_month_payment[cond]
last_month_payments_from_new_customers
```

[Out]

|     | date                |   customer_id |   receipt_amount |   new_customer |
|----:|:--------------------|--------------:|-----------------:|---------------:|
| 621 | 2023-03-01 00:00:00 |          2406 |              426 |              1 |
| 622 | 2023-03-01 00:00:00 |          2761 |               41 |              1 |
| 625 | 2023-03-01 00:00:00 |          2844 |              252 |              1 |
| 627 | 2023-03-01 00:00:00 |          2679 |              323 |              1 |
| 633 | 2023-03-02 00:00:00 |          1475 |               40 |              1 |
| 644 | 2023-03-02 00:00:00 |          1990 |               78 |              1 |
| 657 | 2023-03-03 00:00:00 |          2263 |              305 |              1 |
| 659 | 2023-03-04 00:00:00 |          2655 |              297 |              1 |
| 662 | 2023-03-05 00:00:00 |          1651 |              479 |              1 |
| 665 | 2023-03-05 00:00:00 |          2257 |              101 |              1 |
| 668 | 2023-03-05 00:00:00 |          1627 |              403 |              1 |
| 671 | 2023-03-05 00:00:00 |          2045 |              207 |              1 |
| 673 | 2023-03-05 00:00:00 |          1143 |              405 |              1 |
| 674 | 2023-03-05 00:00:00 |          2553 |              257 |              1 |
| 678 | 2023-03-05 00:00:00 |          2930 |              280 |              1 |
| 679 | 2023-03-06 00:00:00 |          2452 |              394 |              1 |
| 688 | 2023-03-07 00:00:00 |          1949 |              177 |              1 |
| 689 | 2023-03-07 00:00:00 |          2399 |              223 |              1 |
| 692 | 2023-03-07 00:00:00 |          1544 |              335 |              1 |
| 705 | 2023-03-08 00:00:00 |          1201 |              311 |              1 |
| 710 | 2023-03-08 00:00:00 |          1264 |               69 |              1 |
| 712 | 2023-03-08 00:00:00 |          2291 |              214 |              1 |
| 720 | 2023-03-10 00:00:00 |          2477 |               82 |              1 |
| 722 | 2023-03-10 00:00:00 |          1717 |              232 |              1 |
| 723 | 2023-03-10 00:00:00 |          2173 |              127 |              1 |
| 725 | 2023-03-11 00:00:00 |          1703 |               46 |              1 |
| 726 | 2023-03-11 00:00:00 |          2961 |              257 |              1 |
| 734 | 2023-03-12 00:00:00 |          1887 |              497 |              1 |
| 757 | 2023-03-13 00:00:00 |          2851 |               87 |              1 |
| 767 | 2023-03-15 00:00:00 |          1344 |              410 |              1 |
| 768 | 2023-03-15 00:00:00 |          2584 |              115 |              1 |
| 773 | 2023-03-17 00:00:00 |          2528 |              164 |              1 |
| 777 | 2023-03-17 00:00:00 |          1360 |               89 |              1 |
| 781 | 2023-03-17 00:00:00 |          1047 |              446 |              1 |
| 789 | 2023-03-19 00:00:00 |          1588 |              178 |              1 |
| 797 | 2023-03-19 00:00:00 |          1584 |              414 |              1 |
| 798 | 2023-03-19 00:00:00 |          2886 |              371 |              1 |
| 805 | 2023-03-20 00:00:00 |          1667 |              434 |              1 |
| 806 | 2023-03-20 00:00:00 |          2588 |              348 |              1 |
| 812 | 2023-03-20 00:00:00 |          2027 |              105 |              1 |
| 845 | 2023-03-22 00:00:00 |          2462 |              155 |              1 |
| 846 | 2023-03-22 00:00:00 |          1313 |              351 |              1 |
| 853 | 2023-03-23 00:00:00 |          1123 |              413 |              1 |
| 855 | 2023-03-23 00:00:00 |          1862 |              226 |              1 |
| 856 | 2023-03-23 00:00:00 |          1046 |              362 |              1 |
| 860 | 2023-03-23 00:00:00 |          1176 |              494 |              1 |
| 863 | 2023-03-25 00:00:00 |          2323 |              141 |              1 |
| 867 | 2023-03-25 00:00:00 |          2348 |               23 |              1 |
| 876 | 2023-03-26 00:00:00 |          1645 |              101 |              1 |
| 885 | 2023-03-27 00:00:00 |          2112 |              160 |              1 |
| 886 | 2023-03-27 00:00:00 |          2208 |              426 |              1 |
| 887 | 2023-03-27 00:00:00 |          1702 |              309 |              1 |
| 895 | 2023-03-27 00:00:00 |          2791 |              222 |              1 |
| 898 | 2023-03-27 00:00:00 |          1696 |              431 |              1 |
| 899 | 2023-03-27 00:00:00 |          1816 |              307 |              1 |
| 903 | 2023-03-28 00:00:00 |          1149 |              333 |              1 |
| 908 | 2023-03-29 00:00:00 |          1392 |              289 |              1 |
| 911 | 2023-03-29 00:00:00 |          2338 |              356 |              1 |
| 912 | 2023-03-29 00:00:00 |          1746 |              262 |              1 |
| 913 | 2023-03-29 00:00:00 |          2269 |              484 |              1 |
| 914 | 2023-03-29 00:00:00 |          2935 |              478 |              1 |
| 919 | 2023-03-29 00:00:00 |          1297 |              496 |              1 |
| 920 | 2023-03-30 00:00:00 |          2151 |              482 |              1 |


Calculate number of new customers:

[In]

```
Number_of_customers=last_month_payments_from_new_customers['customer_id'].nunique()
Number_of_customers
```

[Out]

```
63
```

Calculate percentage of new customers

[In]

```
percentage_of_new_customers=Number_of_customers/total_customers
percentage_of_new_customers
```

[Out]

```
0.21575342465753425
```

**Calculate last month CAC for new customers:**

[In]

```
CAC = Total_expenses/Number_of_customers
CAC
```

[Out]

```
1213.968253968254
```

### **ARPU**

Calculate total revenue for new customers

[In]

```
total_revenue_new_customers = last_month_payments_from_new_customers['receipt_amount'].sum()
total_revenue_new_customers
```

[Out]

```
17320
```

ARPU Calculation

[In]

```
ARPU = total_revenue_new_customers/Number_of_customers
ARPU
```

[Out]

```
274.92063492063494
```

### **COST OF GOODS SOLD**

Calculate last month expenses related to product:

[in]

```
last_month_product_expense = last_month_expense[last_month_expense['item'].isin(['Atlassian Jira','Slack','AWS Hosting','Google Cloud Storage'])]
last_month_product_expense
```

[Out]

|    |   # | month               | category          | item                 |   amount |
|---:|----:|:--------------------|:------------------|:---------------------|---------:|
| 18 |  19 | 2023-03-01 00:00:00 | Server Costs      | AWS Hosting          |     8400 |
| 19 |  20 | 2023-03-01 00:00:00 | Server Costs      | Google Cloud Storage |     4400 |
| 20 |  21 | 2023-03-01 00:00:00 | Software Licenses | Atlassian Jira       |     1400 |
| 21 |  22 | 2023-03-01 00:00:00 | Software Licenses | Slack                |      900 |


Calculate last month direct product cost

[In]

```
last_month_product_cost=last_month_product_expense['amount'].sum()
last_month_product_cost
```

[Out]

```
15100
```

Calculate last month direct product cost of new customers

[In]

```
last_month_product_cost_new_customers=last_month_product_cost * percentage_of_new_customers
last_month_product_cost_new_customers
```

[Out]

```
3257.87671232876
```
 
Calculate percentage of production employee

[In]

```
# total employee

total_employees = df_payroll['employee_name'].nunique()

# percentage of production employee

percentage_of_production_employee = (department_employee_count['Engineering'] + department_employee_count['Support'])/ total_employees
percentage_of_production_employee
```

[Out]

```
0.35294117647058826
```

Allocated others expenses to production cost

[In]

```
last_month_other_expense = last_month_expense[last_month_expense['item'].isin(['Zoom','Office Rent','Office Supplies','Travel Expenses'])]
last_month_other_expense = last_month_other_expense['amount'].sum()
last_month_other_production_cost = last_month_other_expense * percentage_of_production_employee
last_month_other_production_cost
```

[Out]

```
5061.176470588236
```


Allocated others expenses to production cost of new customers

[In]

```
last_month_other_production_cost_new_customers = last_month_other_production_cost * percentage_of_new_customers
last_month_other_production_cost_new_customers
```

[Out]

```
1091.966156325544
```

Calculate last mont total production cost of new customers:

[In]

```
last_month_total_production_cost_new_customers = last_month_product_cost_new_customers + last_month_other_production_cost_new_customers
last_month_total_production_cost_new_customers
```

[Out]

```
4349.842868654311
```

**Calculate labour cost**

Calculate last month salary for production team

[In]

```
last_month_salary_product_cost=df_last_month_payroll[(df_last_month_payroll['department']=='Engineering')|(df_last_month_payroll['department']=='Support')]
last_month_salary_product_cost

last_month_labor_cost=last_month_salary_product_cost['paid'].sum()
last_month_labor_cost
```

[Out]

|    | month               | department   | employee_name   | position           |   paid | yyyy-mm   |
|---:|:--------------------|:-------------|:----------------|:-------------------|-------:|:----------|
| 40 | 2023-03-01 00:00:00 | Engineering  | Charlie Wilson  | Senior Developer   |   2500 | 2023-03   |
| 41 | 2023-03-01 00:00:00 | Engineering  | Diana Lee       | Developer          |   1700 | 2023-03   |
| 42 | 2023-03-01 00:00:00 | Engineering  | Eva White       | Junior Developer   |   1000 | 2023-03   |
| 45 | 2023-03-01 00:00:00 | Support      | Hannah Scott    | Support Lead       |    600 | 2023-03   |
| 46 | 2023-03-01 00:00:00 | Support      | Ian Harris      | Support Specialist |    500 | 2023-03   |
| 47 | 2023-03-01 00:00:00 | Support      | Emily Clark     | Support Specialist |    450 | 2023-03   |

```
6750
```

Calculate last month labor cost of new customers


[In]

```
last_month_labor_cost_new_customers=last_month_labor_cost*percentage_of_new_customers
last_month_labor_cost_new_customers
```

[Out]

```
1456.3356164383563
```

Calculate COGS of new customers

[In]

```
COGS_new_customers = last_month_total_production_cost_new_customers + last_month_labor_cost_new_customers
COGS_new_customers
```

[Out]

```
5806.178485092667
```

### **Gross Margin**

Calculate Gross Margin

[In]

```
Gross_margin_new_customers= (total_revenue_new_customers-COGS_new_customers)/total_revenue_new_customers
Gross_margin_new_customers
```

[Out]

```
0.6647702953179754
```

### **LTV Calculation**

Customer's life

[In]

```
df_customer_life['customer_life'] = (df_customer_life['churn_date'] - df_customer_life['start_date']).dt.days
df_customer_life.info()
df_customer_life.head()
```

[Out]


![image](https://github.com/user-attachments/assets/a73d6ab8-0f40-402e-a926-ae53cfe4a680)


|    |   Unnamed: 0 | start_date          | churn_date          |   customer_life |
|---:|-------------:|:--------------------|:--------------------|----------------:|
|  0 |         1000 | 2021-11-15 00:00:00 | 2022-09-14 00:00:00 |             303 |
|  1 |         1001 | 2022-04-15 00:00:00 | 2023-02-16 00:00:00 |             307 |
|  2 |         1002 | 2022-10-30 00:00:00 | 2023-02-04 00:00:00 |              97 |
|  3 |         1003 | 2021-08-22 00:00:00 | 2023-02-07 00:00:00 |             534 |
|  4 |         1004 | 2021-08-23 00:00:00 | 2022-02-02 00:00:00 |             163 |


Calculate average customer life (month)

[In]

```
AVG_customer_life = df_customer_life['customer_life'].mean()/30
AVG_customer_life
```

[Out]

```
9.841333333333333
```

**Calculate LTV**

[In]

```
LTV = ARPU * AVG_customer_life * Gross_margin_new_customers
LTV
```

[Out]

```
1798.5929439477466
```

### **LTV/CAC ration**


Calculate LTV/CAC ratio

[In]

```
LTV_CAC_ratio = LTV / CAC
LTV_CAC_ratio
```

[Out]

```
1.4815815307100946
```

## **Result**


**CAC:** 1213.968253968254

**ARPU:** 274.92063492063494

**COGS:** 5806.178485092667

**Gross Margin:** 0.6647702953179754

**LTV:** 1798.5929439477466

**LTV/CAC ratio:** 1.4815815307100946


# **4. Conclusion**

**LTV/CAC ratio = 1.4816**

The LTV/CAC ratio is greater than 1, indicates that TechStream Solutions is generating more revenue from customers than it costs to acquire them. This is a positive sign, suggesting profitability and sustainable growth.

However, an ideal LTV/CAC ratio typically ranges from 3 to 5, indicating a healthy balance between customer acquisition costs and the value generated from customers. TechStream Solutions shoud try to improve this ratio to ideal range.

The average customer lifespans of TechStream Solution is only 9.81 months, company should find solutions to improve this lifespan in order to imporve LTV and LTV/CAC ratio.
