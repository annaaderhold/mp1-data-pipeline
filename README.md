# MP1 Data Pipeline
This pipeline loads, validates, cleans and saves data using the settings put in a YAML config file. "pipeline.py" oversees the workflow and makes sure that the input and config files exist, then it loads them, validates the data, processes it, and then saves the result. The "src/utils.py" sets up the logging and validates the file paths, while the "src/data_validator.py" checks that the required columns exist and then removes the rows that have any invalid numerical values. "src/data_processor.py" removes duplicates, missing values, and any outliers that could exist according to the config, and then builds a cleaning report. "src/data_output.py" writes the cleaned data to a CSV file. The "config" folder holds all of the configuration, and the "fixtures" folder holds the sample data. 

## Example
    python pipeline.py --input fixtures/sample_data.csv --output output/clean.csv --config config/config.yaml --verbose

# Output:
23:41:37 DEBUG    __main__ — Arguments parsed: input=fixtures/sample_data.csv, config=config/config.yaml, output=output/clean.csv
23:41:37 INFO     src.utils — Input file validated: fixtures/sample_data.csv
23:41:37 INFO     src.utils — Input file validated: config/config.yaml
23:41:37 INFO     src.data_loaders — Loaded CSV file: fixtures/sample_data.csv (100 rows)
23:41:37 INFO     src.data_loaders — Loaded YAML file: config/config.yaml
23:41:37 WARNING  src.data_validator — Removed 2 rows with invalid numeric values in rating
23:41:37 DEBUG    src.data_validator — Validation: 100 -> 98 rows
23:41:37 INFO     __main__ — Validation complete: 100 -> 98 rows
23:41:37 DEBUG    src.data_processor — remove_duplicates: 98 → 96 rows
23:41:37 DEBUG    src.data_processor — handle_missing: 96 → 94 rows
23:41:37 DEBUG    src.data_processor — rating: method=iqr, threshold=1.5, lower=43.6, upper=106.6, removed=2
23:41:37 INFO     __main__ — Processing complete: 98 -> 92 rows
23:41:37 DEBUG    src.data_output — Saved 92 rows to output/clean.csv
23:41:37 INFO     __main__ — Saved cleaned data to output/clean.csv
Cleaning report:
{'rows_before': 98, 'rows_after': 92, 'rows_removed': 6, 'columns_before': 5, 'columns_after': 5, 'columns_removed': 0}