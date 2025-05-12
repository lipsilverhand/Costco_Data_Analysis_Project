# Costco Data Analysis Power Bi Dashboard Project

![images](https://github.com/user-attachments/assets/af740559-c15b-45f2-9d79-92ca8bef85d9)

## Power BI Screenshoots Snapshots:
<p align="center">
  <img src="https://github.com/user-attachments/assets/441c86c3-d05d-4141-9a26-57f3d15c0a96" />
  <br>
  Default Dashboard
</p>

<br><br>

<p align="center">
  <img src="https://github.com/user-attachments/assets/358a3faf-b604-4e49-92bd-6673f2e20b72" />
  <br>
  Filter Panel
</p>

<br><br>

<p align="center">
  <img src="https://github.com/user-attachments/assets/b14604bd-b315-4400-ba0f-1ecd022d45cb" />
  <br>
  Year 2020
</p>

<br><br>

<p align="center">
  <img src="https://github.com/user-attachments/assets/006fc5b6-ab15-485a-b9b4-65a141ab60b8" />
  <br>
  Year 2021
</p>

<br><br>

<p align="center">
  <img src="https://github.com/user-attachments/assets/9975e755-0f7c-440d-80f2-9e6a67079d7b"
 />
  <br>
  Year 2022
</p>

<br><br>

<p align="center">
  <img src="https://github.com/user-attachments/assets/1f216de1-081f-4a3a-9f85-911d5913629b"/>
  <br>
  Year 2023
</p>

<br><br>

<p align="center">
  <img src="https://github.com/user-attachments/assets/0379d13d-51af-4ef7-b5cd-4309b96bf93d"/>
  <br>
  Year 2024
</p>




## Financial Metrics:
Revenue = quantity * unit price

Net sale = revenue * (1 - discount)

Gross profit = Net sale - Cogs * quantity

## DAX queries:
### Total revenue:
```text
total revenue = 
SUM(sales[revenue])
```

### Total quantity:
```text
total revenue = 
SUM(sales[quantity])
```

### Total profit:
```text
total profit = 
SUM(sales[gross profit])
```

### Total orders:
```text
total orders = 
DISTINCTCOUNT(sales[order_id])
```

###  Revenue target:
```text
revenue target = 
VAR LastYearSale =
    CALCULATE([total revenue], SAMEPERIODLASTYEAR(date_table[Date]))
RETURN
    IF(ISBLANK(LastYearSale), 70000, 
                                    LastYearSale * 1.5
    )
```

###  Order target:
```text
order target = 
VAR LastYearOrder =
    CALCULATE([total orders], SAMEPERIODLASTYEAR(date_table[Date]))
RETURN
    IF(ISBLANK(LastYearOrder), 15000, 
                                    LastYearOrder * 1.5
    )
```

### KPI revenue:
```text
KPI revenue = 
// This func find last year and current year revenue diff in %

VAR LastYearRevenue = CALCULATE([total revenue], SAMEPERIODLASTYEAR(date_table[Date]))
VAR CurrentYearRevenue = CALCULATE([total revenue], DATESYTD(date_table[Date]))

VAR Diff = CurrentYearRevenue - LastYearRevenue
VAR Diff_ratio = DIVIDE(Diff, LastYearRevenue, 0)
VAR ABS_Diff = ABS(Diff_ratio)

VAR ResultText = IF(ISBLANK(LastYearRevenue), "NO SALE LAST YEAR",
                                            IF(Diff_ratio > 0, FORMAT(ABS_Diff, "0.0%") & " MORE THAN LAST YEAR" & " (▲)", FORMAT(ABS_Diff, "0.0%") & " LESS THAN LAST YEAR" & " (▼)"))
RETURN ResultText
```

### KPI profit:
```text
KPI profit = 
// This func find last year and current year profit diff in %

VAR LastYearProfit = CALCULATE([total profit], SAMEPERIODLASTYEAR(date_table[Date]))
VAR CurrentYearProfit = CALCULATE([total profit], DATESYTD(date_table[Date]))

VAR Diff = CurrentYearProfit - LastYearProfit
VAR Diff_ratio = DIVIDE(Diff, LastYearProfit, 0)
VAR ABS_Diff = ABS(Diff_ratio)

VAR ResultText = IF(ISBLANK(LastYearProfit), "NO SALE LAST YEAR",
                                            IF(Diff_ratio > 0, FORMAT(ABS_Diff, "0.0%") & " MORE THAN LAST YEAR" & " (▲)", FORMAT(ABS_Diff, "0.0%") & " LESS THAN LAST YEAR" & " (▼)"))

RETURN ResultText 
```

### KPI quantity:
```text
KPI quantity = 
// This func find last year and current year quantity diff in %

VAR LastYearQuantity = CALCULATE([total quantity], SAMEPERIODLASTYEAR(date_table[Date]))
VAR CurrentYearQuantity = CALCULATE([total quantity], DATESYTD(date_table[Date]))

VAR Diff = CurrentYearQuantity - LastYearQuantity

VAR ABS_Diff = ABS(Diff)/1000

VAR ResultText = IF(ISBLANK(LastYearQuantity), "NO SALE LAST YEAR",
                                            IF(Diff > 0, FORMAT(ABS_Diff, "0.0")&"K " & "MORE THAN LAST YEAR" & " (▲)", FORMAT(ABS_Diff, "0.0") &"K " & "LESS THAN LAST YEAR" & " (▼)"))

RETURN ResultText
```

## Model View:
![{1CA4D410-7FAD-4B94-B8E0-A123B9FEDA12}](https://github.com/user-attachments/assets/3fd2802f-15ba-4f29-8b30-6e55523f8f75)


## Findings and Conclusion (Insights):
### Findings:
* Total Revenue (All Years): $3.7M

* Total Profit (All Years): $1.6M

* Total Quantity Sold: 49.3K units

* Top Customer Segment: Consumer (53.48% of total revenue)

* Top Region: West (consistently highest revenue contributor)

* Top State: California (highest sales every year)

* Order Fulfillment Rate: 15.9K orders completed vs. 17K target (94%)

| Year | Revenue  | Profit   | Quantity | Orders vs Target | Notes              |
| ---- | -------- | -------- | -------- | ---------------- | ------------------ |
| 2020 | \$536.6K | \$239.4K | 7.6K     | 2K / 15K         | Baseline year      |
| 2021 | \$541.3K | \$242.3K | 8.0K     | 2.1K / 3K        | Flat growth        |
| 2022 | \$898.1K | \$357.8K | 11.2K    | 4K / 3.2K        | Peak growth        |
| 2023 | \$858.2K | \$395.9K | 12.3K    | 3.3K / 6K        | Profit improvement |
| 2024 | \$832.9K | \$345.7K | 10.2K    | 4.6K / 4.9K      | KPI decline        |

### Conclusion:
* 2022 marked the strongest year of growth, with revenue increasing by 66%, driven by higher order volume and improved segment performance.

* 2023 profit increased by 10.7% despite a 4.4% drop in revenue, suggesting gains in cost control or margin improvement.

* 2024 showed a decline in revenue, profit, and quantity sold — this may reflect market saturation, economic conditions, or operational inefficiencies. A deeper analysis is needed.

* Consumer segment is declining in revenue share, while Corporate and Home Office segments are growing, highlighting a shift in customer behavior or opportunity for B2B focus.

* The West region and California consistently led in performance — consider analyzing these areas for strategies that can be scaled across other regions.

* Order fulfillment efficiency improved after 2021, indicating successful operational adjustments — continued investment in logistics and customer experience is recommended.




