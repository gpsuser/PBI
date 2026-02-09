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


### create new aggregate table - use groupby to calculate total weight by product

```dax
CityProductWeights = 
SUMMARIZE(
    'sales_data',
    'sales_data'[City],
    'sales_data'[Product],
    "Total Weight", 
    SUMX(
        'sales_data',
        RELATED('weight'[Weight_grams])
    )
)
```