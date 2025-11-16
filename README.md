
# Power BI Capstone Projects – Corporate Finance & Retail

This repository contains two end-to-end **Power BI capstone projects** with ready-made datasets, designed for hands-on training:

1. **Corporate Financial Performance Dashboard**  
2. **Retail Sales & Margin Performance Dashboard**

Both projects use realistic, relational Excel datasets that mimic real-life BI models (facts + dimensions, proper grain, enough rows to be interesting but still manageable on a laptop).

---

## 1. Projects Overview

### 1.1 Corporate Financial Performance (Atlas Group)

**Scenario**

Atlas Group is a diversified corporate group with multiple legal entities and cost centers. Budget and Actuals are currently scattered across spreadsheets. The CFO wants a **single Financial Performance Dashboard** to support:

- Monthly performance meetings  
- Mid-year and year-end reviews  
- Program steering and cost control  

You get a consolidated dataset at **monthly grain** across:

- Companies  
- Cost centers  
- Programs / initiatives  
- Accounts (Revenue, OPEX, CAPEX)  
- Scenarios: **Budget vs Actual**

The goal is to build an interactive Power BI report that shows:

- Overall performance vs budget  
- Variances by cost center, program and account  
- Trends over time (2024–2025)

---

### 1.2 Retail Sales & Margin Performance (NovaMart)

**Scenario**

NovaMart is a retail chain with physical stores and an online channel. Reporting is fragmented between store managers, category managers and HQ. The Chief Commercial Officer wants a **Retail Performance Dashboard** to track:

- Sales, quantity, margin  
- Store performance  
- Category performance  
- Channel mix (Store vs Online)  
- Impact of promotions  

You get **transaction-level daily data** for one year (2024) across:

- Stores  
- Products / categories / brands  
- Channels (Store / Online)  
- Promotions  
- Quantity, net sales, COGS  

The goal is to build a Power BI report that can answer:

- Which stores and categories drive sales and profit?  
- What is margin % by store, category, product?  
- How do promotions and channels impact performance?

---

## 2. Datasets

### 2.1 Files

- `Corporate_Financial_Dashboard_Training_Data.xlsx`  
- `Retail_Sales_Performance_Training_Data.xlsx`  

Each file contains:

- A main **Fact** table (transactions / records)  
- Multiple **Dimension** tables (date, entity, etc.)  
- A **Data_Dictionary** sheet describing all columns

---

## 3. Corporate Financial Dataset Structure

**File:** `Corporate_Financial_Dashboard_Training_Data.xlsx`  
**Grain:** Monthly, per Company–CostCenter–Program–Account–Scenario

### 3.1 Fact Table

#### `Fact_Finance`

- `RecordID` – unique row ID  
- `Scenario` – `"Budget"` or `"Actual"`  
- `Date` – first of the month (e.g. 2025-01-01)  
- `Year` – year of the record  
- `Month` – month number (1–12)  
- `CompanyID` – company / legal entity  
- `CostCenterID` – cost center  
- `ProgramID` – strategic program / initiative  
- `AccountID` – financial account  
- `Amount_OMR` – amount in OMR (positive values for training simplicity)

### 3.2 Dimension Tables

#### `Dim_Date`

- `Date`  
- `Year`  
- `Month`  
- `MonthName`  
- `YearMonth` (e.g. `2024-01`)

#### `Dim_Company`

- `CompanyID`  
- `CompanyName`  
- `Region`

#### `Dim_CostCenter`

- `CostCenterID`  
- `CostCenterName`  
- `CompanyID`

#### `Dim_Program`

- `ProgramID`  
- `ProgramName`  
- `ProgramCategory` (Digital, Customer, Efficiency, Growth, Compliance, People)

#### `Dim_Account`

- `AccountID`  
- `AccountName`  
- `AccountCategory` (Revenue / OPEX / CAPEX)

---

