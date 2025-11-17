# Power BI Capstone Projects – Corporate Finance & Retail

This repository contains two end-to-end **Power BI capstone projects** with ready-made Excel datasets:

1. **Corporate Financial Performance Dashboard**  
2. **Retail Sales & Margin Performance Dashboard**

Both datasets follow a star-schema structure (Fact + Dimensions) and are sized to feel realistic but still remain lightweight for training.

---

## Project 1 – Corporate Financial Performance (Atlas Group)

### 1. Scenario

Atlas Group is a diversified corporate group with several legal entities and cost centers. Budget and Actual figures are scattered across multiple spreadsheets, making it hard for the CFO and Finance team to get a single, trusted view of performance.

You are given a consolidated dataset at **monthly grain** across:

- Companies  
- Cost centers  
- Strategic programs / initiatives  
- Financial accounts (Revenue, OPEX, CAPEX)  
- Scenarios: **Budget** and **Actual**

The goal is to build a **Financial Performance Dashboard** in Power BI that shows:

- Overall performance vs budget  
- Variances by cost center, program, and account  
- Trends over time (2024–2025)

---

### 2. Dataset

**File:** `Corporate_Financial_Dashboard_Training_Data.xlsx`  
**Grain:** Monthly, per `CompanyID`–`CostCenterID`–`ProgramID`–`AccountID`–`Scenario`.

The workbook contains:

- `Fact_Finance` – core financial records (budget + actual)  
- `Dim_Date` – calendar dimension (monthly detail)  
- `Dim_Company` – legal entities  
- `Dim_CostCenter` – corporate cost centers  
- `Dim_Program` – strategic programs / initiatives  
- `Dim_Account` – financial accounts  
- `Data_Dictionary` – column-level descriptions

---

### 3. Fact Table

#### `Fact_Finance`

Each row represents a **monthly financial amount** for a given combination of company, cost center, program, account and scenario (budget or actual).

Columns:

- `RecordID` – unique row ID  
- `Scenario` – `"Budget"` or `"Actual"`  
- `Date` – first day of the month (e.g. `2025-01-01`)  
- `Year` – year of the record  
- `Month` – month number (`1–12`)  
- `CompanyID` – company / legal entity identifier  
- `CostCenterID` – cost center responsible for the amount  
- `ProgramID` – strategic program / initiative  
- `AccountID` – financial account  
- `Amount_OMR` – amount in OMR (positive values only, sign convention simplified for training)

---

### 4. Dimension Tables

#### `Dim_Date`

- `Date` – date key (first of each month)  
- `Year` – calendar year  
- `Month` – month number (`1–12`)  
- `MonthName` – short month name (`Jan`, `Feb`, …)  
- `YearMonth` – formatted label (e.g. `2024-01`)

#### `Dim_Company`

- `CompanyID` – unique identifier  
- `CompanyName` – company / legal entity name  
- `Region` – primary operating region

#### `Dim_CostCenter`

- `CostCenterID` – unique identifier  
- `CostCenterName` – descriptive name of the cost center  
- `CompanyID` – which company the cost center belongs to

#### `Dim_Program`

- `ProgramID` – unique program ID  
- `ProgramName` – program / initiative name  
- `ProgramCategory` – strategic theme (e.g. Digital, Customer, Efficiency, Growth, Compliance, People)

#### `Dim_Account`

- `AccountID` – account ID  
- `AccountName` – account description (e.g. Salaries & Employee Benefits, IT & Software Expenses)  
- `AccountCategory` – high-level type (e.g. `Revenue`, `OPEX`, `CAPEX`)

---

### 5. Recommended Data Model (Power BI)

Suggested relationships (many-to-one, single direction from dimensions to fact):

- `Fact_Finance[Date]` → `Dim_Date[Date]`  
- `Fact_Finance[CompanyID]` → `Dim_Company[CompanyID]`  
- `Fact_Finance[CostCenterID]` → `Dim_CostCenter[CostCenterID]`  
- `Fact_Finance[ProgramID]` → `Dim_Program[ProgramID]`  
- `Fact_Finance[AccountID]` → `Dim_Account[AccountID]`

This forms a clean **star schema** with `Fact_Finance` at the center.

---

## Project 2 – Retail Sales & Margin Performance (NovaMart)

### 1. Scenario

NovaMart is a retail chain with multiple physical stores and an online channel. Reporting is fragmented across store spreadsheets and category-level summaries. The commercial team wants a **Retail Performance Dashboard** to understand:

- Which stores and categories are driving revenue and margin  
- Performance differences between Store vs Online channels  
- The impact of promotions on sales and profitability

