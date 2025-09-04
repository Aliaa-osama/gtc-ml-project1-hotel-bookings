---
jupyter:
  colab:
    collapsed_sections:
    - 58XjQULFjTuR
    - LieuQIygx2Gj
    - 0pNgYovXrywP
    - LJjt0k8Er6S4
    - Ph82fovwpXk-
  kernelspec:
    display_name: Python 3
    name: python3
  language_info:
    name: python
  nbformat: 4
  nbformat_minor: 0
---

::: {.cell .markdown id="58XjQULFjTuR"}
# Phase 1: Exploratory Data Analysis (EDA) & Data Quality Report {#phase-1-exploratory-data-analysis-eda--data-quality-report}
:::

::: {.cell .markdown id="vOPiaI8dxmUh"}
## Load the data and generate summary statistics
:::

::: {.cell .code execution_count="1" id="j1rvx396iJmG"}
``` python
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns
import missingno as msno
```
:::

::: {.cell .code execution_count="2" id="arj6xHHJjG8L"}
``` python
df=pd.read_csv('/content/hotel_bookings.csv')
```
:::

::: {.cell .code execution_count="3" colab="{\"base_uri\":\"https://localhost:8080/\",\"height\":342}" id="fh8QQc7UjLmQ" outputId="8b4716bc-0e5e-484b-9da7-1b3decbc476a"}
``` python
df.head()
```

::: {.output .execute_result execution_count="3"}
``` json
{"type":"dataframe","variable_name":"df"}
```
:::
:::

::: {.cell .code execution_count="4" colab="{\"base_uri\":\"https://localhost:8080/\"}" id="_awRePZajNCZ" outputId="8f3a471b-0168-4147-9426-f78b01e0238c"}
``` python
df.info()
```

::: {.output .stream .stdout}
    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 119390 entries, 0 to 119389
    Data columns (total 32 columns):
     #   Column                          Non-Null Count   Dtype  
    ---  ------                          --------------   -----  
     0   hotel                           119390 non-null  object 
     1   is_canceled                     119390 non-null  int64  
     2   lead_time                       119390 non-null  int64  
     3   arrival_date_year               119390 non-null  int64  
     4   arrival_date_month              119390 non-null  object 
     5   arrival_date_week_number        119390 non-null  int64  
     6   arrival_date_day_of_month       119390 non-null  int64  
     7   stays_in_weekend_nights         119390 non-null  int64  
     8   stays_in_week_nights            119390 non-null  int64  
     9   adults                          119390 non-null  int64  
     10  children                        119386 non-null  float64
     11  babies                          119390 non-null  int64  
     12  meal                            119390 non-null  object 
     13  country                         118902 non-null  object 
     14  market_segment                  119390 non-null  object 
     15  distribution_channel            119390 non-null  object 
     16  is_repeated_guest               119390 non-null  int64  
     17  previous_cancellations          119390 non-null  int64  
     18  previous_bookings_not_canceled  119390 non-null  int64  
     19  reserved_room_type              119390 non-null  object 
     20  assigned_room_type              119390 non-null  object 
     21  booking_changes                 119390 non-null  int64  
     22  deposit_type                    119390 non-null  object 
     23  agent                           103050 non-null  float64
     24  company                         6797 non-null    float64
     25  days_in_waiting_list            119390 non-null  int64  
     26  customer_type                   119390 non-null  object 
     27  adr                             119390 non-null  float64
     28  required_car_parking_spaces     119390 non-null  int64  
     29  total_of_special_requests       119390 non-null  int64  
     30  reservation_status              119390 non-null  object 
     31  reservation_status_date         119390 non-null  object 
    dtypes: float64(4), int64(16), object(12)
    memory usage: 29.1+ MB
:::
:::

::: {.cell .code execution_count="5" colab="{\"base_uri\":\"https://localhost:8080/\",\"height\":1000}" id="tqPRbNHPjSUc" outputId="f3000140-ff18-4cb6-d769-e70670dac243"}
``` python
df.describe(include='all').T
```

