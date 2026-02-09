# Power BI DAX Worksheet - Solutions

## Exercise 1: Basic Calculated Columns

### Task 1.1: Unit Price Column

```dax
Unit_Price = [Total Spend] / [Items]
```

**Explanation**: This divides the total spend by the number of items to get the price per item. Note: In calculated columns, we reference columns using `[ColumnName]` syntax.

---

### Task 1.2: Date Column in YYYYMMDD Format

```dax
Date_Key = VALUE(FORMAT([Date Time], "YYYYMMDD"))
```

**Explanation**: 
- `FORMAT([Date Time], "YYYYMMDD")` converts the date to a text string like "20230201"
- `VALUE()` converts the text string to a number (20230201)
- This is useful for creating date keys for data warehousing scenarios

---

## Exercise 2: Text and Date Functions

### Task 2.1: Extract Month Name

```dax
Month_Name = FORMAT([Date Time], "MMMM")
```

**Explanation**: The `"MMMM"` format string returns the full month name (e.g., "January", "February"). Use `"MMM"` for abbreviated names (e.g., "Jan", "Feb").

---

### Task 2.2: Extract Year

```dax
Year = YEAR([Date Time])
```

**Explanation**: The `YEAR()` function extracts the year as an integer from a date/time value.

**Alternative Solution**:
```dax
Year = FORMAT([Date Time], "YYYY")
```
This returns the year as text instead of a number.

---

## Exercise 3: Conditional Logic

### Task 3.1: Revenue Tier Column

```dax
Revenue_Tier = 
SWITCH(
    TRUE(),
    [Total Spend] < 2000, "Low",
    [Total Spend] <= 6000, "Medium",
    "High"
)
```

**Explanation**: 
- `SWITCH(TRUE(), ...)` is a pattern for multiple conditions
- Evaluates each condition in order and returns the first match
- The final value "High" is the default if no other condition matches

**Alternative Solution using IF**:
```dax
Revenue_Tier = 
IF(
    [Total Spend] < 2000, 
    "Low",
    IF([Total Spend] <= 6000, "Medium", "High")
)
```

---

### Task 3.2: Weekend Flag

```dax
Day_Type = 
IF(
    WEEKDAY([Date Time], 2) >= 6,
    "Weekend",
    "Weekday"
)
```

**Explanation**: 
- `WEEKDAY([Date Time], 2)` returns 1-7 where 1=Monday, 7=Sunday
- Days 6 and 7 are Saturday and Sunday
- The second parameter (2) sets Monday as day 1

**Alternative Solution**:
```dax
Day_Type = 
IF(
    WEEKDAY([Date Time]) = 1 || WEEKDAY([Date Time]) = 7,
    "Weekend",
    "Weekday"
)
```
Here, the default `WEEKDAY()` uses 1=Sunday, 7=Saturday.

---

## Exercise 4: Aggregation with Related Tables

### Task 4.1: Add Weight to Sales Data

```dax
Product_Weight = RELATED(weight[Weight_grams])
```

**Explanation**: 
- `RELATED()` retrieves a column from a related table
- Requires a relationship between the tables on the Product column
- Works in the "many" side of a one-to-many relationship

---

### Task 4.2: Total Weight per Transaction

```dax
Total_Weight_Grams = [Product_Weight] * [Items]
```

**Explanation**: Multiplies the weight per unit by the quantity sold to get total weight for that transaction.

**Alternative (Single Formula)**:
```dax
Total_Weight_Grams = RELATED(weight[Weight_grams]) * [Items]
```

---

## Exercise 5: Measures (Basic Aggregations)

### Task 5.1: Total Revenue Measure

```dax
Total_Revenue = SUM(sales_data[Total Spend])
```

**Explanation**: 
- Measures use `SUM()`, `AVERAGE()`, etc., to aggregate column values
- Unlike calculated columns, measures respond to filter context
- Always reference: `TableName[ColumnName]` in measures

**Expected Result**: ~$491,088

---

### Task 5.2: Average Transaction Value

```dax
Avg_Transaction_Value = AVERAGE(sales_data[Total Spend])
```

**Explanation**: `AVERAGE()` calculates the mean of all values in the Total Spend column.

**Expected Result**: ~$4,911

---

### Task 5.3: Total Items Sold

```dax
Total_Items_Sold = SUM(sales_data[Items])
```

**Explanation**: Sums all items across all transactions.

**Expected Result**: 1,341 items

---

### Task 5.4: Number of Transactions

```dax
Transaction_Count = COUNTROWS(sales_data)
```

**Explanation**: `COUNTROWS()` counts the number of rows in the table (each row = one transaction).

**Alternative Solutions**:
```dax
Transaction_Count = COUNT(sales_data[Sales ID])
```
or
```dax
Transaction_Count = DISTINCTCOUNT(sales_data[Sales ID])
```

**Expected Result**: 100 transactions

---

