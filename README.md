# Introduction

Welcome to the **Data Engineering Practice Repository**! Here, you’ll find a variety of **hands-on projects** designed to help **data engineers** practice and sharpen their skills. Each project focuses on key aspects of the data engineering lifecycle: ingestion, transformation, orchestration, data modeling, and more.

### What’s in This Repo?

- **Multiple Project Scenarios**: Each with instructions, goals, data sources, and sample code.
- **General Guidance & Best Practices**: Covering testing, CI/CD, code quality, and environment setup.
- **Room for Contribution**: This repo is under active development—feel free to suggest improvements, add new datasets, or propose enhancements via pull requests.

### Next Steps

- **Explore the Projects**: Dive into each folder or guide to see instructions on how to set up and run them.
- **Check the Guidance**: See below for general notes on testing, linting, dev containers, etc.
- **Contribute**: We welcome contributions! Open a pull request or create an issue for bugs, improvements, or additional documentation.

> **Note**: We’ll be updating this repository with more instructions and datasets regularly. If you want to help us enhance the repo—through new data, tutorials, or code fixes—your contributions are greatly appreciated!

---

## General Guidance

1. **Testing Python Code with `pytest`**

   - **Objective**: Ensure your Python scripts (ETL, data transformations, utilities) work as intended and remain stable under changes.
   - **Best Practices**:
     - Write tests that cover both expected and edge cases.
     - Organize tests in a `tests/` directory, each test file named `test_*.py`.
     - Keep tests small and focused on one function or behavior.
     - Run tests frequently (locally, in CI) to catch regressions early.

2. **Integration Tests with `docker-compose`**

   - **Objective**: Validate interactions between your application/code and external services (e.g., Postgres, Spark, APIs) in a realistic environment.
   - **Best Practices**:
     - Use a `docker-compose.yml` file to define containers for databases, data stores, or third-party services.
     - Spin up containers for the test environment, run your integration tests, then tear everything down.
     - Keep these tests separate from unit tests, as they may be slower or more resource-intensive.

3. **Code Quality: Linting and Formatting**

   - **Objective**: Maintain consistent coding style and prevent common mistakes or code smells.
   - **Best Practices**:
     - Use a **linter** (e.g., **Flake8**, **Pylint**) to detect issues (undefined variables, unused imports).
     - Use a **formatter** (e.g., **Black**, **Autopep8**) to standardize indentation, spacing, and line lengths.
     - **Always** run linting/formatting—either locally before committing or via pre-commit hooks—to ensure code consistency across the team.

4. **Git Pre-Commit Hooks**

   - **Objective**: Automate checks that run _before_ code is committed, preventing errors or style violations from entering the repo.
   - **Best Practices**:
     - Use a framework like **pre-commit** to configure hooks for linting, formatting, or security checks.
     - Hooks reject commits that fail lint rules or formatting.
     - This ensures code always meets minimum quality standards.

5. **Always Deploy Using CI/CD (with Tests & Integration Tests)**

   - **Objective**: Automate the build, test, and deployment process so code changes are verified and released quickly and confidently.
   - **Best Practices**:
     - Configure pipelines (e.g., GitHub Actions, GitLab CI, Jenkins) to run unit tests, lint checks, and integration tests.
     - Fail the pipeline if any check or test fails.
     - Deploy only after passing all stages, ensuring robust releases.

6. **Try Mocking Tests**

   - **Objective**: In tests, replace (or “mock”) external services (APIs, databases) with controlled “fake” responses, keeping tests fast and isolated.
   - **Best Practices**:
     - Use mocking libraries (e.g., `unittest.mock`, `pytest-mock`) to emulate external dependencies.
     - Verify your logic without making real network calls or changing real data.
     - Combine mocking tests (fast, isolated) with integration tests (real environment) for thorough coverage.

7. **Use Dev Containers (Devcontainer)**

   - **Objective**: Standardize your development environment so every team member (including CI systems) uses the same dependencies, tools, and configurations.
   - **Best Practices**:
     - Create a **`.devcontainer/`** folder with a **`Dockerfile`** or reference to a base image that includes Python, necessary libraries, and lint/test tools.
     - Configure VS Code (or another IDE) to use the dev container automatically, ensuring consistent local dev and easier onboarding.
     - Sync the dev environment with your CI environment so the same versions of Python, libraries, and system dependencies are used everywhere.

