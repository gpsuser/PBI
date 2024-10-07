# PBI

## Notes

Discuss PBI licensing
Filtering for row level security 

## Section 2

* Download the .txt file an import into PBI

### SQL

Show connection between SQL server and Power BI
Show SQL code in Power BI

```sql
SELECT [ProductName]
      ,[Price]
      ,[PurchaseDate]
FROM [TEST].[dbo].[ShoppingBasket]
```

* Show CTE in Power BI

### Prompt 1

```text
I have a table called Shopping with the following columns: Price, ProductName, PurchaseDate.

Please write a DAX script to create a table that represents a group by ProductName and sum over Price
```

#### Prompt 1 output

```dax
ProductSummary = SUMMARIZE(Shopping, Shopping[ProductName], "Total Price", SUM(Shopping[Price]))
```

#### Test prompt 1 in Power BI

```dax
EVALUATE
  --  TOPN(100, 'financials_new')
  SUMMARIZE(Shopping, Shopping[ProductName], "Total Price", SUM(Shopping[Price]))
```

#### Paste prompt 1 into Power BI

```instructions
Modelling > New Table > ProdTbl = SUMMARIZE(Shopping, Shopping[ProductName], "Total Price", SUM(Shopping[Price]))
```

### Prompt 2

I have a table called financials_new and would like to create a table containing the distinct Country values from financials_new table. Please write a DAX script to create this table.

#### Prompt 2 output

```dax
DistinctCountries = DISTINCT(financials_new[Country])
```

#### Paste prompt2 into Power BI

```instructions
Modelling > New Table > DistinctCountries = DISTINCT(financials_new[Country])
```

### Manually entering data

```instructions
Home  > Enter Data
```

Copy and paste the below into `Enter Data` and then click `Load`

```csv   ShoppingCountry

Product	Country
Product A	Mexico
Product B	France
Product C	Germany
Product D	Canada
Product E	Canada
Product F	United States of America
Product G	United States of America
Product H	United States of America
```

### Keys and relationships

* Notice the Data Model info and primary keys
* If you delete the relationship and then create a table and try and show product with country it wont work
* If you recreate the key and then create the table with product and country - then it works

### TO DO

* Check rendering in PBI service portal  
* Discuss M Language
* Larger data sets:
<https://github.com/microsoft/powerbi-desktop-samples/blob/main/DAX/Adventure%20Works%20DW%202020.pbix>

## Section 3

1. Matrix - hierarchy
2. Scaling  (https://github.com/gpsuser/samples/blob/master/random%20notes.md)
3. Python


### Python Prompt

I have a data frame called dataset with two columns called ProductName and TotalPrice. I would like to generate a histogram in python, showing the ProductName on the x axis and the TotalPrice on the y axis. Please generate this python code for me.

```python
import matplotlib.pyplot as plt
import pandas as pd

# Assuming 'dataset' is your DataFrame and it's already defined
# dataset = pd.DataFrame({'ProductName': ['Product A', 'Product B', ...], 'TotalPrice': [120, 340, ...]})

# Create a bar chart
plt.figure(figsize=(10, 8))  # Adjust the size as needed
plt.bar(dataset['ProductName'], dataset['TotalPrice'], color='skyblue')

# Add title and labels
plt.title('Total Price by Product Name')
plt.xlabel('Product Name')
plt.ylabel('Total Price')

# Rotate the product names on x-axis for better readability
plt.xticks(rotation=45, ha='right')

# Show the plot
plt.tight_layout()  # Adjust the layout
plt.show()
```


4. SQL   GPS-DELL-XPS\MYTEST, TEST, dbo, ShoppingBasket,   SELECT * FROM ShoppingBasket
   Home > Transform Data > Data Source Settings

5. Q&A

how many countries are in financials_new Country?
what is the total financials new cog?

6. KEY INFLUENCES



<!-- Summary -->

Chart Types
Tables and Matrix
Filtering
Power BI ServicePortal
DataModlleing
Formatting
DAX
Python - Custom visuals
SQL
FORECASTING - third party widgets
Q&A
AI - Key Influences

## PART 2 - INTERMEDIATE


Prompt:

Please generate the dax code that will allow me to group by
Dim_Customer[Customer] and Dim_Product[Product] and then 
calculate the sum of Fact_Sales[Total Spend] ,
WHERE Dim_City[Population Size] > 200000 and 
sort by customer and product

Answer - wrong

```dax
EVALUATE
VAR FilteredSales = 
    FILTER(
        ADDCOLUMNS(
            Fact_Sales,
            "Population Size", RELATED(Dim_City[Population Size])
        ),
        [Population Size] > 200000
    )
VAR GroupedSales = 
    SUMMARIZE(
        FilteredSales,
        Dim_Customer[Customer],
        Dim_Product[Product],
        "Total Spend", SUM(Fact_Sales[Total Spend])
    )
RETURN
    SORTBY(GroupedSales,Dim_Customer[Customer], ASC, Dim_Product[Product], ASC)
```
Answer - correct
  
  ```dax
  EVALUATE
  VAR FilteredSales = 
      FILTER(
          ADDCOLUMNS(
              Fact_Sales,
              "Population Size", RELATED(Dim_City[Population Size])
          ),
          [Population Size] > 200000
      )
  VAR GroupedSales = 
      SUMMARIZE(
          FilteredSales,
          Dim_Customer[Customer],
          Dim_Product[Product],
          "Total Spend", SUM(Fact_Sales[Total Spend])
      )
  RETURN
      GroupedSales  ORDER BY Dim_Customer[Customer] ASC, Dim_Product[Product] ASC
  ```

Group BY - using variables and NO SORTING 

* once in table form you can sort - cant impose sort before

```dax

tbl_GroupByTest = VAR FilteredSales = 
    FILTER(
        ADDCOLUMNS(
            Fact_Sales,
            "Population Size", RELATED(Dim_City[Population Size])
        ),
        [Population Size] > 200000
    )
VAR GroupedSales = 
    SUMMARIZE(
        FilteredSales,
        Dim_Customer[Customer],
        Dim_Product[Product],
        "Total Spend", SUM(Fact_Sales[Total Spend])
    )
RETURN
	GroupedSales  
```
