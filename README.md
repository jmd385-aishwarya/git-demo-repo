# Pipeline Analytics
Pipeline Analytics is a form of business performance analysis focused on understanding the process of how potential customers move from initial contact to making a purchase. Pipeline Analytics outputs provide visibility of key metrics related to pipeline to drive revenue growth and operational efficiency through better decision making and strategic planning. Core visuals and metrics that breakdown the key elements affecting the sales pipeline, include pipeline coverage to target, lead conversion rates, sales cycle length, win/loss ratios, and forecast accuracy across a given time frame.

## Table of Contents
- [Overview](#overview)
- [Versions](#versions)
- [Requirements](#requirements)
- [Usage](#usage)
- [Integration Steps](#integration-steps)
- [Features](#features)
- [Data Modelling](#data-modelling)
- [Troubleshooting](#troubleshooting)
- [Contributing](#contributing)
- [Contact](#contact)

## Overview
Pipeline Analytics provides visibility into how potential customers move through the sales process — from first contact to purchase — enabling data-driven decisions that drive revenue growth and operational efficiency.
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
|              | Old Version | Latest Version |
|:-------------|:------------|:---------------|
|Source|Hubspot|Dynamics365|
|important models|accounts, opportunity_line_item, opportunity, opportunity_history, stage_mapping| target, pipeline_stage_history, opportunity |
|Business Units|not compatible for multiple business units|scompatible for multiple business units|

## Requirements
- python 3.7 or higher
- dbt-core (package for dbt core)
- dbt-snowflake (adapter for snowflake warehouse)

## Integration Steps
### Step 1
Explain how to install or clone the repo.

## Configuration
Show config examples.

## Usage
Explain how to run it with code blocks.

## Features
Use bullet points for features.

## Data Modelling
Pipeline Analytics, a data model serves as the structured foundation for organizing and managing sales pipeline data. It defines how key entities such as customers, leads, opportunities, products, and sales activities are interconnected and represented in a database. This model ensures that data is captured, processed, and analyzed consistently, enabling accurate measurement of metrics like lead conversion rates, sales velocity, and win/loss ratios.
_Data Model Diagram_ : [Data Diagram](https://lucid.app/lucidchart/b4ec1fe4-1f6f-49bd-852b-590a6e623bc4/edit?page=G7OlA_oX8Xjb#)

## Troubleshooting
List common issues and fixes.

## Contributing
Add contribution steps.

## Contact
List maintainers and links.


# sales_pipeline_analytics



## 📝 Description

This project focuses on Sales Pipeline Analytics. Leveraging a robust tech stack, it delivers key insights into your sales process. While the specific technologies used are not listed here, the system's features include comprehensive tracking of leads, automated pipeline stage management, and predictive analytics to forecast sales outcomes. This allows for data-driven decision-making, optimized resource allocation, and ultimately, increased sales conversion rates.

## 📁 Project Structure

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
