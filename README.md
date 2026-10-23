# Weather & Demographic Data Pipeline

An end-to-end data engineering project that combines weather information
with demographic data, transforms the datasets using Python, stores
processed records in Google BigQuery, and presents insights through SQL
analysis and a Looker Studio dashboard.

## Objectives

-   Collect weather data from a weather API.
-   Integrate demographic data with weather information.
-   Clean and transform datasets using Python and Pandas.
-   Store and query processed data in Google BigQuery.
-   Analyze temperature patterns using SQL.
-   Visualize insights in Looker Studio.
-   Keep the workflow organized and reproducible.

## Tech Stack

  Technology        Purpose
  ----------------- --------------------------------------------
  Python            Data ingestion and workflow orchestration
  Pandas            Data cleaning, transformation, and merging
  Requests          Calling the weather API
  Google BigQuery   Cloud data storage and analytical queries
  SQL               Data exploration and analysis
  Looker Studio     Dashboard and visual reporting
  Streamlit         Optional local dashboard
  Git and GitHub    Version control and project hosting
  Pytest            Automated testing

## Project Structure

``` text
weather-demographic-pipeline/
├── app.py                  # Optional Streamlit dashboard
├── requirements.txt        # Python dependencies
├── README.md               # Project documentation
├── .gitignore               # Excluded files and local data
├── scripts/
│   └── main.py             # Main data-ingestion workflow
├── src/
│   ├── pipeline.py         # Pipeline execution entry point
│   └── transform.py        # Data cleaning and transformation
├── sql/                    # SQL queries and analysis
├── tests/
│   └── .gitkeep            # Placeholder for tests
└── data/                   # Local input and generated data (may be gitignored)
```

*The structure above describes the intended layout. The actual files in
the repository may differ.*

## Data Sources

### Weather data

Weather information is collected from a weather API using Python's
`requests` library. The available fields depend on the API response and
the implementation in `scripts/main.py`.

### Demographic data

The demographic dataset comes from the bootcamp's training resources:

[Training Bootcamp Assignment
Resources](https://github.com/yashhh0001/Training_Bootcamp_Assignment_Resources/tree/main)

### Data integration

Weather and demographic records are cleaned and matched using shared
location identifiers, such as city and country where available. The
final coverage depends on the records returned by the API and the
quality of the location matches.

## Data Pipeline

``` text
Weather API ──────────┐
                      ├──> Data ingestion
Demographic data ─────┘
                             |
                             v
                       Data cleaning
                             |
                             v
                    Data transformation
                             |
                             v
                  Weather + demographic merge
                             |
                             v
                      Processed dataset
                       /            \
                      v              v
               Google BigQuery   Local CSV output
                      |
                      v
                  SQL analysis
                      |
                      v
                Looker Studio
                   dashboard
```

### Pipeline stages

1.  **Ingestion:** Fetch weather information and load demographic data.
2.  **Cleaning:** Standardize location identifiers and prepare the input
    datasets.
3.  **Transformation:** Convert fields into a consistent structure for
    analysis.
4.  **Integration:** Merge weather and demographic records using
    location-based keys.
5.  **Export:** Save processed data locally and upload the relevant data
    to BigQuery.
6.  **Analysis:** Query the processed information using SQL and a
    BigQuery view.
7.  **Visualization:** Connect BigQuery to Looker Studio.

## Data Coverage and Validation

Configured BigQuery resources:

-   **Project ID:** `weather-demographic-bootcamp`
-   **Dataset:** `global_weather_data`
-   **Table:** `current_weather_status`
-   **Analytical view:** `vw_hottest_large_countries`

Coverage depends on the source datasets, API response, and successful
matching between locations.

Recommended validation checks:

-   Identify missing location, temperature, or demographic values.
-   Confirm numeric fields have appropriate data types.
-   Check for duplicate city-country combinations.
-   Verify that merge keys match the intended locations.
-   Confirm temperature categories follow the configured thresholds.
-   Verify the BigQuery upload and row count.
-   Re-run the pipeline to check reproducibility.

### Temperature categories

  Category   Rule
  ---------- --------------------
  Cold       Below 10°C
  Mild       10°C to below 25°C
  Hot        25°C and above

*Add measured row counts, geographic coverage, and validation results
after checking the actual pipeline output.*

## Google BigQuery Queries

### 1. Count records

``` sql
SELECT
  COUNT(*) AS total_records
FROM
  `weather-demographic-bootcamp.global_weather_data.current_weather_status`;
```

### 2. Inspect the analytical view

``` sql
SELECT *
FROM
  `weather-demographic-bootcamp.global_weather_data.vw_hottest_large_countries`
LIMIT 100;
```

### 3. Count temperature categories

Run this query only if `Temperature_Category` exists in the selected
table:

``` sql
SELECT
  Temperature_Category,
  COUNT(*) AS location_count
FROM
  `weather-demographic-bootcamp.global_weather_data.current_weather_status`
GROUP BY
  Temperature_Category
ORDER BY
  location_count DESC;
```

*Confirm the actual column names in BigQuery before running queries that
reference specific fields.*

## SQL Analysis

The view `vw_hottest_large_countries` is intended to support analysis of
hotter locations among larger countries. Further analysis could compare
temperatures by location, examine category distributions, and identify
missing or inconsistent records.

Document specific findings only after checking the actual query results.

## Looker Studio Dashboard

[**Open the Weather & Demographic
Dashboard**](https://datastudio.google.com/u/0/reporting/7a58b0e9-1f3d-4e7c-8e51-1a1f255d97da/page/bXzBG/edit)

The dashboard uses BigQuery as a reporting source. Depending on its
configuration and available fields, it can include:

-   A geographic map of available locations.
-   Temperature values represented with a color gradient.
-   A temperature-category filter, if the category field is available.
-   Interactive charts and shareable reporting.

Dashboard features and access depend on the report configuration and
sharing permissions.

## Project Features

-   Organized Python ingestion and transformation workflow.
-   Combined weather and demographic data for analysis.
-   BigQuery storage and SQL-based exploration.
-   Reusable analytical view.
-   Looker Studio reporting.
-   Optional Streamlit dashboard.
-   Version-controlled source code and documentation.

## Future Improvements

-   Schedule regular pipeline runs using a cloud scheduler.
-   Store historical observations for trend analysis.
-   Add automated data-quality checks and Pytest tests.
-   Improve logging, error handling, and failure notifications.
-   Expand location coverage and data sources.
-   Add more SQL analysis and dashboard filters.
-   Use GitHub Actions for automated checks and tests.
-   Record verified row counts and analysis results in this README.

## How to Run the Project

### 1. Clone the repository

``` bash
git clone https://github.com/KathakaliNath/weather-demographic-pipeline.git
cd weather-demographic-pipeline
git checkout feature/data-ingestion
```

### 2. Install dependencies

``` bash
python -m pip install -r requirements.txt
```

### 3. Run the pipeline

``` bash
python src/pipeline.py
```

The pipeline requires the expected input data, valid API configuration,
and appropriate Google Cloud/BigQuery permissions.

### 4. Run the optional Streamlit dashboard

``` bash
streamlit run app.py
```

The processed CSV expected by `app.py` must exist locally. Install the
dependencies first if Streamlit is not installed.

## Notes

-   **Credentials:** Never commit API secrets, service-account keys, or
    other sensitive credentials to GitHub.
-   **Local data:** Files excluded by `.gitignore` may need to be
    supplied separately.
-   **Cloud costs:** BigQuery usage may incur charges depending on
    billing configuration and query volume.
-   **Reproducibility:** Confirm that the API, input files, BigQuery
    resources, and permissions are available before running the full
    workflow.

## Project Links

-   **Repository:**
    [weather-demographic-pipeline](https://github.com/KathakaliNath/weather-demographic-pipeline/tree/feature/data-ingestion)
-   **Dashboard:** [Looker Studio
    Report](https://datastudio.google.com/u/0/reporting/7a58b0e9-1f3d-4e7c-8e51-1a1f255d97da/page/bXzBG/edit)