::: {.output .execute_result execution_count="5"}
``` json
{"summary":"{\n  \"name\": \"df\",\n  \"rows\": 32,\n  \"fields\": [\n    {\n      \"column\": \"count\",\n      \"properties\": {\n        \"dtype\": \"date\",\n        \"min\": 6797.0,\n        \"max\": \"119390\",\n        \"num_unique_values\": 5,\n        \"samples\": [\n          119386.0,\n          6797.0,\n          \"118902\"\n        ],\n        \"semantic_type\": \"\",\n        \"description\": \"\"\n      }\n    },\n    {\n      \"column\": \"unique\",\n      \"properties\": {\n        \"dtype\": \"date\",\n        \"min\": 2,\n        \"max\": 926,\n        \"num_unique_values\": 9,\n        \"samples\": [\n          4,\n          12,\n          10\n        ],\n        \"semantic_type\": \"\",\n        \"description\": \"\"\n      }\n    },\n    {\n      \"column\": \"top\",\n      \"properties\": {\n        \"dtype\": \"category\",\n        \"num_unique_values\": 11,\n        \"samples\": [\n          \"TA/TO\",\n          \"City Hotel\",\n          \"Check-Out\"\n        ],\n        \"semantic_type\": \"\",\n        \"description\": \"\"\n      }\n    },\n    {\n      \"column\": \"freq\",\n      \"properties\": {\n        \"dtype\": \"date\",\n        \"min\": \"1461\",\n        \"max\": \"104641\",\n        \"num_unique_values\": 12,\n        \"samples\": [\n          \"75166\",\n          \"89613\",\n          \"79330\"\n        ],\n        \"semantic_type\": \"\",\n        \"description\": \"\"\n      }\n    },\n    {\n      \"column\": \"mean\",\n      \"properties\": {\n        \"dtype\": \"date\",\n        \"min\": 0.007948739425412514,\n        \"max\": 2016.156554150264,\n        \"num_unique_values\": 20,\n        \"samples\": [\n          0.37041628277075134,\n          101.83112153446686,\n          189.26673532440782\n        ],\n        \"semantic_type\": \"\",\n        \"description\": \"\"\n      }\n    },\n    {\n      \"column\": \"std\",\n      \"properties\": {\n        \"dtype\": \"date\",\n        \"min\": 0.09743619130130332,\n        \"max\": 131.65501463850987,\n        \"num_unique_values\": 20,\n        \"samples\": [\n          0.48291822659316763,\n          50.5357902855456,\n          131.65501463850987\n        ],\n        \"semantic_type\": \"\",\n        \"description\": \"\"\n      }\n    },\n    {\n      \"column\": \"min\",\n      \"properties\": {\n        \"dtype\": \"date\",\n        \"min\": -6.38,\n        \"max\": 2015.0,\n        \"num_unique_values\": 5,\n        \"samples\": [\n          2015.0,\n          -6.38,\n          1.0\n        ],\n        \"semantic_type\": \"\",\n        \"description\": \"\"\n      }\n    },\n    {\n      \"column\": \"25%\",\n      \"properties\": {\n        \"dtype\": \"date\",\n        \"min\": 0.0,\n        \"max\": 2016.0,\n        \"num_unique_values\": 10,\n        \"samples\": [\n          62.0,\n          18.0,\n          1.0\n        ],\n        \"semantic_type\": \"\",\n        \"description\": \"\"\n      }\n    },\n    {\n      \"column\": \"50%\",\n      \"properties\": {\n        \"dtype\": \"date\",\n        \"min\": 0.0,\n        \"max\": 2016.0,\n        \"num_unique_values\": 10,\n        \"samples\": [\n          179.0,\n          69.0,\n          1.0\n        ],\n        \"semantic_type\": \"\",\n        \"description\": \"\"\n      }\n    },\n    {\n      \"column\": \"75%\",\n      \"properties\": {\n        \"dtype\": \"date\",\n        \"min\": 0.0,\n        \"max\": 2017.0,\n        \"num_unique_values\": 11,\n        \"samples\": [\n          2.0,\n          1.0,\n          270.0\n        ],\n        \"semantic_type\": \"\",\n        \"description\": \"\"\n      }\n    },\n    {\n      \"column\": \"max\",\n      \"properties\": {\n        \"dtype\": \"date\",\n        \"min\": 1.0,\n        \"max\": 5400.0,\n        \"num_unique_values\": 18,\n        \"samples\": [\n          1.0,\n          737.0,\n          10.0\n        ],\n        \"semantic_type\": \"\",\n        \"description\": \"\"\n      }\n    }\n  ]\n}","type":"dataframe"}
```
:::
:::

