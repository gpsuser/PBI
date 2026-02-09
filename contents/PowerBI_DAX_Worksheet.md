# Power BI DAX Worksheet for Beginners

## Introduction

Welcome to this hands-on Power BI DAX worksheet! This exercise set will help you learn the fundamentals of DAX (Data Analysis Expressions) using a real sales dataset.

**Dataset**: `sales_data.csv`

**Columns**:
- Sales ID
- Date Time (format: DD/MM/YYYY HH:MM)
- Items
- Total Spend
- Product (Product A, B, C, D, E)
- City (City 1-10)
- Customer

**Setup Instructions**:
1. Open Power BI Desktop
2. Import the `sales_data.csv` file
3. Create a separate `weight` table using "Enter Data" with the following:

| Product | Weight_grams |
|---------|--------------|
| Product A | 115 |
| Product B | 220 |
| Product C | 3000 |
| Product D | 410 |
| Product E | 525 |

4. Create a relationship between `sales_data[Product]` and `weight[Product]`

---

## Exercise 1: Basic Calculated Columns

**Objective**: Learn to create calculated columns using basic arithmetic operations.

### Task 1.1: Create a Unit Price Column
Calculate the unit price for each transaction by dividing Total Spend by Items.

**Column Name**: `Unit_Price`

**Your DAX Code**:
```dax
Unit_Price = 


```

**Expected Result**: A new column showing the price per item for each sale.

---

### Task 1.2: Create a Date Column in YYYYMMDD Format
Convert the Date Time column to a numeric date format (e.g., 01/02/2023 becomes 20230201).

**Column Name**: `Date_Key`

**Your DAX Code**:
```dax
Date_Key = 


```

**Expected Result**: A column with dates as integers (e.g., 20230201, 20230318).

---

## Exercise 2: Text and Date Functions

**Objective**: Work with date functions and text formatting.

### Task 2.1: Extract Month Name
Create a column that shows the month name from the Date Time column.

**Column Name**: `Month_Name`

**Your DAX Code**:
```dax
Month_Name = 


```

**Expected Result**: Month names like "January", "February", "March", etc.

---

### Task 2.2: Extract Year
Create a column that extracts just the year from the Date Time column.

**Column Name**: `Year`

**Your DAX Code**:
```dax
Year = 


```

**Expected Result**: Years like 2023.

---

## Exercise 3: Conditional Logic

**Objective**: Use conditional functions to categorize data.

### Task 3.1: Create Revenue Tier Column
Categorize each sale as:
- "Low" if Total Spend < $2,000
- "Medium" if Total Spend between $2,000 and $6,000
- "High" if Total Spend > $6,000

**Column Name**: `Revenue_Tier`

**Your DAX Code**:
```dax
Revenue_Tier = 


```

**Expected Result**: Each sale categorized as Low, Medium, or High.

---

### Task 3.2: Create Weekend Flag
Create a column that shows "Weekend" if the sale occurred on Saturday or Sunday, otherwise "Weekday".

**Column Name**: `Day_Type`

**Your DAX Code**:
```dax
Day_Type = 


```

**Expected Result**: "Weekend" or "Weekday" for each transaction.

---

## Exercise 4: Aggregation with Related Tables

**Objective**: Use the RELATED function to bring data from another table.

### Task 4.1: Add Weight to Sales Data
Create a calculated column that brings the weight value from the weight table.

**Column Name**: `Product_Weight`

**Your DAX Code**:
```dax
Product_Weight = 


```

**Expected Result**: Weight in grams for each product in the sales data.

---

### Task 4.2: Calculate Total Weight per Transaction
Calculate the total weight for each transaction by multiplying Product Weight by Items sold.

**Column Name**: `Total_Weight_Grams`

**Your DAX Code**:
```dax
Total_Weight_Grams = 


```

**Expected Result**: Total weight in grams for each sale.

---

## Exercise 5: Measures (Basic Aggregations)

**Objective**: Create measures that aggregate data across the entire dataset or filtered context.

### Task 5.1: Total Revenue Measure
Create a measure that calculates the total revenue across all sales.

**Measure Name**: `Total_Revenue`

**Your DAX Code**:
```dax
Total_Revenue = 


```

**Expected Result**: A measure showing the sum of all sales (~$491,000).

---

### Task 5.2: Average Transaction Value
Create a measure that calculates the average transaction value.

**Measure Name**: `Avg_Transaction_Value`

**Your DAX Code**:
```dax
Avg_Transaction_Value = 


```

**Expected Result**: Average spend per transaction.

---

### Task 5.3: Total Items Sold
Create a measure that sums all items sold across all transactions.

**Measure Name**: `Total_Items_Sold`

**Your DAX Code**:
```dax
Total_Items_Sold = 


```

**Expected Result**: Total count of items sold.

---

### Task 5.4: Number of Transactions
Create a measure that counts the number of unique transactions.

**Measure Name**: `Transaction_Count`

**Your DAX Code**:
```dax
Transaction_Count = 


```

**Expected Result**: Count of sales (100).

---

### Task 5.5: Total Weight Shipped (Measure)
Create a measure that calculates the total weight shipped across all sales.

**Measure Name**: `Total_Weight_Shipped`

**Your DAX Code**:
```dax
Total_Weight_Shipped = 


```

**Expected Result**: Total weight in grams.

---

## Bonus Challenge: Advanced Measures

### Task 6.1: High Value Sales Count
Create a measure that counts sales with Total Spend greater than $5,000.

**Measure Name**: `High_Value_Sales_Count`

**Your DAX Code**:
```dax
High_Value_Sales_Count = 


```

---

### Task 6.2: Revenue from Product A
Create a measure that calculates total revenue only from Product A.

**Measure Name**: `Product_A_Revenue`

**Your DAX Code**:
```dax
Product_A_Revenue = 


```

---

### Task 6.3: Average Items per Sale by City
Create a measure that shows the average number of items per transaction, which can be broken down by City in a visual.

**Measure Name**: `Avg_Items_Per_City`

**Your DAX Code**:
```dax
Avg_Items_Per_City = 


```

---

## Testing Your Work

After completing the exercises:

1. Create a simple table visual showing your calculated columns
2. Create a card visual for each measure to verify the results
3. Test your measures with slicers (City, Product, Month) to see how they respond to filters
4. Use DAX Studio to run test queries (optional)

---

## Learning Checklist

By completing this worksheet, you should now understand:

- ✓ How to create calculated columns vs. measures
- ✓ Basic arithmetic operations in DAX
- ✓ Date and time functions (FORMAT, YEAR, MONTH, etc.)
- ✓ Conditional logic (IF, SWITCH)
- ✓ The RELATED function for related tables
- ✓ Basic aggregation functions (SUM, AVERAGE, COUNT)
- ✓ Filter context and CALCULATE function

---

**Next Steps**: 
- Review the solutions file to check your answers
- Refer to the DAX Cheat Sheet for quick syntax reference
- Practice creating additional calculated columns and measures
- Explore more advanced DAX functions like FILTER, ALL, and time intelligence
