# Data Analyst in Power BI

## Introduction to Power BI

### Power BI

```
Power BI interface, consists of :
- **Report View**
- **Table View**
- **Model View**
- **DAX Query Editor**
```

### What is DAX?

- DAX stands for Data Analysis Expressions.
- Formula language to create calculations
  - Columns, tables, measures
- Based on Excel formulas and functions
- Used in other Microsoft tools
  Power Pivot and Analysis Services

#### DAX functions

- Predefined formulas that perform calculations on specific values called **arguments**.
- **Function syntax** indicates the order of arguments expected
- Function categories

  - Agrreggations - `SUM`, `AVERAGE`, `COUNT`
  - Date and time - `YEAR`, `MONTH`, `DAY`
  - Logical - `IF`, `AND`, `OR`
  - Text - `CONCATENATE`, `LEFT`, `RIGHT`
  - And many more...

- DAX function example:

```dax
<!-- SUM() -->
SUM(Sales[Total])

<!-- LEFT -->
LEFT(Customer[Name], 5) = "Smith"
```

#### Creating calculated columns

- Expands our existing datasets without editing the source
- Evaluates at row level and adds a new column to an existing table
- Calculated at data load and when data is refreshed
- Example:

```dax
Price_w_tax = Price + Price * tax_rate
```

#### Creating calculated measures

- Enables complex calculations
- Aggregates multiple rows and adds a new field that can be added to visualizations
- Calculated at **query time** as you interact and filter

| Calculated Column           | Calculated Measure                |
| --------------------------- | --------------------------------- |
| For evaluating each row     | For aggregating multiple rows     |
| Add a new column to a table | Add a new field to visualizations |
| Calculated at data load     | Calculated at query time          |

#### Context in DAX

- Enables dynamic analysis where results of a formula change to reflect the selected data
- There are 3 type of context in DAX:
  | Context Type | Definition | Common Example |
  |--------------------|-------------------------------------------------------------|----------------------------------------------------------|
  | **Row Context** | Evaluation that happens **row by row** | `Calculated column`, `SUMX`, `FILTER` (row iteration) |
  | **Filter Context** | Active filters applied during DAX evaluation | Slicers, chart legends, `CALCULATE()` |
  | **Query Context** | Instruction from Power BI to retrieve and display data | Dragging fields into visuals (e.g., rows/columns/values) |

- Iterator functions like `SUMX`, `AVERAGEX`, and `FILTER` are used to iterate over rows in a table, applying calculations row by row.

```dax
SUMX(Sales, Sales[Quantity] * Sales[Price])
```

```dax
GoldVolume21 = CALCULATE(
  SUM(Commodities[Volume]),
  FILTER(Commodities, Commodities[Symbol] = "Gold),
  FILTER(Commodities, Commodities[Year] = 2021)
)
```

```dax
Gold21vs20 =
VAR Gold21 = CALCULATE(
  SUM(Commodities[Volume]),
  FILTER(Commodities, Commodities[Symbol] = "Gold"),
  FILTER(Commodities, Commodities[Year] = 2021)
)
RETURN DIVIDE([Gold21]-GoldVolume20, GoldVolume20)
```

```dax
TotalSales_w_increase =
VAR Increase = 0.05
RETURN [TotalSales] + ([TotalSales]*Increase)
```

```dax
SalesCount = DISTINCTCOUNT(Sales[OrderNo])
```

```dax
2018 Bikes Revenue = CALCULATE(
    SUM(Sales[LinePrice]),
    FILTER(
        Sales,
        Sales[ProductCategory] = "Bikes"
        && YEAR(Sales[OrderDate]) = 2018
    )
)
```

#### Working with dates

- Date and Time functions

  - `YEAR()`
  - `MONTH()`
  - `DAY()`
  - `WEEKDAY()`
  - `DATE()`
  - `DATEDIFF()`
  - `QUARTER()`

- Format dates using `FORMAT()`

  - FORMAT(<date>, <"dddd">) -> Friday
  - FORMAT(<date>, <"h"nn"ss">) -> 1:30:00

- Time Intelligence functions

  - `LASTDATE()`
  - `DATESBETWEEN()`
  - `DATEADD()`

#### Creatung a Date Table

- A dedicated date table is highly recommended for accurate reporting using time intelligence functions.
- Filter by multiple date attributes such as Year and Month
- Custom calendar view/definitions such as fiscal dates
- Use of time-intelligence to select a time horizon (Today, Yesterday, Last 30 days, etc.)
- Types of Analysis:
  - Revenue by Day of Week, Fisca Performance, Public Holidays
- `CALENDAR(start_date, end_date)` function to create a date table
- `CALENDARAUTO()` function to automatically generate a date table based on the data model

- Example

```dax
Dates = CALENDAR(MIN(Sales[OrderDate]), MAX(Sales[OrderDate]))
```
