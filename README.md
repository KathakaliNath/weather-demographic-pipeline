**Weather \& Demographic Data Pipeline**

An end-to-end data engineering project that combines weather information

with demographic data, transforms the datasets using Python, stores

processed records in Google BigQuery, and presents insights through SQL

analysis and a Looker Studio dashboard.



&#x20;**Objectives**

\- Collect weather data from a weather API.

\- Integrate demographic data with weather information.

\- Clean and transform datasets using Python and Pandas.

\- Store and query processed data in Google BigQuery.

\- Analyze temperature patterns using SQL.

\- Visualize insights in Looker Studio.

\- Keep the workflow organized and reproducible.



&#x20;**Tech Stack**

&#x20; Technology        Purpose

&#x20; Python            Data ingestion and workflow orchestration

&#x20; Pandas            Data cleaning, transformation, and merging

&#x20; Requests          Calling the weather API

&#x20; Google BigQuery   Cloud data storage and analytical queries

&#x20; SQL               Data exploration and analysis

&#x20; Looker Studio     Dashboard and visual reporting

&#x20; Streamlit         Optional local dashboard

&#x20; Git and GitHub    Version control and project hosting

&#x20; Pytest            Automated testing



&#x20;**Project Structure**

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

The structure above describes the intended layout. The actual files in

the repository may differ.



&#x20;**Data Sources**

**Weather data:**

Weather information is collected from a weather API using Python's

requests library. The available fields depend on the API response and

the implementation in scripts/main.py.

**Demographic data:**

The demographic dataset comes from the bootcamp's training resources:

Training Bootcamp Assignment



&#x20;**Resources**

Data integration

Weather and demographic records are cleaned and matched using shared

location identifiers, such as city and country where available. The

final coverage depends on the records returned by the API and the

quality of the location matches.



&#x20;**Data Pipeline**

Weather API ──────────┐

&#x20;                     ├──> Data ingestion

Demographic data ─────┘

&#x20;                            |

&#x20;                            v

&#x20;                      Data cleaning

&#x20;                            |

&#x20;                            v

&#x20;                   Data transformation

&#x20;                            |

&#x20;                            v

&#x20;                 Weather + demographic merge

&#x20;                            |

&#x20;                            v

&#x20;                     Processed dataset

&#x20;                      /            \\

&#x20;                     v              v

&#x20;              Google BigQuery   Local CSV output

&#x20;                     |

&#x20;                     v

&#x20;                 SQL analysis

&#x20;                     |

&#x20;                     v

&#x20;               Looker Studio

&#x20;                  dashboard



&#x20;**Pipeline stages**

1\. Ingestion: Fetch weather information and load demographic data.

2\. Cleaning: Standardize location identifiers and prepare the input

&#x20;  datasets.

3\. Transformation: Convert fields into a consistent structure for

&#x20;  analysis.

4\. Integration: Merge weather and demographic records using

&#x20;  location-based keys.

5\. Export: Save processed data locally and upload the relevant data

&#x20;  to BigQuery.

6\. Analysis: Query the processed information using SQL and a

&#x20;  BigQuery view.

7\. Visualization: Connect BigQuery to Looker Studio.



&#x20;**Data Coverage and Validation**

Configured BigQuery resources:

\- Project ID: weather-demographic-bootcamp

\- Dataset: global\_weather\_data

\- Table: current\_weather\_status

\- Analytical view: vw\_hottest\_large\_countries

Coverage depends on the source datasets, API response, and successful

matching between locations.



&#x20;**Recommended validation checks:**

\- Identify missing location, temperature, or demographic values.

\- Confirm numeric fields have appropriate data types.

\- Check for duplicate city-country combinations.

\- Verify that merge keys match the intended locations.

\- Confirm temperature categories follow the configured thresholds.

\- Verify the BigQuery upload and row count.

\- Re-run the pipeline to check reproducibility.



&#x20;**Temperature categories**

&#x20; Category   Rule

&#x20; Cold       Below 10°C

&#x20; Mild       10°C to below 25°C

&#x20; Hot        25°C and above



Add measured row counts, geographic coverage, and validation results

after checking the actual pipeline output.



&#x20;**Google BigQuery Queries**

1\. Count records

SELECT

&#x20; COUNT(\*) AS total\_records

FROM

&#x20; `weather-demographic-bootcamp.global\_weather\_data.current\_weather\_status`;

2\. Inspect the analytical view

SELECT \*

FROM

&#x20; `weather-demographic-bootcamp.global\_weather\_data.vw\_hottest\_large\_countries`

LIMIT 100;

3\. Count temperature categories

Run this query only if Temperature\_Category exists in the selected

table:

SELECT

&#x20; Temperature\_Category,

&#x20; COUNT(\*) AS location\_count

FROM

&#x20; `weather-demographic-bootcamp.global\_weather\_data.current\_weather\_status`

GROUP BY

&#x20; Temperature\_Category

ORDER BY

&#x20; location\_count DESC;

Confirm the actual column names in BigQuery before running queries that

reference specific fields.



&#x20;**SQL Analysis**

The view vw\_hottest\_large\_countries is intended to support analysis of

hotter locations among larger countries. Further analysis could compare

temperatures by location, examine category distributions, and identify

missing or inconsistent records.



Document specific findings only after checking the actual query results.



&#x20;**Looker Studio Dashboard**

[Open the Weather \& Demographic Dashboard](https://datastudio.google.com/u/0/reporting/7a58b0e9-1f3d-4e7c-8e51-1a1f255d97da/page/bXzBG/edit)



The dashboard uses BigQuery as a reporting source. Depending on its

configuration and available fields, it can include:

\- A geographic map of available locations.

\- Temperature values represented with a color gradient.

\- A temperature-category filter, if the category field is available.

\- Interactive charts and shareable reporting.

Dashboard features and access depend on the report configuration and

sharing permissions.

&#x20;

**Project Features**

\- Organized Python ingestion and transformation workflow.

\- Combined weather and demographic data for analysis.

\- BigQuery storage and SQL-based exploration.

\- Reusable analytical view.

\- Looker Studio reporting.

\- Optional Streamlit dashboard.

\- Version-controlled source code and documentation.



&#x20;**Future Improvements**

\- Schedule regular pipeline runs using a cloud scheduler.

\- Store historical observations for trend analysis.

\- Add automated data-quality checks and Pytest tests.

\- Improve logging, error handling, and failure notifications.

\- Expand location coverage and data sources.

\- Add more SQL analysis and dashboard filters.

\- Use GitHub Actions for automated checks and tests.

\- Record verified row counts and analysis results in this README.



&#x20;**Project Links**

\- Repository:

&#x20; [weather-demographic-pipeline](https://github.com/KathakaliNath/weather-demographic-pipeline/tree/feature/data-ingestion)

\- Dashboard:

&#x20; [Looker Studio Report](https://datastudio.google.com/u/0/reporting/7a58b0e9-1f3d-4e7c-8e51-1a1f255d97da/page/bXzBG/edit)
