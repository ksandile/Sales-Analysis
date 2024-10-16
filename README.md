
# Sales and Marketing Data Analysis

This project involves an analysis of a sales and marketing dataset to understand various aspects of sales performance, customer behavior, and product popularity. The dataset provides insights into monthly sales trends, city-wise sales, and product-wise sales. The analysis was conducted to answer several key business questions and visualize data trends.

## Dataset Overview

The dataset includes the following key information:
- **Order Date**: Date when the order was placed.
- **Quantity Ordered**: Number of products ordered.
- **Price Each**: Price of each product ordered.
- **Order ID**: Unique identifier for each order.
- **City**: City where the order was placed (added during the analysis).
- **Product**: Product that was ordered.

## Project Workflow

### Step 1: Reading the Data and Initial Exploration
We start by reading the dataset and looking at the first five rows to get a sense of the data.

```python
import pandas as pd
df = pd.read_csv('Sales_Data.csv')
print(df.head())
```

### Step 2: Data Cleaning
We found some NaN values in the dataset, which we had to clean. Additionally, we identified and deleted rows containing unwanted values such as 'Or'.

```python
# Drop NaN values and remove rows with 'Or'
df = df.dropna()
df = df[df['Quantity Ordered'] != 'Or']
```

### Step 3: Adding Additional Columns
We added columns such as 'City' and 'Hour' to perform more detailed analysis. This involved splitting existing columns and converting data types where necessary.

```python
df['Order Date'] = pd.to_datetime(df['Order Date'])
df['City'] = df['Purchase Address'].apply(lambda x: x.split(',')[1])
df['Hour'] = df['Order Date'].dt.hour
```

### Step 4: Best Month for Sales
To determine the best month for sales, we aggregated sales data by month and calculated the total earnings for each month.

```python
df['Sales'] = df['Quantity Ordered'].astype('int') * df['Price Each'].astype('float')
monthly_sales = df.groupby(df['Order Date'].dt.month)['Sales'].sum()
print(monthly_sales)
```

### Step 5: Sales Visualization
We plotted a bar graph to visualize which month had the highest sales.

```python
import matplotlib.pyplot as plt
monthly_sales.plot(kind='bar')
plt.show()
```

### Step 6: Analysis of Sales by City
Next, we checked which city had the highest number of sales. We added a 'City' column and grouped sales data by city.

```python
city_sales = df.groupby('City')['Sales'].sum()
print(city_sales)
city_sales.plot(kind='bar')
plt.show()
```

### Step 7: Most Sold Products
We analyzed which products were most often sold together and which individual product had the highest quantity ordered.

```python
product_sales = df.groupby('Product')['Quantity Ordered'].sum()
product_sales.plot(kind='bar')
plt.show()
```

### Step 8: Price Analysis of Products
To understand why some products sold more than others, we plotted a graph showing product prices alongside their sales quantities.

```python
prices = df.groupby('Product')['Price Each'].mean()
fig, ax1 = plt.subplots()
ax2 = ax1.twinx()
ax1.bar(product_sales.index, product_sales.values, color='g')
ax2.plot(prices.index, prices.values, color='b')
plt.show()
```

## Conclusion
The analysis provided valuable insights into monthly sales trends, city-wise performance, and product popularity. The company can use this information to optimize marketing strategies, adjust pricing, and focus on high-demand products.

### Key Insights:
- The best month for sales was [insert month] with total earnings of [insert amount].
- San Francisco had the highest sales among all cities.
- iPhones and Lightning Charging Cables were the most sold products together.
- AAA batteries were the most sold individual product due to their low price.

## Future Work
- Investigate the impact of external factors (e.g., economic events, holidays) on sales trends.
- Explore customer demographics and their influence on sales.

### Technologies Used
- **Python**: For data cleaning, manipulation, and analysis.
- **Pandas**: For handling data in a structured format.
- **Matplotlib**: For data visualization.
- **Seaborn**: For advanced visualizations.

THE END
