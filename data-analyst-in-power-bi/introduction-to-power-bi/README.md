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