::: {.cell .markdown id="4JNdiwAHxq39"}
## Identify all missing values. {#identify-all-missing-values}
:::

::: {.cell .code execution_count="6" colab="{\"base_uri\":\"https://localhost:8080/\"}" id="uNycB7ESjfcd" outputId="222872fa-ef90-4864-fc18-4bb94aaefb2f"}
``` python
missing_values=df.isnull().sum()
missing_values_percentage=(df.isnull().sum()/len(df))*100
missing_values_df=pd.DataFrame({'Missing Values':missing_values,'Missing Values Percentage':missing_values_percentage})
print(missing_values_df)
print('--'*50)
#percentage of missing data

total_cells=np.prod(df.shape)
total_missing=missing_values_df['Missing Values'].sum()
missing_data_percentage=(total_missing/total_cells)*100
print(f'percentage of missing data:{missing_data_percentage}')
```

::: {.output .stream .stdout}
                                    Missing Values  Missing Values Percentage
    hotel                                        0                   0.000000
    is_canceled                                  0                   0.000000
    lead_time                                    0                   0.000000
    arrival_date_year                            0                   0.000000
    arrival_date_month                           0                   0.000000
    arrival_date_week_number                     0                   0.000000
    arrival_date_day_of_month                    0                   0.000000
    stays_in_weekend_nights                      0                   0.000000
    stays_in_week_nights                         0                   0.000000
    adults                                       0                   0.000000
    children                                     4                   0.003350
    babies                                       0                   0.000000
    meal                                         0                   0.000000
    country                                    488                   0.408744
    market_segment                               0                   0.000000
    distribution_channel                         0                   0.000000
    is_repeated_guest                            0                   0.000000
    previous_cancellations                       0                   0.000000
    previous_bookings_not_canceled               0                   0.000000
    reserved_room_type                           0                   0.000000
    assigned_room_type                           0                   0.000000
    booking_changes                              0                   0.000000
    deposit_type                                 0                   0.000000
    agent                                    16340                  13.686238
    company                                 112593                  94.306893
    days_in_waiting_list                         0                   0.000000
    customer_type                                0                   0.000000
    adr                                          0                   0.000000
    required_car_parking_spaces                  0                   0.000000
    total_of_special_requests                    0                   0.000000
    reservation_status                           0                   0.000000
    reservation_status_date                      0                   0.000000
    ----------------------------------------------------------------------------------------------------
    percentage of missing data:3.387663330262166
:::
:::

::: {.cell .code execution_count="7" colab="{\"base_uri\":\"https://localhost:8080/\",\"height\":612}" id="kHFiTgvHlehV" outputId="1d24054d-011f-4eb3-e87e-0baef9a8cc97"}
``` python
plt.figure(figsize=(10,8))
msno.matrix(df)
plt.show()
```

::: {.output .display_data}
    <Figure size 1000x800 with 0 Axes>
:::

::: {.output .display_data}
![](1fe05b84987ff7ea2f5c1fbd5b21bfbcd02ccfe9.png)
:::
:::

::: {.cell .code execution_count="8" colab="{\"base_uri\":\"https://localhost:8080/\",\"height\":785}" id="W50WbF7UmIaO" outputId="cb9e0350-a6ea-433b-ab90-9364d531193b"}
``` python
plt.figure(figsize=(10,8))
msno.heatmap(df)
plt.show()
```

::: {.output .display_data}
    <Figure size 1000x800 with 0 Axes>
