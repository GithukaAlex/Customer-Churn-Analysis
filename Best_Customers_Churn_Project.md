```python
import pandas as pd
import datetime as dt
```


```python
df = pd.read_csv(r"C:\Users\BUSINESS ANALYST\Downloads\Retail_Data_Transactions.csv")

# data = pd.read_csv("rfm_xmas19.txt", parse_dates=["trans_date"])
```


```python
df['trans_date'].max()
```




    '31-Oct-14'




```python
# Convert the date time
df['trans_date'] = pd.to_datetime(df['trans_date'],format='%Y-%m-%d')
df.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>customer_id</th>
      <th>trans_date</th>
      <th>tran_amount</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>CS5295</td>
      <td>2013-02-11</td>
      <td>35</td>
    </tr>
    <tr>
      <th>1</th>
      <td>CS4768</td>
      <td>2015-03-15</td>
      <td>39</td>
    </tr>
    <tr>
      <th>2</th>
      <td>CS2122</td>
      <td>2013-02-26</td>
      <td>52</td>
    </tr>
    <tr>
      <th>3</th>
      <td>CS1217</td>
      <td>2011-11-16</td>
      <td>99</td>
    </tr>
    <tr>
      <th>4</th>
      <td>CS1850</td>
      <td>2013-11-20</td>
      <td>78</td>
    </tr>
  </tbody>
</table>
</div>




```python
groupby_customer_id = df.groupby('customer_id')
```


```python
# return customers most recent buy date
last_trans = groupby_customer_id['trans_date'].max()
last_trans.sample(10)

# convert the result into a dataframe

df1 = pd.DataFrame(last_trans) 
df1.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>trans_date</th>
    </tr>
    <tr>
      <th>customer_id</th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>CS1112</th>
      <td>2015-01-14</td>
    </tr>
    <tr>
      <th>CS1113</th>
      <td>2015-02-09</td>
    </tr>
    <tr>
      <th>CS1114</th>
      <td>2015-02-12</td>
    </tr>
    <tr>
      <th>CS1115</th>
      <td>2015-03-05</td>
    </tr>
    <tr>
      <th>CS1116</th>
      <td>2014-08-25</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Create a new column that returns 2024 if last buy is in 2024 and not 2024 if not

cut_off = dt.datetime(2015,1,1)

df1['churned'] = df1['trans_date'].apply(lambda x: 1 if x < cut_off else 0)
df1.sample(10)
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>trans_date</th>
      <th>churned</th>
    </tr>
    <tr>
      <th>customer_id</th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>CS2515</th>
      <td>2015-02-16</td>
      <td>0</td>
    </tr>
    <tr>
      <th>CS3246</th>
      <td>2015-02-25</td>
      <td>0</td>
    </tr>
    <tr>
      <th>CS1916</th>
      <td>2015-03-08</td>
      <td>0</td>
    </tr>
    <tr>
      <th>CS2092</th>
      <td>2015-01-28</td>
      <td>0</td>
    </tr>
    <tr>
      <th>CS2815</th>
      <td>2015-02-08</td>
      <td>0</td>
    </tr>
    <tr>
      <th>CS7440</th>
      <td>2014-12-05</td>
      <td>1</td>
    </tr>
    <tr>
      <th>CS3361</th>
      <td>2014-10-17</td>
      <td>1</td>
    </tr>
    <tr>
      <th>CS3585</th>
      <td>2015-02-08</td>
      <td>0</td>
    </tr>
    <tr>
      <th>CS5410</th>
      <td>2014-12-16</td>
      <td>1</td>
    </tr>
    <tr>
      <th>CS1391</th>
      <td>2015-03-12</td>
      <td>0</td>
    </tr>
  </tbody>
</table>
</div>




```python
df1['num_of_transactions'] = groupby_customer_id.size()
df1.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>trans_date</th>
      <th>churned</th>
      <th>num_of_transactions</th>
    </tr>
    <tr>
      <th>customer_id</th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>CS1112</th>
      <td>2015-01-14</td>
      <td>0</td>
      <td>15</td>
    </tr>
    <tr>
      <th>CS1113</th>
      <td>2015-02-09</td>
      <td>0</td>
      <td>20</td>
    </tr>
    <tr>
      <th>CS1114</th>
      <td>2015-02-12</td>
      <td>0</td>
      <td>19</td>
    </tr>
    <tr>
      <th>CS1115</th>
      <td>2015-03-05</td>
      <td>0</td>
      <td>22</td>
    </tr>
    <tr>
      <th>CS1116</th>
      <td>2014-08-25</td>
      <td>1</td>
      <td>13</td>
    </tr>
  </tbody>
</table>
</div>




```python
df1['sales_Amt'] = groupby_customer_id.sum('tran_amount')
df1.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>trans_date</th>
      <th>churned</th>
      <th>num_of_transactions</th>
      <th>sales_Amt</th>
    </tr>
    <tr>
      <th>customer_id</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>CS1112</th>
      <td>2015-01-14</td>
      <td>0</td>
      <td>15</td>
      <td>1012</td>
    </tr>
    <tr>
      <th>CS1113</th>
      <td>2015-02-09</td>
      <td>0</td>
      <td>20</td>
      <td>1490</td>
    </tr>
    <tr>
      <th>CS1114</th>
      <td>2015-02-12</td>
      <td>0</td>
      <td>19</td>
      <td>1432</td>
    </tr>
    <tr>
      <th>CS1115</th>
      <td>2015-03-05</td>
      <td>0</td>
      <td>22</td>
      <td>1659</td>
    </tr>
    <tr>
      <th>CS1116</th>
      <td>2014-08-25</td>
      <td>1</td>
      <td>13</td>
      <td>857</td>
    </tr>
  </tbody>