8. **Always Put Linting and Formatting Front and Center**
   - **Objective**: From the moment you set up a project, ensure code style is enforced consistently to avoid “style drift” and merge conflicts.
   - **Best Practices**:
     - Document your lint and format processes (e.g., “Run `black . && flake8 .` before committing”).
     - Integrate them into your dev container configuration, pre-commit hooks, and CI.
     - Use a standard config file (like `pyproject.toml` for Black) so the entire team (and CI) uses the same style rules.

---

### Summary

By following these **general practices**, junior developers can:

- **Improve reliability**: Automated tests (unit + integration) catch issues early.
- **Enhance maintainability**: Standardized linting/formatting keeps code readable.
- **Ensure consistency**: Git pre-commit hooks and dev containers align local and CI environments.
- **Optimize development flow**: Quick feedback loops (via mocking and CI/CD) let you iterate rapidly without unexpected production issues.

Incorporate these steps into each data engineering project—whether building ETL pipelines, analytics workflows, or machine learning models—to maintain **high-quality, scalable, and resilient** solutions.

---

# Projects

## 1) COVID-19 Case Data Cleansing & Modeling

### Business Goal

- **Objective**: Provide **daily and cumulative** COVID-19 case, recovery, and death counts to assist **medical facilities** in resource planning (e.g., hospital bed allocation).
- **Key Insight**: By having a cleansed, modeled dataset, public health officials can easily see **new daily cases** and **hospitalization trends**.

### Data & Dataset Description