:::

::: {.output .display_data}
![](acefa1c2dc71a94ab8b09a35e27eb49c39a9ac1a.png)
:::
:::

::: {.cell .markdown id="VPaMA0Lexu8c"}
## Detect outliers
:::

::: {.cell .code execution_count="9" colab="{\"base_uri\":\"https://localhost:8080/\",\"height\":913}" id="U0YPMm18mg5q" outputId="ce71e501-2649-4688-acc8-846e53f1bf8c"}
``` python
plt.figure(figsize=(10,8))
sns.boxplot(data=df.select_dtypes(include=['int64','float64']))
plt.xticks(rotation=90)   # rotate x-axis labels
plt.grid(True)
plt.title("Boxplots of All Numerical Columns")
plt.show()
```

::: {.output .display_data}
![](da73dc5e6d9310e1ce9eeddc9237e1901a7706f2.png)
:::
:::

::: {.cell .markdown id="LieuQIygx2Gj"}
## Document your findings
:::

::: {.cell .code execution_count="10" id="lhz7kdSgcG-a"}
``` python
def check_for_neagtive(column):
  negative_values=df[df[column]<0]
  print(f'Number of negative values in {column}:{len(negative_values)}')

  return negative_values
```
:::

::: {.cell .code execution_count="11" colab="{\"base_uri\":\"https://localhost:8080/\"}" id="bS4m34uRdMm2" outputId="9f603f9d-e56a-4558-e443-7e7c0661833a"}
``` python
for column in df.select_dtypes(include=['int64','float64']):
  check_for_neagtive(column)
```

::: {.output .stream .stdout}
    Number of negative values in is_canceled:0
    Number of negative values in lead_time:0
    Number of negative values in arrival_date_year:0
    Number of negative values in arrival_date_week_number:0
    Number of negative values in arrival_date_day_of_month:0
    Number of negative values in stays_in_weekend_nights:0
    Number of negative values in stays_in_week_nights:0
    Number of negative values in adults:0
    Number of negative values in children:0
    Number of negative values in babies:0
    Number of negative values in is_repeated_guest:0
    Number of negative values in previous_cancellations:0
    Number of negative values in previous_bookings_not_canceled:0
    Number of negative values in booking_changes:0
    Number of negative values in agent:0
    Number of negative values in company:0
    Number of negative values in days_in_waiting_list:0
    Number of negative values in adr:1
    Number of negative values in required_car_parking_spaces:0
    Number of negative values in total_of_special_requests:0
:::
:::

::: {.cell .markdown id="jF7l-T6irZAS"}
Data Quality Issues:

children: Some missing values.

company: High proportion of missing values.

agent: High proportion of missing values.

Most other columns are complete with no missing data.

adr (Average Daily Rate):

Extreme high values (\> 5000).

Negative values exist (impossible in reality).

lead_time:

Outliers above 2000 days (5+ years before booking, unrealistic).

days_in_waiting_list:

A few extreme cases (\> 300 days).

Other columns:

Minor outliers in children, babies, and stays_in_week_nights.

Data Imbalance

Columns such as company and agent are heavily skewed toward missing
values or dominated by a single category.
:::

::: {.cell .markdown id="0pNgYovXrywP"}
# Phase 2: Data Cleaning (The Core of the Project)
:::

::: {.cell .markdown id="Uox9cBiWrr4H"}
## Handle Missing Values:
:::

::: {.cell .code execution_count="12" id="fKXBCTbarIcG"}
``` python
df['company']=df['company'].fillna(0)
df['agent']=df['agent'].fillna(0)
```
:::

::: {.cell .code execution_count="13" id="yan1haomn-35"}
``` python
df['country']=df['country'].fillna(df['country'].mode()[0])
```
:::

::: {.cell .code execution_count="14" id="fEXSLl8otC0i"}
``` python
df['children']=df['children'].fillna(df['children'].median())
```
:::