## 4. Retail Sales Dataset Structure

**File:** `Retail_Sales_Performance_Training_Data.xlsx`  
**Grain:** One row per Date–Store–Product–Channel–(optional Promotion)

### 4.1 Fact Table

#### `Fact_Sales`

- `SalesID` – unique row ID  
- `Date` – transaction date  
- `StoreID` – store where sale occurred  
- `ProductID` – product sold  
- `ChannelID` – Store / Online  
- `PromotionID` – promotion applied (nullable)  
- `Quantity` – units sold  
- `NetSales_OMR` – net sales value after discounts (OMR)  
- `COGS_OMR` – cost of goods sold (OMR)

### 4.2 Dimension Tables

#### `Dim_Date`

- `Date`  
- `Year`  
- `Month`  
- `MonthName`  
- `Day`  
- `DayOfWeek`  
- `IsWeekend`  
- `WeekOfYear`

#### `Dim_Channel`

- `ChannelID`  
- `ChannelName` (Store / Online)

#### `Dim_Store`

- `StoreID`  
- `StoreName`  
- `City`  
- `Region`  
- `StoreType` (Mall, Neighborhood, Hypermarket, Travel, Warehouse, Fulfillment Center, etc.)  
- `OpeningDate`

#### `Dim_Product`

- `ProductID`  
- `ProductName`  
- `Category` (Grocery - Dry, Fresh Food, Beverages, Home & Cleaning, Personal Care, Electronics)  
- `Subcategory` (Pasta & Rice, Fruit, Soft Drinks, Detergents, Skincare, Small Appliances, etc.)  
- `Brand`  
- `RegularPrice_OMR` – standard price per unit, before discounts

#### `Dim_Promotion`

- `PromotionID`  
- `PromotionName`  
- `PromotionType` (Discount %, Bundle, Weekend Promo, Buy 1 Get 1)  
- `StartDate`  
- `EndDate`

---

## 5. Recommended Data Model (Power BI)

### 5.1 Corporate Finance Model

Suggested relationships (many-to-one, single direction):

- `Fact_Finance[Date]` → `Dim_Date[Date]`  
- `Fact_Finance[CompanyID]` → `Dim_Company[CompanyID]`  
- `Fact_Finance[CostCenterID]` → `Dim_CostCenter[CostCenterID]`  
- `Fact_Finance[ProgramID]` → `Dim_Program[ProgramID]`  
- `Fact_Finance[AccountID]` → `Dim_Account[AccountID]`

This forms a clean **star schema** with `Fact_Finance` at the center.

---

### 5.2 Retail Sales Model

Suggested relationships:

- `Fact_Sales[Date]` → `Dim_Date[Date]`  
- `Fact_Sales[StoreID]` → `Dim_Store[StoreID]`  
- `Fact_Sales[ProductID]` → `Dim_Product[ProductID]`  
- `Fact_Sales[ChannelID]` → `Dim_Channel[ChannelID]`  
- `Fact_Sales[PromotionID]` → `Dim_Promotion[PromotionID]`  

This also forms a star schema with `Fact_Sales` at the center.  
You can keep the Promotion relationship active or experiment with inactive relationships and `USERELATIONSHIP` if you want to turn this into a more advanced modeling exercise.

---

## 6. How to Use This Repo

1. **Clone or download** the repository.
2. Open **Power BI Desktop**.
3. For each project:
   - `Get Data` → `Excel` → select the relevant file.  
   - Load all tables (Fact + Dimensions + Data_Dictionary if you want reference).  
   - Build the relationships as described above.  
   - Create your own measures and visuals on top of the model.
4. Use these projects for:
   - Internal Power BI training  
   - Workshops / bootcamps  
   - Portfolio / GitHub showcase  
   - Self-study practice  

Feel free to fork and extend (add more years, more channels, additional facts like Targets, Inventory, etc.) if you want to turn this into a broader BI demo environment.

