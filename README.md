# Red Alert ETL Pipeline

## Overview
This project implements an ETL (Extract, Transform, Load) pipeline for processing Israeli Red Alert (air raid siren) data. The pipeline fetches data from the Oref API, adds geographic region information, and filters by specific regions and dates.

## Features
- Fetches Red Alert data from the Oref API for specified date ranges
- Special handling for high-volume days (like October 7-8, 2023) with optimized time-chunked requests
- Geocodes locations to add region information
- Filters alerts by region and date
- Provides data analysis and statistics
- Exports processed data in multiple formats

## Requirements
- Python 3.6+
- Required packages: pandas, requests, openpyxl

## Getting Started

### Installation
1. Clone this repository:
   ```
   git clone https://github.com/Hagai-BY/red-alert-ETL-pipeline.git
   cd red-alert-etl-pipeline
   ```
2. Install required packages:
   ```
   pip install pandas requests openpyxl
   ```

### Running the Pipeline
Run the main script:
```
python red_alert_etl.py
```

## User Inputs Guide

Throughout the execution of the pipeline, users will be prompted with several Y/N questions to customize the ETL process. Here's what each prompt means:

### DIAGNOSTIC MODE: Force completely clean start? (y/n, default: n)
- `y`: Deletes all existing data files for a clean start
- `n`: Preserves existing files and uses them when possible

### Start at transform stage? (y/n, default: n)
- `y`: Skips the extraction stage and loads existing data files
- `n`: Runs the full pipeline starting with data extraction

### Skip to analysis stage? (y/n, default: n)
- `y`: Skips both extraction and transformation stages, using only the final filtered data file
- `n`: Proceeds with transform stage (and extract if not skipped)

### Do you want to fetch fresh data from the API? (y/n, default: n)
- `y`: Forces fetching new data even if existing files are available
- `n`: Uses cached data if available

### Use custom date range instead of defaults? (y/n, default: n)
- `y`: Allows specifying custom start and end dates
- `n`: Uses default dates (Oct 1, 2023 to Dec 31, 2023)

### Process high-volume days (Oct 7-8, 2023)? (y/n, default: n)
- `y`: Uses special processing with smaller time chunks for the high alert volume days
- `n`: Processes all days with standard time chunks

### Enter chunk size in days (default: 7)
- Allows customizing the size of date chunks for API requests
- Smaller chunks are more reliable but result in more API calls

## Key Functions

### Extract Stage
- `fetch_alerts()`: Retrieves alerts from the Oref API for a given date range
- `fetch_alerts_in_chunks()`: Splits date ranges into manageable chunks for API requests
- `fetch_high_volume_days()`: Special function for high-volume days using hourly time chunks
- `extract_unique_locations()`: Extracts all unique locations from alert data
- `batch_geocode_locations()`: Geocodes locations to determine their geographic region

### Transform Stage
- `add_region_to_alerts()`: Adds region information to each alert
- `translate_regions_to_english()`: Converts Hebrew region names to standardized English names 
- `filter_by_regions()`: Filters alerts to include only specified regions
- `filter_by_dates()`: Filters alerts by specific dates

### Load Stage
- `save_to_csv()`: Exports data to CSV format
- `save_to_excel()`: Exports data to Excel format with optimized column widths

### Analysis
- `analyze_results()`: Provides statistics and analysis on the processed data

## Data Flow
1. Raw alerts are fetched from the Oref API
2. Unique locations are extracted and geocoded
3. Region information is added to each alert
4. Alerts are filtered by region (focusing on central regions)
5. Alerts are filtered by specific dates (66 days)
6. Final data is saved and analyzed

## Output Files
- `merged_alarms_data.csv`: Raw combined alert data
- `israel_cities_regions.xlsx`: City to region mapping data
- `israel_cities_regions_updated.xlsx`: Updated mapping with manual corrections
- `alarms_with_regions.xlsx`: Alerts with region information
- `alarms_with_english_regions.xlsx`: Alerts with standardized English region names
- `central_regions_alarms.xlsx`: Alerts filtered for central regions
- `central_regions_66days_sorted.xlsx`: Final filtered alerts for the 66 specified days

## Notes
- The pipeline includes robust error handling and retry mechanisms
- Special attention is given to encoding for Hebrew text
- The geocoding process uses a combination of OpenStreetMap API and manual mappings
- High-volume days are processed with smaller time chunks to avoid API limitations

## License
[Your License]