- **Data Source**:
  - [Johns Hopkins CSSE COVID-19 Data](https://github.com/CSSEGISandData/COVID-19)
  - [Kaggle COVID-19 Datasets](https://www.kaggle.com/datasets?search=covid)
- **Data Format**: Multiple CSVs with daily counts by region (cases, deaths, recoveries).
- **Data Details**: Inconsistent country/state naming, negative corrections, and partial data for some dates.

### Expectations

1. **Ingest**
   - Use Python or Airflow to download new daily CSVs (or pull from GitHub).
2. **Cleanse**
   - Convert date columns to a standard YYYY-MM-DD.
   - Standardize location names (e.g., “US” vs. “United States”).
   - Handle missing/negative values.
3. **Model in Postgres**
   - **fact_covid_cases** table with date, region, metrics (cases, deaths, recoveries).
   - **dim_date**, **dim_location** for time-series and region-based analysis.
4. **Orchestrate**
   - An **Airflow DAG** that automates daily ingestion and loading into Postgres.
5. **Simple Dashboard**
   - **Charts**: daily new cases, cumulative metrics, 7-day moving average.
   - **Use Case**: hospital admins see trending cases to plan ICU capacity.

---

## 2) E-Commerce Sales Cleansing & Modeling

### Business Goal

- **Objective**: Enable an e-commerce store to track **revenue, top products**, and **monthly growth** accurately.
- **Key Insight**: Clean, reliable data helps marketing and finance forecast sales and optimize inventory.

### Data & Dataset Description

- **Data Source**:
  - [UCI Online Retail Data](https://archive.ics.uci.edu/ml/datasets/online+retail)
  - [Kaggle E-Commerce Datasets](https://www.kaggle.com/datasets?search=ecommerce)
- **Data Format**: CSV with `InvoiceNo`, `StockCode`, `Description`, `Quantity`, `InvoiceDate`, `UnitPrice`, `CustomerID`.
- **Data Details**: Missing `CustomerID`s, negative quantities (returns), inconsistent date/time formats.

### Expectations

1. **Ingest**
   - Download CSV from UCI/Kaggle, store locally or in cloud.
   - Load into Postgres using Python or an Airflow ingest task.
2. **Cleanse**
   - Fix negative `Quantity` for returns or exclude them if needed.
   - Convert `InvoiceDate` to a standard timestamp.
   - Remove duplicates or missing CustomerIDs where appropriate.
3. **Model in Postgres**
   - **fact_sales**: invoice number, product key, date key, customer key, quantity, total amount.
   - **dim_product**, **dim_date**, **dim_customer** for analytics.
4. **Orchestrate**
   - **Airflow** pipeline for monthly or weekly ingestion and transformations.
5. **Simple Dashboard**
   - **Charts**: daily/weekly revenue, top products, growth rate.
   - **Use Case**: marketing sees product performance, finance monitors revenue trends.

---

## 3) Real Estate Transactions Cleansing & Modeling

### Business Goal

- **Objective**: Help realtors/agencies understand **property price trends** (average sale price, monthly volume) for better market insights.
- **Key Insight**: Standardized views reveal which neighborhoods are “hot” or undervalued.

### Data & Dataset Description

- **Data Source**:
  - [NYC Property Sales](https://www1.nyc.gov/site/finance/taxes/property-annualized-sales-update.page) updated link [NYC New Website] (https://www.nyc.gov/site/finance/property/property-annualized-sales-update.page)
- **Data Format**: CSV with address, sale price, sale date, building class, etc.
- **Data Details**: Potential outliers ($0 sales), partial addresses, inconsistent date formats.

### Expectations

1. **Ingest**
   - Download monthly/annual real estate CSV, store in local/cloud.
   - Load into Postgres staging.
2. **Cleanse**
   - Remove invalid or zero sale prices (unless legitimate).
   - Standardize addresses, fix building class codes, unify sale dates.
3. **Model in Postgres**
   - **fact_property_sales**: sale date, sale price, property ID.
   - **dim_property** (address, building type), **dim_date**, **dim_location** (borough, ZIP).
4. **Orchestrate**
   - **Airflow** DAG for monthly refresh of property sales.
5. **Simple Dashboard**
   - **Charts**: average sale price by neighborhood, monthly sales volume.
   - **Use Case**: realtors identify market trends, plan listings/pricing.

---

## 4) Social Media Insights with DBT (Tracking Sentiment on Elections)

### Business Goal

- **Objective**: Provide **public sentiment** metrics on elections (e.g., US Elections) to inform political campaigns or media outlets.
- **Key Insight**: Understanding how sentiment shifts over time helps tailor messaging or coverage.

### Data & Dataset Description

- **Data Source**:
  - [Kaggle: “U.S. Election 2020 Tweets”](https://www.kaggle.com/datasets/rohanrao/usa-election-2020-tweets)
- **Data Format**: CSV or JSON with tweet text, user handle, timestamp, possibly location.
- **Data Details**: Large volumes, duplicates, spam accounts, various timestamp formats.

### Expectations

1. **Ingest**
   - Pull data from Kaggle, store raw in Postgres.
2. **Cleanse**
   - Remove duplicates, unify timestamps, filter out retweets or spam.
   - (Optional) run sentiment analysis script to add a sentiment column.
3. **Model in Postgres + DBT**
   - **Staging**: parse text fields, unify user IDs.
   - **Marts**: `fact_tweets` with sentiment scores, key hashtags, tweet counts.
4. **Orchestrate**
   - **Airflow**: daily ingestion and DBT transformations.
5. **Simple Dashboard**
   - **Charts**: sentiment vs. time, top hashtags, tweet volume by day.
   - **Use Case**: campaigns or media see trending sentiment on key election topics.

---

## 5) Stock or Financial Data Pipeline with DBT

### Business Goal

- **Objective**: Use Python (pandas, yfinance) and DBT to analyze **daily stock price data** for **top 10 companies** (e.g., AAPL, MSFT, AMZN, GOOGL, etc.), gaining insights into **long-term trends**, **price volatility**, and **potential investment opportunities** over time.
- **Key Insight**: Consolidating historical stock data and running transformations/tests in DBT allows **traders, analysts, or finance teams** to discover patterns, compare performance across companies, and create dashboards that highlight price movements and trading volume.

### Data & Dataset Description

- **Data Source**:
  - [Yahoo Finance](https://finance.yahoo.com/) (using the `yfinance` Python package).
  - Example for Apple (AAPL) from **2010-01-01** to **2020-12-31**, but you can adjust ticker symbols and date ranges for other companies.
- **Data Format**: CSV or direct DataFrame from yfinance, with columns like `Date`, `Open`, `High`, `Low`, `Close`, `Adj Close`, and `Volume`.
- **Data Details**: The dataset spans **daily trading days** (no weekends/holidays), with possible missing dates (holidays). Stock splits, dividends, or corporate actions might need special consideration (adjusted close prices).

Below is a **sample Python script** to download Apple’s daily stock prices from 2010 to 2020 using `yfinance` and save them to a CSV:

```python
import yfinance as yf

ticker = "AAPL"  # Example: Apple Inc.
start_date = "2010-01-01"
end_date = "2020-12-31"

# Download historical stock prices
stock_prices_data = yf.download(ticker, start=start_date, end=end_date)

# Save to CSV
stock_prices_data.to_csv("stock_prices_data.csv")
```

To analyze multiple companies, you can loop through a list of **ticker symbols** (e.g., `["AAPL", "MSFT", "AMZN", "GOOGL", "FB", "TSLA", "NVDA", "JPM", "V", "WMT"]`) and combine their data in one CSV or separate files.

### Expectations

1. **Ingest**

   - Write a Python script (or an Airflow task) using `yfinance` to download daily stock data for each of your **top 10 companies** over a chosen date range (e.g., 2010–2020).
   - Save each company’s data as a CSV, or combine them with a “ticker” column for a unified dataset.
   - Load the CSV(s) into a **PostgreSQL** staging table.

2. **Cleanse**

   - Standardize date formats (ensure `Date` is in `YYYY-MM-DD`).
   - Handle or flag missing values on non-trading days.
   - Adjust for stock splits or dividends if you want more precise historical comparisons (often done via the `Adj Close` column).

3. **Model in Postgres + DBT**

   - **Staging Models**: unify all companies’ data, rename columns consistently (`open`, `close`, `volume`, etc.).
   - **Marts**: Create a `fact_stock_prices` table (columns: `date`, `ticker`, `open`, `high`, `low`, `close`, `volume`).
   - Implement DBT tests for data integrity (e.g., no null `date`, unique `(date, ticker)` pairs).

4. **Orchestrate**

   - Use **Airflow** to schedule the Python ingestion script daily/weekly.
   - Trigger **DBT** transformations to update staging and mart tables, ensuring new data is reflected.

5. **Simple Dashboard**
   - **Charts**:
     - Line chart comparing **closing prices** of the 10 tickers over time.
     - A bar/line chart of **daily trading volume** or **monthly average** volume by ticker.
     - Moving averages (e.g., 50-day, 200-day) for each stock to identify trends.
   - **Use Case**: financial analysts can quickly see which company outperforms others over certain timeframes, evaluate volatility, or identify potential investment signals.

---

Below is the **replacement project** (originally "Product Analytics with DBT") that now focuses on the **US Weather Events (2016–2022)** dataset. It follows the same structure as the other projects.

---

## 6) US Weather Events (2016–2022) with DBT

### Business Goal

- **Objective**: Build a data pipeline to ingest and analyze **8.6 million weather events** occurring from 2016 to 2022 across the United States, enabling a deeper understanding of long-term weather patterns and potential impacts on various sectors (transportation, agriculture, city planning).
- **Key Insight**: By cleaning and modeling event data (e.g., rain, snow, fog, storms) in a structured way, **meteorologists**, **transportation agencies**, or **city planners** can detect trends (e.g., increased storm frequency), identify seasonal severity shifts, and allocate resources more effectively based on historical evidence.

### Data & Dataset Description

- **Data Source**:
  - [US Weather Events (2016–2022) on Kaggle](https://www.kaggle.com/datasets/sobhanmoosavi/us-weather-events) (1.09 GB CSV).
  - Contains weather records from **2,071 airport-based stations** across 49 states, encompassing **8.6 million events**.
- **Data Format**: CSV with columns such as `EventId`, `Type` (Rain, Snow, Fog, etc.), `Severity`, `StartTime(UTC)`, `EndTime(UTC)`, `Precipitation(in)`, `TimeZone`, `AirportCode`, `LocationLat`, `LocationLng`, etc.
- **Data Details**:
  - Events span January 2016 to December 2022 (UTC).
  - Event Types include **Rain, Snow, Fog, Storm, Hail, Severe-Cold, Other Precipitation**.
  - **Severity** can be Light, Moderate, or Severe.
  - Precipitation in inches, sometimes zero or with outliers.
  - Latitude/longitude identify the weather station location.

### Expectations

1. **Ingest**

   - Download the large CSV from Kaggle (1.09 GB).
   - Store it locally or in cloud storage (e.g., S3).
   - Use **Python** or **Airflow** to read the CSV in manageable chunks (due to size) and load the raw data into a **PostgreSQL** staging table.

2. **Cleanse**

   - Convert/standardize date/time fields (`StartTime(UTC)`, `EndTime(UTC)`) into a uniform **timestamp**.
   - Validate severity fields, remove or flag rows with invalid severity.
   - Check for anomalous precipitation values (e.g., excessively high inches).
   - Handle missing or ambiguous event types (like “Other Precipitation”) by labeling them consistently.

3. **Model in Postgres + DBT**

   - **Staging Models**: unify column names, parse timestamps, ensure consistent data types.
   - **Marts**:
     - `fact_weather_events` with all details (event type, severity, start/end times, precipitation).
     - Optionally, `dim_station` capturing `AirportCode`, latitude/longitude, time zone, etc.
   - Consider an **incremental** DBT model if loading monthly or yearly segments.
   - Include **DBT tests** (e.g., check no null `event_type`, valid date ranges, lat/lng within the US).

4. **Orchestrate**

   - Use **Airflow** to schedule the pipeline (e.g., monthly or a one-time historical backfill).
   - DAG tasks: (1) ingestion → (2) DBT staging → (3) DBT marts → (4) data quality checks or notifications.

5. **Simple Dashboard**
   - **Charts**:
     - Count of events by type (rain, snow, storm) over time.
     - Severity distribution (light vs. severe) by season or region.
     - Average precipitation by month/year across different states or stations.
   - **Use Case**:
     - **Transportation agencies** forecast resource needs for highway maintenance during storm seasons.
     - **Local governments** track frequency of severe-cold or dense fog events to plan emergency services.
     - **Researchers** analyze long-term weather shifts, potentially linking them to climate change indicators.

> **Note**: The dataset is released under the **CC BY-NC-SA 4.0 license** for **research/non-commercial** use. If utilized in published research, cite the paper “Short and Long-term Pattern Discovery Over Large-Scale Geo-Spatiotemporal Data” by Moosavi et al. (2019).

---

## 7) Spark-Based Log Processing with Parquet

### Business Goal

- **Objective**: Analyze **web server access logs** at scale to identify traffic spikes, error patterns, and popular resources for an e-commerce platform.
- **Key Insight**: DevOps and business stakeholders can leverage insights from logs to **optimize site performance**, detect **security issues**, improve **crawlability** by search engines, and enhance **user experience** through data-driven decisions.

### Data & Dataset Description

- **Data Source**:
  - [Web Server Access Logs on Kaggle](https://www.kaggle.com/datasets/eliasdabbas/web-server-access-logs) (approx. 3.3GB).
  - Logs from an Iranian e-commerce website (zanbil.ir), including IPs, timestamps, HTTP requests, status codes, user agents, etc.
- **Data Format**:
  - **Nginx** server access logs in standard text format, with lines resembling:
    ```
    54.36.149.41 - - [22/Jan/2019:03:56:14 +0330] "GET /filter/... HTTP/1.1" 200 30577 "-" "Mozilla/5.0 (compatible; AhrefsBot/6.1; +http://ahrefs.com/robot/)" "-"
    ```
- **Data Details**:
  - Typical log fields: IP address, date/time, HTTP method, endpoint/URL, status code, response size, referrer, user agent.
  - May contain **bot traffic** (Googlebot, Bingbot, AhrefsBot), real user sessions, and possible error codes (404, 500, etc.).
  - The dataset is large (3.3GB), requiring chunked ingestion or distributed processing.

### Expectations

1. **Ingest**

   - Download the **compressed logs** (if provided) from Kaggle.
   - Store them in a local directory or in HDFS/S3 for large-scale processing.
   - Use **Spark** (PySpark/Scala) to **read** the log lines—possibly with a custom log parsing library or regex.

2. **Cleanse**

   - Parse each line to extract columns: IP, timestamp, request path, HTTP status code, response size, user agent.
   - Filter out malformed lines or extremely large/out-of-range values.
   - Optionally categorize known bot traffic or exclude it if you want only human visitor insights.

3. **Write to Parquet**

   - Convert the parsed DataFrame into a structured schema (e.g., `ip STRING`, `timestamp TIMESTAMP`, `method STRING`, `endpoint STRING`, `status INT`, `bytes INT`, `user_agent STRING`).
   - **Partition** by date or status code for more efficient queries (e.g., partition column = `date`).
   - Write the final result in **Parquet** format, which supports columnar storage and compression.
   - (Optional) Summarize data (daily aggregates, top endpoints) in a second Parquet dataset or load aggregates into Postgres for quick queries.

4. **Orchestrate**

   - Use **Airflow** or a similar scheduler to automate this process daily or weekly (especially if logs are continuously generated).
   - Tasks might include: (1) fetch logs → (2) run Spark parsing → (3) write Parquet → (4) (optional) load aggregates into a database for further analytics.

5. **Simple Dashboard**
   - **Charts**:
     - Request volume over time (hourly/daily).
     - Distribution of HTTP status codes (e.g., 2xx, 3xx, 4xx, 5xx).
     - Top requested endpoints or URLs.
     - Bot vs. human traffic ratio if distinguished.
   - **Use Case**:
     - **DevOps** monitors traffic spikes, peak hours, or error surges (e.g., a sudden rise in 500 errors).
     - **Marketing/SEO** teams analyze how search engine bots crawl the site, ensuring important pages are indexed.
     - **Security** can investigate suspicious IPs or unusual user agent patterns indicative of an attack.

---

## 8) End-to-End Data Lake (Generic Implementation)

### Business Goal

- **Objective**: Establish a **scalable, cost-efficient** repository for large datasets (e.g., NYC Taxi or any open data) to enable **ad-hoc analytics** and historical trend exploration without the overhead of a traditional data warehouse.
- **Key Insight**: Analysts, data scientists, and stakeholders can **query historical data on demand**, quickly producing insights about usage trends, performance, or customer behaviors—supporting more agile and data-driven decisions.

### Data & Dataset Description

- **Data Source** (Examples):
  - [NYC Taxi Trip Records](https://www1.nyc.gov/site/tlc/about/tlc-trip-record-data.page) for ride data,
  - [data.gov](https://catalog.data.gov/) open datasets (e.g., city or federal data).
- **Data Format**: Often CSV or Parquet, potentially **gigabytes to terabytes** in size.
- **Data Details**:
  - Datasets might be released **monthly** or **quarterly**.
  - **Partitioning** by date (or region, category) is crucial for performance.
  - Data can reside in **cloud object storage** (S3, GCS, Azure Blob) or on-premises (HDFS, MinIO, etc.).

### Expectations

1. **Ingest**

   - Store **raw** files (CSV, JSON, etc.) in a **landing zone** (e.g., a cloud bucket or on-prem file system).
   - For NYC Taxi data, you might have monthly CSV files named `yellow_tripdata_2023-01.csv`, `yellow_tripdata_2023-02.csv`, etc.
   - Use Python scripts, CLI tools, or an automated scheduler (e.g., Airflow) to handle new file arrivals.

2. **Cleanse & Curate**

   - Use a **Spark** or **PySpark** job (or any ETL tool) to read raw CSV, fix data types, handle outliers, and enrich or filter records.
   - Convert the cleansed data into **partitioned Parquet** in a **curated zone** (e.g., partition by year, month).
   - This step drastically **improves query performance** and compression.

3. **Orchestrate**

   - Employ **Airflow**, **Luigi**, or another workflow manager to schedule tasks:
     - Download or detect new monthly data
     - Run the Spark/ETL job for cleansing and partitioning
     - Update metadata in a data catalog (e.g., Hive Metastore, AWS Glue Catalog, or another service)
   - Monitor job runs and logs for any failures or data anomalies.

4. **(Optional) Model in a Relational DB**

   - If needed, load aggregated or frequently accessed summaries (e.g., daily ridership totals, average fares) into **PostgreSQL** or another relational database.
   - This can expedite dashboards that need quick responses on smaller, aggregated datasets.

5. **Simple Dashboard & Business Queries**
   - **Query Engine**: Use **Presto**, **Hive**, **Trino**, **Spark SQL**, or a cloud-native service (Athena, BigQuery, Synapse) to run SQL queries on the partitioned Parquet data.
   - **Dashboard**: Connect a BI tool (e.g., Tableau, Power BI, Metabase) to the query engine.
   - **Business Insights** might include:
     - **Ridership Patterns**: “Which days of the week or hours of the day see the highest taxi usage?”
     - **Revenue Analysis**: “What is the average fare or tip amount by month?”
     - **Trip Distance Trends**: “Are trip lengths changing over time? Which routes are most common?”
     - **Geospatial Insights**: “Which neighborhoods generate the most rides, and how does that change seasonally?”
   - **Use Case**:
     - **City planners** identify peak hours to manage traffic or adjust public transport.
     - **Taxi companies** see demand variations, plan driver schedules, and optimize resource allocation.
     - **Policy makers** evaluate infrastructure needs based on ridership trends.

> **Tip**: The core concept is to create **Raw → Curated → Analytics** zones in your data lake, ensuring a **modular** approach. This end-to-end pipeline is adaptable to any **cloud** platform (AWS, Azure, GCP) or **on-prem** system (Hadoop ecosystem) for large-scale data management.

---

## 9) Spark-Based Historical Tracking (Dimension Changes over Time)

### Business Goal

- **Objective**: Maintain **full historical records** of changing **electronic product details** (e.g., prices, availability, merchant, condition) to provide accurate point-in-time analysis for pricing strategies and inventory management.
- **Key Insight**: By storing **every change** rather than overwriting records, teams can analyze how product prices/conditions evolved over time, correlate with market trends, and respond to competitor pricing strategies effectively.

### Data & Dataset Description

- **Dataset**:
  - [Electronic Products and Pricing Data](https://www.kaggle.com/datasets/datafiniti/electronic-products-prices) (Provided by Datafiniti)
  - This dataset has **15,000+ electronic products** with up to 10 fields of pricing info, including `prices.amountMax`, `prices.condition`, `prices.dateSeen`, etc.
- **Recommendation**:
  - Download the **full dataset** (~192 MB in CSV) for a substantial volume of updates.
  - Note that this is still a _sample_ of a much larger dataset available from Datafiniti.
- **Data Format**:
  - CSV with ~30 columns, e.g.:
    - `id` (unique product identifier),
    - `prices.amountMin/Max` (price ranges),
    - `prices.condition` (new, used, etc.),
    - `prices.dateSeen` (date/time the price was recorded),
    - `prices.merchant`, `prices.availability`, etc.
- **Data Details**:
  - A single `id` might appear multiple times with different `prices.amountMax`, `prices.condition`, or `prices.dateSeen`, reflecting **price changes** or updated merchant offerings.
  - We want to keep each historical entry as a **point-in-time** snapshot.

### Expectations

1. **Ingest**

   - Periodically retrieve or download the updated CSV from Kaggle (e.g., monthly).
   - Store it in a **landing zone** (local or cloud).
   - Use **Spark** to read the file; consider chunking if it grows larger.
   - If incremental updates are provided separately, ingest only the new or changed records.

2. **Cleanse**

   - **Validate** `id` (no duplicates within the same batch unless truly representing a new snapshot).
   - Convert `prices.dateSeen` to a proper **timestamp** or date for sorting and comparisons.
   - Clean up missing or illogical data (e.g., negative prices, malformed date strings).

3. **Write to Parquet** with Historical Tracking

   - Each row in the dataset can represent a **moment in time** for that product’s price/condition.
   - Instead of overwriting existing records for a product, **append** new entries to keep a full history.
     - If `prices.dateSeen` indicates a new snapshot for the same `id`, store it as a separate row.
   - **Partition** the final Parquet dataset by `dateSeen` (e.g., monthly partitions) or by product category for efficient queries.
   - This approach is analogous to an **SCD Type 2** pattern, where each price change is a new row with its own effective date.

4. **Orchestrate**

   - Use **Airflow** (or another scheduler) to run these steps automatically:
     1. Detect or download the newest CSV from the Kaggle dataset.
     2. Launch a **Spark** job to parse, cleanse, and append to the historical Parquet table.
     3. (Optional) Load aggregated data (e.g., daily average price per product) into Postgres or a warehouse for quick queries.

5. **Simple Dashboard**
   - **Charts** you might build:
     1. **Price Trend Over Time**: For a specific product `id`, show how `prices.amountMin`/`amountMax` changed week by week.
     2. **Condition vs. Price**: Compare `prices.condition` (new, used, refurbished) with average price by date or merchant.
     3. **Merchant Competition**: For a common product, see how multiple merchants priced the item over time (`prices.merchant`).
   - **Use Case**:
     - **E-commerce analysts** can see historical pricing to decide if they should match or undercut competitor prices.
     - **Inventory planners** observe how availability (in-stock, out-of-stock) correlates with price changes over time.
     - **Finance or marketing teams** study the effect of seasonal changes (holiday or back-to-school) on product pricing.

---

> **Note**: Since the Kaggle file is a sample, you can practice **incremental** or **historical** loads by **splitting** the data by dateSeen (or by merchant). The key is to treat each row as a snapshot in time. This ensures you retain **all** historical changes for advanced analytics, rather than overwriting older rows whenever a price update occurs.

---

## 10) Basic Machine Learning Pipeline Integration

### Business Goal

- **Objective**: Predict **customer churn** to help **telecom providers** improve **retention** strategies or cross-sell opportunities. By identifying at-risk customers early, the company can proactively offer promotions or interventions.
- **Key Insight**: Storing model outputs (e.g., churn probabilities) in Postgres (or a data warehouse) lets **business teams** quickly query which customers are at highest risk and compare predicted outcomes to actual churn behavior, enabling **data-driven** retention and pricing strategies.

### Data & Dataset Description

- **Data Source**:
  - [Telco Customer Churn (IBM)](https://www.kaggle.com/datasets/blastchar/telco-customer-churn) on Kaggle
  - CSV file: `WA_Fn-UseC_-Telco-Customer-Churn.csv` (~977.5 kB)
- **Data Format & Columns**:
  - **customerID**: Unique customer identifier (e.g., “7590-VHVEG”)
  - **gender**: Male/Female
  - **SeniorCitizen**: 0 = Not a senior, 1 = Senior
  - **Partner**: Yes/No (whether customer has a partner)
  - **Dependents**: Yes/No
  - **tenure**: Number of months the customer has stayed with the company (range 0–72)
  - **PhoneService**: Yes/No
  - **MultipleLines**: Yes/No/No phone service
  - **InternetService**: DSL/Fiber optic/No
  - **OnlineSecurity**: Yes/No/No internet service
  - … (additional columns like MonthlyCharges, TotalCharges, Contract, PaymentMethod, Churn)
- **Data Details**:
  - Some columns may have **missing** or **null** values.
  - Categorical variables (Yes/No/Fiber optic) need **encoding** (e.g., one-hot).
  - **Outliers** in numeric columns (e.g., unusually high monthly charges) might need removing or capping.
  - **Churn** (Yes/No) is the target variable.

### Expectations

1. **Ingest**

   - **Download** the CSV from Kaggle and store locally or in a cloud storage.
   - Load data into a **Postgres** staging table (e.g., `stg_telco_churn`).
   - Optionally split into **training** and **test** sets if modeling offline.

2. **Cleanse**

   - Handle **missing** data (imputation or dropping rows).
   - **Encode** categorical fields (e.g., one-hot for `gender`, `Partner`, `InternetService`).
   - **Detect and remove** or transform extreme outliers (in `MonthlyCharges`, `TotalCharges`).
   - Ensure data types are consistent (e.g., `tenure` is numeric, `SeniorCitizen` is boolean/int).

3. **Model in Postgres**

   - Store the cleaned/engineered dataset back to Postgres (e.g., `fact_churn_features`).
   - **Train** a simple classification model (e.g., logistic regression) in a Python script or notebook.
   - Generate **predictions** or **churn probabilities**.
   - **Write** those predictions back to a **Postgres** table (e.g., `fact_churn_predictions`) for easy querying by analysts or BI tools.

4. **Orchestrate**

   - Use **Airflow** (or another scheduler) to automate the process:
     1. **Data Prep**: run a PythonOperator to read the CSV, clean & encode, store in Postgres.
     2. **Model Training**: run a Python script or a container that retrains the logistic regression (or other model) periodically.
     3. **Predictions**: once trained, generate new churn predictions for each customer.
     4. **Load & Notify**: store updated predictions in Postgres, optionally notify a Slack channel or email if churn risk is high.

5. **Simple Dashboard**
   - **Charts**:
     - **Churn Rate** by segment (gender, SeniorCitizen, InternetService type).
     - **Predicted vs. Actual** churn confusion matrix or ROC curve.
     - **Top Features** that influence churn (if using a model that can provide feature importance).
   - **Use Case**:
     - Marketing or retention teams can **filter** customers with high churn probability, offering them targeted promotions.
     - Executives can **track overall churn trend** month-to-month and evaluate if interventions are working.
     - Product teams see whether **bundle services** or certain contract types reduce churn risk.

---