::: {.cell .code execution_count="15" colab="{\"base_uri\":\"https://localhost:8080/\"}" id="F46mjDwzSDdZ" outputId="46c91f25-5c52-492e-bdc3-cf20a6831081"}
``` python
missing_values=df.isnull().sum()
missing_values_percentage=(df.isnull().sum()/len(df))*100
missing_values_df=pd.DataFrame({'Missing Values':missing_values,'Missing Values Percentage':missing_values_percentage})
print(missing_values_df)
print('--'*50)
#percentage of missing data

total_cells=np.prod(df.shape)
total_missing=missing_values_df['Missing Values'].sum()
missing_data_percentage=(total_missing/total_cells)*100
print(f'percentage of missing data:{missing_data_percentage}')
```

::: {.output .stream .stdout}
                                    Missing Values  Missing Values Percentage
    hotel                                        0                        0.0
    is_canceled                                  0                        0.0
    lead_time                                    0                        0.0
    arrival_date_year                            0                        0.0
    arrival_date_month                           0                        0.0
    arrival_date_week_number                     0                        0.0
    arrival_date_day_of_month                    0                        0.0
    stays_in_weekend_nights                      0                        0.0
    stays_in_week_nights                         0                        0.0
    adults                                       0                        0.0
    children                                     0                        0.0
    babies                                       0                        0.0
    meal                                         0                        0.0
    country                                      0                        0.0
    market_segment                               0                        0.0
    distribution_channel                         0                        0.0
    is_repeated_guest                            0                        0.0
    previous_cancellations                       0                        0.0
    previous_bookings_not_canceled               0                        0.0
    reserved_room_type                           0                        0.0
    assigned_room_type                           0                        0.0
    booking_changes                              0                        0.0
    deposit_type                                 0                        0.0
    agent                                        0                        0.0
    company                                      0                        0.0
    days_in_waiting_list                         0                        0.0
    customer_type                                0                        0.0
    adr                                          0                        0.0
    required_car_parking_spaces                  0                        0.0
    total_of_special_requests                    0                        0.0
    reservation_status                           0                        0.0
    reservation_status_date                      0                        0.0
    ----------------------------------------------------------------------------------------------------
    percentage of missing data:0.0
:::
:::

::: {.cell .markdown id="2_E3jB9trwuG"}
## Remove Duplicates:
:::

::: {.cell .code execution_count="16" colab="{\"base_uri\":\"https://localhost:8080/\"}" id="pE8vG6y9tI1h" outputId="15a358c9-6942-4377-f434-bdc3a25b7224"}
``` python
df.duplicated().sum()
```

::: {.output .execute_result execution_count="16"}
    np.int64(32013)
:::
:::

::: {.cell .code execution_count="17" colab="{\"base_uri\":\"https://localhost:8080/\",\"height\":617}" id="p3rsWBZAtNgW" outputId="0e7bb4d2-7c05-4c5f-a818-ce91153982fb"}
``` python
df[df.duplicated()]
```

::: {.output .execute_result execution_count="17"}
``` json
{"type":"dataframe"}
```
:::
:::

::: {.cell .code execution_count="18" id="zpvGT87yta4r"}
``` python
df.drop_duplicates(inplace=True)
```
:::

::: {.cell .code execution_count="19" colab="{\"base_uri\":\"https://localhost:8080/\"}" id="ac9kRoFSvXMV" outputId="68e1066a-34d2-499f-e65c-c0dd9310d98a"}
``` python
df.duplicated().sum()
```

::: {.output .execute_result execution_count="19"}
    np.int64(0)
:::
:::

::: {.cell .code execution_count="20" colab="{\"base_uri\":\"https://localhost:8080/\"}" id="VKFXIFR8vg6H" outputId="8a78e37a-314c-49e4-c542-787b6d2d6ba6"}
``` python
df.info()
```

