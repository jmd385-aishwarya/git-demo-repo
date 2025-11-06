# Pipeline Analytics
Pipeline Analytics is a form of business performance analysis focused on understanding the process of how potential customers move from initial contact to making a purchase. Pipeline Analytics outputs provide visibility of key metrics related to pipeline to drive revenue growth and operational efficiency through better decision making and strategic planning. Core visuals and metrics that breakdown the key elements affecting the sales pipeline, include pipeline coverage to target, lead conversion rates, sales cycle length, win/loss ratios, and forecast accuracy across a given time frame.

## Table of Contents
- [Overview](#overview)
- [Versions](#versions)
- [Requirements](#requirements)
- [Usage](#usage)
- [Configuration](#configuration)
- [Project Structure](#-project-structure)
- [Features](#features)
- [Data Modelling](#data-modelling)
- [Contributing](#contributing)
- [Contact](#contact)

## Overview
Pipeline Analytics provides visibility into how potential customers move through the sales process from first contact to purchase — enabling data-driven decisions that drive revenue growth and operational efficiency.
- ### Key Metrics
    - Pipeline coverage
    - Lead conversion rates
    - Sales cycle length
    - Win/loss ratios
    - Forecast accuracy
- ### Core Benefits
    - Assesses sales performance to target across multiple levels (rep, team, business unit, company).
    - Identifies drivers of forecasted performance and areas for improvement.
    - Supports strategic decision-making for leadership and operational teams.
    - Enables Exit Prep and Value Creation projects to demonstrate sustainable growth.
    - Uncovers cross-sell opportunities and performance enhancement initiatives.
- ### Stakeholders and Use Cases
    - **C-Suite:** Strategic planning, board reporting, and target forecasting.
    - **Sales Teams:** Track opportunities, conversion rates, and pipeline health.
    - **Marketing Teams:** Measure campaign impact and lead quality.
    - **Financial Analysts:** Evaluate revenue forecasts and financial health.
    - **Product Managers:** Align offerings with market demand and customer feedback.

## Versions
The difference between the old and the latest versions are: 
|                |  Old Version                                                                   | Latest Version                              |
|:---------------|:-------------------------------------------------------------------------------|:--------------------------------------------|
|Source          |Hubspot                                                                         |Dynamics365                                  |
|important models|accounts, opportunity_line_item, opportunity, opportunity_history, stage_mapping| target, pipeline_stage_history, opportunity |
|Business Units  |not compatible for multiple business units                                      |scompatible for multiple business units      |

## Requirements
- python 3.7 or higher
- dbt-core (package for dbt core)
- dbt-snowflake (adapter for snowflake warehouse)

## Configuration
### 1. Clone the Github repo locally
### 2. Update `sources.yml`
Modify the `sources.yml` file to ensure proper source mapping for your project's data warehouse.
```yaml
version: 2
sources:
  - name: your_source_name # hubspot or dynamics365
    descriptrion : give_description # give the relevant description
    database: PIPELINE_ANALYTICS 
    schema: RAW
    tables:
      - name: your_table_name  # Specify the relevant tables
```
### 3. Update `dbt_project.yml`
Update project settings for compatibility with your environment.
```yaml
name: 'pipeline_analytics_dbt'
version: '1.0.0'
config-version: 2

profile: 'your_profile'  # Update with the relevant dbt profile
source-paths: ["models"]
target-path: "target"
clean-targets:
  - "target"
  - "dbt_modules"
```
### 4. Configure `profiles.yml`
Ensure your connection settings match your environment.
```yaml
pipeline_analytics_dbt:
  outputs:
    dev: # update these with relevant credentials/data
      account: your_account
      authenticator: externalbrowser
      database: PIPELINE_ANALYTICS
      schema: DBO
      role: your_user_role
      threads: 4
      type: snowflake
      user: your_username
      warehouse: COMPUTE_WH
  target: dev
```
### 5. Update source configuration file
Make sure your source configuration seed file references the correct source, table_name, source and destination column names and datatypes.
```csv
source,table_name,source_column_name,dest_column_name,column_name_with_data_type
dynamics_365,ACCOUNT,ACCOUNTID,ACCOUNTID,ACCOUNTID::VARCHAR
dynamics_365,ACCOUNT_TYPE,LOADED_AT,LOADED_AT,LOADED_AT::Timestamp_NTZ
...
```
### 6. Update Source in macro
Ensure that you update the macro `generate_dynamics_table` with your source configuration seed file name.
```sql
    {% set source_columns = get_reference_data('your_source_configuration_file', source_name, table_name) %} -- update with the correct source configuration seed file name
```

## Features
Use bullet points for features.

## Data Modelling
Pipeline Analytics, a data model serves as the structured foundation for organizing and managing sales pipeline data. It defines how key entities such as customers, leads, opportunities, products, and sales activities are interconnected and represented in a database. This model ensures that data is captured, processed, and analyzed consistently, enabling accurate measurement of metrics like lead conversion rates, sales velocity, and win/loss ratios.

_Data Model Diagram_ : [Data Diagram](https://lucid.app/lucidchart/b4ec1fe4-1f6f-49bd-852b-590a6e623bc4/edit?page=G7OlA_oX8Xjb#)


## Contributing
Add contribution steps.

## Contact
List maintainers and links.

## Project Structure

```
Pipeline_Analytics_dbt_Project
├── macros
│   ├── default
│   │   └── generate_schema_name.sql
│   ├── source_specific
│   │   └── dynamics365.sql
│   ├── utils
│   │   └── common.sql
│   └── macros.yml
└── models
    ├── 01_standardization
    │   ├── models
    │   │   ├── account.sql
    │   │   ├── account_type.sql
    │   │   └── ....
    │   └── sources.yml
    ├── 02_core
    │   ├── models
    │   │   ├── account.sql
    │   │   ├── calendar.sql
    │   │   ├── lead.sql
    │   │   └── ....
    │   └── sources.yml
    ├── 03_curated
    │   ├── models
    │   │   ├── dim_account.sql
    │   │   ├── dim_calendar.sql
    │   │   └── ....
    │   └── sources.yml
    └── sources.yml

```            

## 👥 Contributing

Contributions are welcome! Here's how you can help:

1. **Fork** the repository
2. **Clone** your fork: `git clone https://github.com/jmd385-aishwarya/sales_pipeline_analytics/tree/main/pipeline_analytics_sql-main.git`
3. **Create** a new branch: `git checkout -b feature/your-feature`
4. **Commit** your changes: `git commit -am 'Add some feature'`
5. **Push** to your branch: `git push origin feature/your-feature`
6. **Open** a pull request

Please ensure your code follows the project's style guidelines and includes tests where applicable.
