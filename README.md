# Pipeline Analytics
Pipeline Analytics is a form of business performance analysis focused on understanding the process of how potential customers move from initial contact to making a purchase. Pipeline Analytics outputs provide visibility of key metrics related to pipeline to drive revenue growth and operational efficiency through better decision making and strategic planning. Core visuals and metrics that breakdown the key elements affecting the sales pipeline, include pipeline coverage to target, lead conversion rates, sales cycle length, win/loss ratios, and forecast accuracy across a given time frame.

## Table of Contents
- [Overview](#overview)
- [Versions](#versions)
- [Requirements](#requirements)
- [Usage](#usage)
- [Configuration](#configuration)
- [Project Structure](#-project-structure)
- [Data Modelling](#data-modelling)
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
    {% set source_columns = get_reference_data('your_source_configuration_file', source_name, table_name) %} 
    -- update with the correct source configuration seed file name
```
### 7. Update Model dependencies
Make sure your models reference the correct schemas and tables.
```sql
    {{ generate_dynamics_table("your_source_name", "your_table_name") }}
    -- update with the correct source_name and table_name
```

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
    │   ├── source_name
    │   │   ├── account.sql
    │   │   ├── account_type.sql
    │   │   └── ....
    │   └── sources.yml
    ├── 02_core
    │   ├── ssot
    │   │   ├── account.sql
    │   │   ├── calendar.sql
    │   │   ├── lead.sql
    │   │   └── ....
    │   └── sources.yml
    ├── 03_curated
    │   ├── datamart
    │   │   ├── dim_account.sql
    │   │   ├── dim_calendar.sql
    │   │   └── ....
    │   └── sources.yml
    └── sources.yml
```            

## Data Modelling
Pipeline Analytics, a data model serves as the structured foundation for organizing and managing sales pipeline data. It defines how key entities such as customers, leads, opportunities, products, and sales activities are interconnected and represented in a database. This model ensures that data is captured, processed, and analyzed consistently, enabling accurate measurement of metrics like lead conversion rates, sales velocity, and win/loss ratios.

_Data Model Diagram_ : [Data Diagram](https://lucid.app/lucidchart/b4ec1fe4-1f6f-49bd-852b-590a6e623bc4/edit?page=G7OlA_oX8Xjb#)

**Source**
1. The Source is where the original data and schema are located.

2. Ensure you have the data migrated from your crm to ``snowflake`` or any other platform where you would like to connect your `dbt project` with.

**Transformation Layers / DataFlow**
1. **RAW Layer:**
RAW layer stores unaltered data ingested from CRM systems like HubSpot or Salesforce. It includes tables such as company, contact, opportunity, and opportunity_history, which capture foundational sales pipeline data like company details, contact information, opportunity tracking, and line item mappings. 
Here, we defined appropriate data type for every field and renamed fields.This layer ensures data consistency and serves as the source for downstream transformations and analytics.

2. **SSOT Layer :**
The SSOT (Single Source of Truth) layer in Pipeline Analytics consolidates and transforms raw data from various sources into a structured and reliable format, ensuring accurate insights for business intelligence and analytics. In this layer, key transformations and business logic are applied to ensure data consistency, accuracy, and integrity, serving as the foundation for all downstream analytics.

   _Key models and Their Role in the SSOT Layer:_

   ``account``: This model stores essential account-related details, such as account IDs, types, and other identifying attributes. It helps ensure that all account-related analysis in the pipeline is derived from a single, reliable source. This model provides a consolidated view of accounts across the pipeline, crucial for segmentation and performance tracking.
   
   ``lead``: The lead model captures lead information, including attributes like lead ID, source, and status. It enables tracking from lead generation to qualification and conversion. By consolidating this data, the model supports lead conversion analysis and effectiveness of lead generation strategies.
   
   ``opportunity``: The opportunity model consolidates details about sales opportunities, including opportunity ID, forecast category, and related financial details. This model helps track the progress of sales opportunities through the pipeline, providing a comprehensive view of the sales process.
   
   ``opportunity_line_item``: This model links specific products or services to individual opportunities. It ensures that the sales pipeline reflects detailed product or service-level data, allowing for deeper analysis of the sales performance across product categories and opportunities.
   
   ``pipeline_delta``: The pipeline_delta model calculates changes in the pipeline between periods, using flags to identify shifts in opportunity status or forecast changes. This helps businesses monitor changes in the pipeline, such as new opportunities, lost deals, and closed deals, across time periods.
   
   ``pipeline_period_snapshot``: The pipeline_period_snapshot model creates snapshots of the opportunities at each period end, capturing key data points like stage, status, and forecasted revenue. It ensures that historical performance can be analyzed at various points in time, providing a clear view of pipeline evolution.
   
   ``sales_pipeline``: This model integrates data from the opportunity and opportunity line item models to provide a comprehensive view of the sales pipeline. It consolidates all opportunities, including their associated line items, helping track total pipeline value and performance metrics like conversion rates and sales cycle length.
   
   ``sales_rep``: The sales_rep model contains information about the sales representatives, including ID, region, and performance metrics. It links opportunities and sales data to individual reps, allowing businesses to evaluate rep performance and contribution to pipeline outcomes.

4. **Data_mart**:
   The Gold layer is the final stage of data transformation, optimized for strategic reporting, advanced analytics, and decision-making. It aggregates high-level data, providing key metrics and KPIs to support critical business decisions through dashboards and reports.
   
   In this layer, we begin with the ``dim_account`` model, which gives insights into account performance. We then use ``dim_calendar`` for time-based reporting, followed by ``dim_lead`` for lead conversion and funnel analysis. The ``dim_opportunity`` model aggregates opportunity-related data, while ``dim_product`` connects product performance to opportunities. ``dim_sales_rep`` helps analyze sales rep performance.
   
   The ``fact_sales_pipeline`` model captures overall pipeline metrics, and ``fact_sales_pipeline_conversion`` tracks deal progression through pipeline stages. Lastly, the ``fact_sales_pipeline_movement`` model monitors the movement of opportunities, enabling detailed analysis of pipeline health and velocity.
   
   These models together offer a comprehensive view of the sales pipeline, enabling insightful reporting, trend analysis, and data-driven decision-making.

**Target**
1. The Target is where the changes will be deployed.

2. This could be ``snowflake`` or any other platform where you connected your `dbt project` with.
   
**Outcomes: Build PBI Dashboards for Sales Performance**

By building these dashboards in Power BI, we enable the business to:

1. Visualize key sales pipeline metrics and KPIs.
2. Track sales performance in real-time.
3. Analyze sales team effectiveness, deal conversion rates, and overall pipeline movement.
4. Make data-driven decisions to improve sales strategies, forecast accurately, and optimize resources.

These dashboards are designed to provide a user-friendly interface for stakeholders at various levels of the organization to monitor, analyze, and act upon critical sales insights.

_Pipeline Analytics Dashboards_ : [Github PBI Dashboards](https://github.com/jmangroup/pipeline_analytics_power_bi)

## Contact
For any support contact
- Karthi Sivakumar
- Vishal Verma
- Aishwarya Shree
- Shrinilamangai

