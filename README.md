# RFM Analysis: Understanding Customer Behavior

## Overview
RFM Analysis is a widely used technique in the marketing domain to segment customers based on their buying behavior. This approach helps me assess customers' engagement, loyalty, and value by analyzing:

- **Recency**: The date of their last purchase.
- **Frequency**: How often they make purchases.
- **Monetary Value**: The total amount spent on purchases.

By calculating these metrics, I can gain valuable insights into customer patterns and behaviors.

## Dataset Requirements
To perform RFM Analysis using Python, I use a [dataset](https://statso.io/rfm-analysis-case-study/) containing:
- **Customer IDs**
- **Purchase Dates**
- **Transaction Amounts**

With this information, I can compute RFM values for each customer and analyze their behavior.

## Steps Involved in RFM Analysis
### 1. Importing Necessary Libraries and Dataset
```python
import pandas as pd
import numpy as np
from datetime import datetime
```

### 2. Calculating RFM Values
#### Recency Calculation
I calculate recency by subtracting the last purchase date from the current date:
```python
rfm['Recency'] = (datetime.now().date() - rfm['LastPurchaseDate']).dt.days
```

#### Frequency Calculation
Frequency is determined by counting the number of purchases made by each customer:
```python
rfm['Frequency'] = rfm.groupby('CustomerID')['OrderID'].nunique()
```

#### Monetary Value Calculation
Monetary value is calculated by summing up the total amount spent by each customer:
```python
rfm['Monetary'] = rfm.groupby('CustomerID')['TransactionAmount'].sum()
```

### 3. Resulting Data
![RFM Value Segment](https://github.com/Sourabh1710/RFM-Analysis/blob/main/images/RFM%20Value%20Segment%20Distribution.png)

## Calculating RFM Scores
I assign scores to each RFM metric:
- **Recency Score**: Higher scores for more recent purchases (5 = most recent, 1 = least recent)
- **Frequency Score**: Higher scores for more frequent purchases (5 = most frequent, 1 = least frequent)
- **Monetary Score**: Higher scores for higher spending (5 = highest spending, 1 = lowest spending)

```python
rfm['R_Score'] = pd.qcut(rfm['Recency'], 5, labels=[5, 4, 3, 2, 1])
rfm['F_Score'] = pd.qcut(rfm['Frequency'], 5, labels=[1, 2, 3, 4, 5])
rfm['M_Score'] = pd.qcut(rfm['Monetary'], 5, labels=[1, 2, 3, 4, 5])
```

### Data Overview After Assigning Scores
![RFM Customer Segments by Value](https://github.com/Sourabh1710/RFM-Analysis/blob/main/images/RFM%20Customer%20Segments%20by%20Value.png)

## RFM Value Segmentation
I calculate the final **RFM Score**:
```python
rfm['RFM_Score'] = rfm['R_Score'].astype(int) + rfm['F_Score'].astype(int) + rfm['M_Score'].astype(int)
```

Customers are segmented into three groups:
- **Low-Value Customers**
- **Mid-Value Customers**
- **High-Value Customers**

Segmentation is performed using `pd.qcut()`:
```python
rfm['Segment'] = pd.qcut(rfm['RFM_Score'], 3, labels=['Low-Value', 'Mid-Value', 'High-Value'])
```

### Segment Distribution
![Distribution of RFM Values within Champions Segment](https://github.com/Sourabh1710/RFM-Analysis/blob/main/images/Distribution%20of%20RFM%20Values%20within%20Champions%20Segment.png)

## RFM Customer Segments
Beyond value-based segmentation, I define **customer behavioral segments** such as:
- **Champions**
- **Potential Loyalists**
- **Can’t Lose Customers**

```python
def segment_customers(df):
    if df['R_Score'] >= 4 and df['F_Score'] >= 4:
        return 'Champions'
    elif df['R_Score'] >= 3 and df['F_Score'] >= 3:
        return 'Potential Loyalists'
    elif df['R_Score'] == 1 and df['F_Score'] <= 2:
        return "Can't Lose"
    else:
        return 'Others'

rfm['Customer_Segment'] = rfm.apply(segment_customers, axis=1)
```

### RFM Customer Segments Distribution
![](https://github.com/Sourabh1710/RFM-Analysis/blob/main/images/RFM%20Customer%20Segments%20by%20Value.png)

### Distribution of RFM Values in Champions Segment
![](https://github.com/Sourabh1710/RFM-Analysis/blob/main/images/Distribution%20of%20RFM%20Values%20within%20Champions%20Segment.png)

### Correlation of RFM Scores within Champions Segment
![](https://github.com/Sourabh1710/RFM-Analysis/blob/main/images/Correlation%20Matrix%20of%20RFM%20Values%20within%20Champions%20Segment.png)

### Number of Customers in Each Segment
![Comparison of RFM Segments](https://github.com/Sourabh1710/RFM-Analysis/blob/main/images/Comparison%20of%20RFM%20Segments.png)

### Recency, Frequency, and Monetary Scores Across Segments
![Comparison of RFM Segments based on Recency, Frequency, and Monetary Scores](https://github.com/Sourabh1710/RFM-Analysis/blob/main/images/Comparison%20of%20RFM%20Segments%20based%20on%20Recency%2C%20Frequency%2C%20and%20Monetary%20Scores.png)

## Summary
RFM Analysis is a powerful method to:
- Understand and segment customers based on their purchasing behavior.
- Identify high-value customers for targeted marketing.
- Improve customer retention strategies.

By implementing RFM Analysis, I can make data-driven decisions to enhance customer relationships and drive revenue growth.


## Author
Sourabh Sonker <br>
Data Scientist

