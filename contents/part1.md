# Power BI - Part 1

## data

### flatfile



## To Do


* Add `Unit Price` column

```dax
unit_price = [Total Spend] / [Items]
```

* Add `Month` column

```dax
Month = FORMAT([Date Time], "MMMM")
```

* Add `Data Date` column 

```dax
data_date = VALUE(FORMAT([Date Time], "YYYYMMDD"))
```

* Add `Revenue Tier` column


 Revenue Tier - Categorized as Low (<$2,000), Medium ($2,000-$6,000), or High (>$6,000)

 ```dax
 Revenue Tier = 
SWITCH(
    TRUE(),
    [Total Spend] < 2000, "Low",
    [Total Spend] <= 6000, "Medium",
    "High"
)
 ```

### Manually create `weight` table

Copy and paste the below into `Enter Data`, right lick on Column 1 :  `Paste >  Name: weight > Load`

```csv

Product	Weight_grams
Product A	115
Product B	220
Product C	3000
Product D	410
Product E	525
```


### Using DAX Studio to "test" aggregation - use groupby to calculate total weight by city and product

```dax
EVALUATE
SUMMARIZE(
    'sales_data',
    'sales_data'[City],
    "Total Weight", 
    SUMX(
        'sales_data',
        RELATED('weight'[Weight_grams])
    )
)
ORDER BY [Total Weight] DESC
```

### Consider sorting in DAX 

Example 1:

```dax
EVALUATE
SUMMARIZE(
    'sales_data',
    'sales_data'[City],
    "Total Weight", 
    SUMX(
        'sales_data',
        RELATED('weight'[Weight_grams])
    )
)
ORDER BY [Total Weight] DESC

```

### Creating and using measures in DAX

Example 1: Create a measure to calculate total weight

```dax
Total Weight = 
SUMX(
    'sales_data',
    RELATED('weight'[Weight_grams])
)

```
Example 2: Use the measure in a visual
1. Create a table visual in Power BI
2. Add `City` from `sales_data` to the rows
3. Add the `Total Weight` measure to the values
4. Sort the table by `Total Weight` in descending order

Example 3: Create a measure to calculate average weight per item

```dax
Average Weight per Item =
DIVIDE(
    [Total Weight],
    SUM('sales_data'[Items])
)
```

Example 4: Use the average weight measure in a visual
1. Create a table visual in Power BI
2. Add `City` from `sales_data` to the rows
3. Add the `Average Weight per Item` measure to the values
4. Sort the table by `Average Weight per Item` in descending order