</table>
</div>




```python
# to find the best customer in the churn
    # we use the criteria of (0.5* num_of_transactioons) + (0.5 * trans_amount)

# To emsure that the criteria is fair, we use the Min-Max feature scaling   

df1[['num_of_transactions','sales_Amt']].describe().loc[['min','max']]
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>num_of_transactions</th>
      <th>sales_Amt</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>min</th>
      <td>4.0</td>
      <td>149.0</td>
    </tr>
    <tr>
      <th>max</th>
      <td>39.0</td>
      <td>2933.0</td>
    </tr>
  </tbody>
</table>
</div>



## The dist is skewwed and so we need to normalize it
![image.png](attachment:image.png)



```python
df1["scaled_tran"] = (df1["num_of_transactions"] - df1["num_of_transactions"].min()) \
                             / \
                    (df1["num_of_transactions"].max()  - df1["num_of_transactions"].min())

df1["scaled_amount"] = (df1["sales_Amt"] -df1["sales_Amt"].min()) \
                               / \
                       (df1["sales_Amt"].max() - df1["sales_Amt"].min())

df1["score"] = 100*(.5*df1["scaled_tran"] + .5*df1["scaled_amount"])

df1.sort_values("score", inplace=True, ascending=False)
```


```python
df1.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>trans_date</th>
      <th>churned</th>
      <th>num_of_transactions</th>
      <th>sales_Amt</th>
      <th>scaled_tran</th>
      <th>scaled_amount</th>
      <th>score</th>
    </tr>
    <tr>
      <th>customer_id</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>CS4424</th>
      <td>2015-01-19</td>
      <td>0</td>
      <td>39</td>
      <td>2933</td>
      <td>1.000000</td>
      <td>1.000000</td>
      <td>100.000000</td>
    </tr>
    <tr>
      <th>CS4320</th>
      <td>2014-12-26</td>
      <td>1</td>
      <td>38</td>
      <td>2647</td>
      <td>0.971429</td>
      <td>0.897270</td>
      <td>93.434934</td>
    </tr>
    <tr>
      <th>CS3799</th>
      <td>2014-10-16</td>
      <td>1</td>
      <td>36</td>
      <td>2513</td>
      <td>0.914286</td>
      <td>0.849138</td>
      <td>88.171182</td>
    </tr>
    <tr>
      <th>CS5109</th>
      <td>2015-02-04</td>
      <td>0</td>
      <td>35</td>
      <td>2506</td>
      <td>0.885714</td>
      <td>0.846624</td>
      <td>86.616892</td>
    </tr>
    <tr>
      <th>CS3805</th>
      <td>2014-12-11</td>
      <td>1</td>
      <td>35</td>
      <td>2453</td>
      <td>0.885714</td>
      <td>0.827586</td>
      <td>85.665025</td>
    </tr>
  </tbody>
</table>
</div>



![image.png](attachment:image.png)
![image-2.png](attachment:image-2.png)


```python
coupon = df['tran_amount'].mean()*0.3
num_customers_targeted = 1000/coupon
print(coupon)
print(num_customers_targeted)
```

    19.4975736
    51.28843314123969
    

![image.png](attachment:image.png)


```python
# List of the customers to target

top_50_best_churned_customers = df1[df1['churned']==1]
top_50_best_churned_customers.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>trans_date</th>
      <th>churned</th>
      <th>num_of_transactions</th>
      <th>sales_Amt</th>
      <th>scaled_tran</th>
      <th>scaled_amount</th>
      <th>score</th>
    </tr>
    <tr>
      <th>customer_id</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>CS4320</th>
      <td>2014-12-26</td>
      <td>1</td>
      <td>38</td>
      <td>2647</td>
      <td>0.971429</td>
      <td>0.897270</td>
      <td>93.434934</td>
    </tr>
    <tr>
      <th>CS3799</th>
      <td>2014-10-16</td>
      <td>1</td>
      <td>36</td>
      <td>2513</td>
      <td>0.914286</td>
      <td>0.849138</td>
      <td>88.171182</td>
    </tr>
    <tr>
      <th>CS3805</th>
      <td>2014-12-11</td>
      <td>1</td>
      <td>35</td>
      <td>2453</td>
      <td>0.885714</td>
      <td>0.827586</td>
      <td>85.665025</td>
    </tr>
    <tr>
      <th>CS5752</th>
      <td>2014-12-28</td>
      <td>1</td>
      <td>33</td>
      <td>2612</td>
      <td>0.828571</td>
      <td>0.884698</td>
      <td>85.663485</td>
    </tr>
    <tr>
      <th>CS4074</th>
      <td>2014-12-05</td>
      <td>1</td>
      <td>34</td>
      <td>2462</td>
      <td>0.857143</td>
      <td>0.830819</td>
      <td>84.398091</td>
    </tr>
  </tbody>
</table>
</div>




```python
# To download the file to TXT
# top_50_best_churned_customers.to_csv('target_customers.txt')

```


```python

```