::: {.output .stream .stdout}
    <class 'pandas.core.frame.DataFrame'>
    Index: 87377 entries, 0 to 119389
    Data columns (total 32 columns):
     #   Column                          Non-Null Count  Dtype  
    ---  ------                          --------------  -----  
     0   hotel                           87377 non-null  object 
     1   is_canceled                     87377 non-null  int64  
     2   lead_time                       87377 non-null  int64  
     3   arrival_date_year               87377 non-null  int64  
     4   arrival_date_month              87377 non-null  object 
     5   arrival_date_week_number        87377 non-null  int64  
     6   arrival_date_day_of_month       87377 non-null  int64  
     7   stays_in_weekend_nights         87377 non-null  int64  
     8   stays_in_week_nights            87377 non-null  int64  
     9   adults                          87377 non-null  int64  
     10  children                        87377 non-null  float64
     11  babies                          87377 non-null  int64  
     12  meal                            87377 non-null  object 
     13  country                         87377 non-null  object 
     14  market_segment                  87377 non-null  object 
     15  distribution_channel            87377 non-null  object 
     16  is_repeated_guest               87377 non-null  int64  
     17  previous_cancellations          87377 non-null  int64  
     18  previous_bookings_not_canceled  87377 non-null  int64  
     19  reserved_room_type              87377 non-null  object 
     20  assigned_room_type              87377 non-null  object 
     21  booking_changes                 87377 non-null  int64  
     22  deposit_type                    87377 non-null  object 
     23  agent                           87377 non-null  float64
     24  company                         87377 non-null  float64
     25  days_in_waiting_list            87377 non-null  int64  
     26  customer_type                   87377 non-null  object 
     27  adr                             87377 non-null  float64
     28  required_car_parking_spaces     87377 non-null  int64  
     29  total_of_special_requests       87377 non-null  int64  
     30  reservation_status              87377 non-null  object 
     31  reservation_status_date         87377 non-null  object 
    dtypes: float64(4), int64(16), object(12)
    memory usage: 22.0+ MB
:::
:::

::: {.cell .markdown id="xn-yL_Mwr2N3"}
## Handle Outliers:
:::

::: {.cell .code execution_count="21" id="JsI3V7eMvjQU"}
``` python
def cap_outliers_iqr(df, column):
    Q1 = df[column].quantile(0.25)
    Q3 = df[column].quantile(0.75)
    IQR = Q3 - Q1

    lower_bound = Q1 - 1.5 * IQR
    upper_bound = Q3 + 1.5 * IQR

    df[column] = df[column].clip(lower=lower_bound, upper=upper_bound)

    print(f"{column}: values capped between {lower_bound:.2f} and {upper_bound:.2f}")
    return df
```
:::

::: {.cell .code execution_count="22" colab="{\"base_uri\":\"https://localhost:8080/\"}" id="FClSeYiya322" outputId="0f4a76c6-8c39-427c-b53c-112fadd89d61"}
``` python
for column in df.select_dtypes(include=['int64','float64']):
  cap_outliers_iqr(df,column)
```

::: {.output .stream .stdout}
    is_canceled: values capped between -1.50 and 2.50
    lead_time: values capped between -160.00 and 296.00
    arrival_date_year: values capped between 2014.50 and 2018.50
    arrival_date_week_number: values capped between -15.50 and 68.50
    arrival_date_day_of_month: values capped between -14.50 and 45.50
    stays_in_weekend_nights: values capped between -3.00 and 5.00
    stays_in_week_nights: values capped between -3.50 and 8.50
    adults: values capped between 2.00 and 2.00
    children: values capped between 0.00 and 0.00
    babies: values capped between 0.00 and 0.00
    is_repeated_guest: values capped between 0.00 and 0.00
    previous_cancellations: values capped between 0.00 and 0.00
    previous_bookings_not_canceled: values capped between 0.00 and 0.00
    booking_changes: values capped between 0.00 and 0.00
    agent: values capped between -328.50 and 571.50
    company: values capped between 0.00 and 0.00
    days_in_waiting_list: values capped between 0.00 and 0.00
    adr: values capped between -21.00 and 227.00
    required_car_parking_spaces: values capped between 0.00 and 0.00
    total_of_special_requests: values capped between -1.50 and 2.50
:::
:::