You are given **daily sales data for 2024** with:

- Stores (locations, types, regions)  
- Products (categories, brands, regular price)  
- Channels (Store, Online)  
- Promotions (types and periods)  
- Quantity, net sales, and COGS

The goal is to build a Power BI report that explains:

- Sales and margin by store, category, product  
- Channel mix and performance  
- Promotion effectiveness

---

### 2. Dataset

**File:** `Retail_Sales_Performance_Training_Data.xlsx`  
**Grain:** One row per `Date`–`StoreID`–`ProductID`–`ChannelID`–(optional `PromotionID`).

The workbook contains:

- `Fact_Sales` – daily sales lines  
- `Dim_Date` – calendar dimension (daily detail)  
- `Dim_Channel` – Store / Online  
- `Dim_Store` – store locations and types  
- `Dim_Product` – products, categories, brands and regular prices  
- `Dim_Promotion` – promotion campaigns and their date ranges  
- `Data_Dictionary` – column-level descriptions

---

### 3. Fact Table

#### `Fact_Sales`

Each row represents a **summarised transaction line** (daily level) for a product sold in a given store, channel and (optionally) promotion.

Columns:

- `SalesID` – unique row ID  
- `Date` – transaction date  
- `StoreID` – store where the sale occurred  
- `ProductID` – product sold  
- `ChannelID` – sales channel (`Store` / `Online`)  
- `PromotionID` – promotion applied, if any (nullable)  
- `Quantity` – units sold  
- `NetSales_OMR` – net sales value after discounts, in OMR  
- `COGS_OMR` – cost of goods sold, in OMR (for margin analysis)

---

### 4. Dimension Tables

#### `Dim_Date`

- `Date` – date key  
- `Year` – calendar year  
- `Month` – month number (`1–12`)  
- `MonthName` – short month name  
- `Day` – day of month  
- `DayOfWeek` – name of day (e.g. `Monday`)  
- `IsWeekend` – boolean flag (`TRUE`/`FALSE`)  
- `WeekOfYear` – ISO week number

#### `Dim_Channel`

- `ChannelID` – unique identifier  
- `ChannelName` – channel label (`Store`, `Online`)

#### `Dim_Store`

- `StoreID` – unique store ID  
- `StoreName` – store name (e.g. NovaMart City Center)  
- `City` – city name  
- `Region` – sales region  
- `StoreType` – type of store (`Mall`, `Neighborhood`, `Hypermarket`, `Travel`, `Warehouse`, `Fulfillment Center`, etc.)  
- `OpeningDate` – date when the store was opened

#### `Dim_Product`

- `ProductID` – unique product ID (SKU)  
- `ProductName` – display name  
- `Category` – high-level category (e.g. `Grocery - Dry`, `Fresh Food`, `Beverages`, `Home & Cleaning`, `Personal Care`, `Electronics`)  
- `Subcategory` – more detailed group (e.g. `Pasta & Rice`, `Fruit`, `Detergents`, `Skincare`, `Small Appliances`)  
- `Brand` – brand name  
- `RegularPrice_OMR` – standard price per unit before promotions/discounts

#### `Dim_Promotion`

- `PromotionID` – unique ID  
- `PromotionName` – promotion campaign name  
- `PromotionType` – promotion type (`Discount %`, `Bundle`, `Weekend Promo`, `Buy 1 Get 1`)  
- `StartDate` – promotion start date  
- `EndDate` – promotion end date

---

### 5. Recommended Data Model (Power BI)

Suggested relationships:

- `Fact_Sales[Date]` → `Dim_Date[Date]`  
- `Fact_Sales[StoreID]` → `Dim_Store[StoreID]`  
- `Fact_Sales[ProductID]` → `Dim_Product[ProductID]`  
- `Fact_Sales[ChannelID]` → `Dim_Channel[ChannelID]`  
- `Fact_Sales[PromotionID]` → `Dim_Promotion[PromotionID]`  

This forms a star schema centered on `Fact_Sales`. You can keep the promotion relationship active or tune it (e.g. use inactive relationship with `USERELATIONSHIP`) if you want to extend the complexity for advanced modeling scenarios.

---

## How to Use This Repository

1. Clone or download the repository.  
2. Open **Power BI Desktop**.  
3. For each project:
   - `Get Data` → `Excel` → select `Corporate_Financial_Dashboard_Training_Data.xlsx` or `Retail_Sales_Performance_Training_Data.xlsx`.  
   - Load all Fact + Dimension tables and optionally the `Data_Dictionary`.  
   - Build the relationships as described under each project.  
4. Build your own measures and visuals on top of the models for training, workshops, or portfolio demos.
