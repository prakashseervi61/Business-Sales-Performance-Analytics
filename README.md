# Business Sales Performance Analytics

An end-to-end sales analytics project that cleans raw transaction data in Python,
loads it into a MySQL database, and visualizes key business metrics in a Power BI
dashboard.

## Project Structure

```
.
├── data/
│   └── sales_data.csv   # Raw sales transactions (1,000 records)
├── sales.ipynb          # Jupyter notebook: exploration, cleaning, DB load
├── sales.pbix           # Power BI dashboard
└── README.md
```

## Dataset

`data/sales_data.csv` contains 1,000 sales transactions with the following fields:

| Column | Description |
| --- | --- |
| `CustomerID` | Unique customer identifier |
| `Region` | Sales region (East, West, North, South; some missing) |
| `Sales` | Transaction sales amount (some missing) |
| `Discount` | Discount percentage applied (some missing) |
| `Date` | Transaction timestamp (currently in `MM:SS.s` format) |
| `ProductID` | Unique product identifier |
| `Quantity` | Number of units sold |
| `Return` | Return flag (0/1) |
| `CustomerType` | New / Returning / VIP |
| `SalesChannel` | Online / In-Store / Direct |
| `PaymentMethod` | Credit Card / Bank Transfer / Cash |
| `SatisfactionScore` | Customer satisfaction rating (1–10) |
| `ProductCategory` | Product category (Electronics, Sports, Books, etc.) |

## Data Cleaning (sales.ipynb)

The notebook performs exploratory analysis and the following cleaning steps:

- Loads and inspects the data with `head()`, `info()`, `describe()`, and null checks.
- Fills missing `Region` values using the most frequent region per `CustomerID`.
- Fills missing `Sales` values with the category median, then the overall median.
- Fills missing `Discount` values with `0.0`.
- Loads the cleaned dataframe into a MySQL table named `sales_data`.

> Note: `Date` parsing is currently commented out in the notebook because the
> source values use a `MM:SS.s` format that standard datetime conversion cannot
> parse. This needs to be resolved before time-based analysis.

## Setup

Prerequisites:

- Python 3.x with `pandas` and `jupyter`
- `sqlalchemy` and `pymysql` (installed in the notebook via `%pip install`)
- A MySQL server with a database named `business_sales`
- Power BI Desktop (to open the dashboard)

## Usage

1. Clone the repository and place `sales_data.csv` in the `data/` folder.
2. Create the MySQL database: `CREATE DATABASE business_sales;`
3. Set your database credentials via environment variables
   (`DB_USER`, `DB_PASSWORD`, `DB_HOST`, `DB_PORT`, `DB_NAME`) and update the
   connection string in `sales.ipynb` to read them — avoid hardcoding passwords.
4. Run the notebook cells to explore, clean, and load the data into the
   `sales_data` table.
5. Open `sales.pbix` in Power BI Desktop and connect it to the MySQL database
   to refresh the dashboard.

## Dashboard

`sales.pbix` is the Power BI report built on the cleaned sales data. It covers
sales by region, product category, customer type and channel, discounts,
returns, and satisfaction scores.

## Future Improvements

- Fix `Date` parsing by converting the source format to proper timestamps.
- Move database credentials out of the notebook into environment variables.
- Add automated tests and a reproducible requirements file.