::: {.cell .code execution_count="23" colab="{\"base_uri\":\"https://localhost:8080/\",\"height\":913}" id="jYy7uyMSbbLv" outputId="0b47e36d-453a-45ef-8712-545210f2ca06"}
``` python
plt.figure(figsize=(10,8))
sns.boxplot(data=df.select_dtypes(include=['int64','float64']))
plt.xticks(rotation=90)   # rotate x-axis labels
plt.grid(True)
plt.title("Boxplots of All Numerical Columns after cap")
plt.show()
```

::: {.output .display_data}
![](3269ddb0fedadba2162e44b23e5a2fbde3d1feb2.png)
:::
:::

::: {.cell .markdown id="LJjt0k8Er6S4"}
## Fix Data Types:
:::

::: {.cell .code execution_count="24" colab="{\"base_uri\":\"https://localhost:8080/\"}" id="gig0lUy8mpYp" outputId="ea12bada-ca88-4bad-e50f-3aa6e5d26f2e"}
``` python
print(df['arrival_date_month'].unique())
```

::: {.output .stream .stdout}
    ['July' 'August' 'September' 'October' 'November' 'December' 'January'
     'February' 'March' 'April' 'May' 'June']
:::
:::

::: {.cell .code execution_count="25" id="fboyfhXXd4Uo"}
``` python
df['reservation_status_date']=pd.to_datetime(df['reservation_status_date'])

df['arrival_month_num'] = pd.to_datetime(
    df['arrival_date_month'], format='%B', errors='coerce'
).dt.month

# Build the full arrival_date
df['arrival_date'] = pd.to_datetime(
    dict(
        year=df['arrival_date_year'],
        month=df['arrival_month_num'],
        day=df['arrival_date_day_of_month']
    ),
    errors='coerce'
)

df.drop(columns=['arrival_month_num'], inplace=True)
```
:::

::: {.cell .code execution_count="26" colab="{\"base_uri\":\"https://localhost:8080/\"}" id="CJB3rULEjK6P" outputId="2413dbd1-2582-418a-c627-fbdab98586d6"}
``` python
print(df[['arrival_date_year','arrival_date_month','arrival_date_day_of_month','arrival_date']].head())
```

::: {.output .stream .stdout}
       arrival_date_year arrival_date_month  arrival_date_day_of_month  \
    0               2015               July                          1   
    1               2015               July                          1   
    2               2015               July                          1   
    3               2015               July                          1   
    4               2015               July                          1   

      arrival_date  
    0   2015-07-01  
    1   2015-07-01  
    2   2015-07-01  
    3   2015-07-01  
    4   2015-07-01  
:::
:::

::: {.cell .code execution_count="27" colab="{\"base_uri\":\"https://localhost:8080/\"}" id="2_OB4luVkJDO" outputId="7a8e2119-1561-49fc-e39b-b44daab4db8f"}
``` python
print(df[['arrival_date','reservation_status_date']].dtypes)
```

::: {.output .stream .stdout}
    arrival_date               datetime64[ns]
    reservation_status_date    datetime64[ns]
    dtype: object
:::
:::

::: {.cell .code execution_count="28" colab="{\"base_uri\":\"https://localhost:8080/\"}" id="unbeu2GinOpD" outputId="78161746-1678-414b-ee5b-a2ecd7d9b425"}
``` python
print("Arrival:", df['arrival_date'].min(), "→", df['arrival_date'].max())
print("Reservation status:", df['reservation_status_date'].min(), "→", df['reservation_status_date'].max())
```

::: {.output .stream .stdout}
    Arrival: 2015-07-01 00:00:00 → 2017-08-31 00:00:00
    Reservation status: 2014-10-17 00:00:00 → 2017-09-14 00:00:00
:::
:::

::: {.cell .markdown id="L23pf_MIoFbt"}
arrival_date

Earliest: 2015-07-01

Latest: 2017-08-31\
means this datadet is covering 2 years

reservation_status_date

Earliest: 2014-10-17

Latest: 2017-09-14

Reservation activity was recorded a bit earlier (starting October 2014)
and continues slightly beyond the arrivals. This makes sense: people
book earlier than arrival, and cancellations/no-shows can be recorded
after the planned arrival.
:::