### Task 5.5: Total Weight Shipped (Measure)

```dax
Total_Weight_Shipped = 
SUMX(
    sales_data,
    sales_data[Items] * RELATED(weight[Weight_grams])
)
```

**Explanation**: 
- `SUMX()` is an iterator function that evaluates an expression for each row
- For each row, it multiplies Items by the related Weight
- Then sums all the results

**Alternative (if you created the Total_Weight_Grams column)**:
```dax
Total_Weight_Shipped = SUM(sales_data[Total_Weight_Grams])
```

---

## Bonus Challenge: Advanced Measures

### Task 6.1: High Value Sales Count

```dax
High_Value_Sales_Count = 
COUNTROWS(
    FILTER(
        sales_data,
        sales_data[Total Spend] > 5000
    )
)
```

**Explanation**: 
- `FILTER()` returns a table with only rows matching the condition
- `COUNTROWS()` counts the filtered rows

**Alternative using CALCULATE**:
```dax
High_Value_Sales_Count = 
CALCULATE(
    COUNTROWS(sales_data),
    sales_data[Total Spend] > 5000
)
```

---

### Task 6.2: Revenue from Product A

```dax
Product_A_Revenue = 
CALCULATE(
    SUM(sales_data[Total Spend]),
    sales_data[Product] = "Product A"
)
```

**Explanation**: 
- `CALCULATE()` modifies the filter context
- The second parameter applies a filter for Product A only
- Then calculates the sum in that filtered context

**Alternative using SUMX and FILTER**:
```dax
Product_A_Revenue = 
SUMX(
    FILTER(sales_data, sales_data[Product] = "Product A"),
    sales_data[Total Spend]
)
```

---

### Task 6.3: Average Items per Sale by City

```dax
Avg_Items_Per_City = AVERAGE(sales_data[Items])
```

**Explanation**: 
- This simple measure will automatically respond to filter context
- When used with City in a visual, it calculates the average for each city
- No explicit filtering needed - Power BI handles this automatically

**Alternative (more explicit)**:
```dax
Avg_Items_Per_City = 
DIVIDE(
    SUM(sales_data[Items]),
    COUNTROWS(sales_data),
    0
)
```
This calculates average manually: total items / number of transactions. The third parameter (0) handles division by zero.

---

## Testing Your Solutions with DAX Studio

You can test aggregations using DAX Studio with EVALUATE queries:

### Test 1: Total Revenue by City
```dax
EVALUATE
SUMMARIZE(
    sales_data,
    sales_data[City],
    "Total Revenue", SUM(sales_data[Total Spend])
)
ORDER BY [Total Revenue] DESC
```

---

### Test 2: Total Weight by City
```dax
EVALUATE
SUMMARIZE(
    sales_data,
    sales_data[City],
    "Total Weight", 
    SUMX(
        sales_data,
        sales_data[Items] * RELATED(weight[Weight_grams])
    )
)
ORDER BY [Total Weight] DESC
```

---

### Test 3: Product Performance Summary
```dax
EVALUATE
ADDCOLUMNS(
    SUMMARIZE(
        sales_data,
        sales_data[Product]
    ),
    "Total Revenue", 
        CALCULATE(SUM(sales_data[Total Spend])),
    "Total Items Sold", 
        CALCULATE(SUM(sales_data[Items])),
    "Avg Price", 
        CALCULATE(AVERAGE(sales_data[Unit_Price])),
    "Transaction Count", 
        CALCULATE(COUNTROWS(sales_data))
)
ORDER BY [Total Revenue] DESC
```

---

## Common DAX Mistakes to Avoid

1. **Using calculated columns when you need measures**
   - Columns are evaluated row-by-row and stored in the model
   - Measures are evaluated based on filter context and not stored

2. **Forgetting table names in measures**
   - ✗ `SUM([Total Spend])`
   - ✓ `SUM(sales_data[Total Spend])`

3. **Not handling BLANK values**
   - Use `DIVIDE()` instead of `/` to avoid errors
   - Use `IF(ISBLANK(...))` to handle blank values

4. **Confusing FILTER vs. CALCULATE**
   - `FILTER()` returns a table
   - `CALCULATE()` modifies filter context and returns a value

5. **Using RELATED on the wrong side of the relationship**
   - `RELATED()` works from the many side to the one side
   - `RELATEDTABLE()` works from the one side to the many side

---

## Key Takeaways

✓ **Calculated Columns** are computed row-by-row and stored in the model  
✓ **Measures** are computed dynamically based on filter context  
✓ **Iterator functions** (SUMX, FILTER, etc.) evaluate row-by-row  
✓ **CALCULATE** is one of the most powerful DAX functions for modifying filter context  
✓ **RELATED** brings data from related tables (follows relationships)  
✓ Format functions return text; conversion functions like VALUE convert back to numbers  

---

**Congratulations on completing the worksheet!** Continue practicing with your own datasets to master DAX.