::: {.cell .markdown id="Ph82fovwpXk-"}
# Phase 3: Feature Engineering & Preprocessing {#phase-3-feature-engineering--preprocessing}
:::

::: {.cell .markdown id="N2JkzVP0rmqv"}
## Create New Features:
:::

::: {.cell .code execution_count="29" id="vbLDNyY4nW5E"}
``` python
df['total_guests']=df['adults']+df['children']+df['babies']
df['total_nights']=df['stays_in_weekend_nights']+df['stays_in_week_nights']
df['is_family']=((df['babies']>0)|(df['children']>0)).astype(int)
```
:::

::: {.cell .code execution_count="30" colab="{\"base_uri\":\"https://localhost:8080/\",\"height\":342}" id="beWApzp2rWIv" outputId="409f3fed-8ede-40f8-e3ae-5083e97e4ec2"}
``` python
df.head()
```

::: {.output .execute_result execution_count="30"}
``` json
{"type":"dataframe","variable_name":"df"}
```
:::
:::

::: {.cell .markdown id="uguovv0ir-wo"}
## Encode Categorical Variables:
:::

::: {.cell .code execution_count="31" colab="{\"base_uri\":\"https://localhost:8080/\"}" id="P3sNi-17rZfC" outputId="ce22728e-a843-48eb-839f-f8d19fe5c05c"}
``` python
cat_cols = df.select_dtypes(include=["object"]).columns
for col in cat_cols:
    print(col, ":", df[col].nunique())
```

::: {.output .stream .stdout}
    hotel : 2
    arrival_date_month : 12
    meal : 5
    country : 177
    market_segment : 8
    distribution_channel : 5
    reserved_room_type : 10
    assigned_room_type : 12
    deposit_type : 3
    customer_type : 4
    reservation_status : 3
:::
:::

::: {.cell .code execution_count="32" id="5gTDcNo-tU7V"}
``` python
low_card_cols = [
    "hotel", "arrival_date_month", "meal", "market_segment",
    "distribution_channel", "reserved_room_type", "assigned_room_type",
    "deposit_type", "customer_type"
]

high_card_cols = ["country"]
```
:::

::: {.cell .code execution_count="33" id="UCDVHa5JtUvb"}
``` python
df = pd.get_dummies(df, columns=low_card_cols, drop_first=False, dtype="int8")
```
:::

::: {.cell .code execution_count="34" id="jXNXahDOtUjr"}
``` python
for col in high_card_cols:
    freqs = df[col].value_counts(normalize=True)
    df[col + "_freq"] = df[col].map(freqs).fillna(0.0)
    df = df.drop(columns=[col])
```
:::

::: {.cell .code execution_count="35" colab="{\"base_uri\":\"https://localhost:8080/\",\"height\":273}" id="HWrD7PnluImy" outputId="343baf7d-ffdf-4a50-9caa-a2610be34662"}
``` python
df.head()
```

::: {.output .execute_result execution_count="35"}
``` json
{"type":"dataframe","variable_name":"df"}
```
:::
:::

::: {.cell .markdown id="ZjxrHn5GsmQk"}
## CRITICAL STEP:
:::

::: {.cell .code execution_count="36" id="iQrpTCarsov5"}
``` python
df.drop(columns=['reservation_status','reservation_status_date'],inplace=True)
```
:::

::: {.cell .markdown id="-mUvgyOts8k-"}
## Final Preparation:
:::

::: {.cell .code execution_count="37" id="SYl9_dors5XQ"}
``` python
from sklearn.model_selection import train_test_split
```
:::

::: {.cell .code execution_count="38" id="QyGZOZQ2tCT2"}
``` python
x=df.drop(columns=['is_canceled'])
y=df['is_canceled']

x_train,x_test,y_train,y_test=train_test_split(x,y,test_size=0.2,random_state=42)
```
:::

::: {.cell .code execution_count="38" id="8v4IcUtMsoFJ"}
``` python
```
:::
