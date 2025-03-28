---
title: Performance & Quality
parent: Home
hide_hero: true
nav_order: 1
---

# BulkUP - the Bulk Data User Partnership

## Bulk FHIR Performance in the Wild
The SMART team has previously performed a 
[survey of performance of bulk FHIR](https://pmc.ncbi.nlm.nih.gov/articles/PMC11031206/) 
across five sites and three software implementations. The results showed that bulk FHIR performance varied significantly across implementations. In practice, slow bulk performance can mean that it is impractical to investigate your population-scale data, despite the promise of bulk FHIR of being able to do just that.

But implementations are always changing, and there are certainly more implementations to test. That's where you come in - measure the performance and and data quality of your Bulk FHIR interface using the open source CumulusQ tools and share your results with the community! 

---
## Submit Bulk Export Logs
SMART bulk export clients generate a 
[standard log format](https://github.com/smart-on-fhir/bulk-data-client/wiki/Bulk-Data-Export-Log-Items)
that has interesting data about the timing of the export and how many resources were exported. (It holds no sensitive or personal data.)

SMART is interested in collecting these bulk export performance logs from as many different sites as possible. This will help us understand the challenges and limitations of the current crop of bulk export implementations, in a variety of environments.

### How to generate export logs
All you have to do is run a bulk export using one of the SMART tools (bulk-data-client or cumulus-etl) and there should be a log.ndjson file sitting in the data download folder (or several files with names like log*.ndjson), next to your other NDJSON files.

### How to submit your logs to SMART
TODO: Form

---
## Submit CumulusQ Metrics
CumulusQ Metrics are a collection of data metrics that make it easier to surface interesting trends in your FHIR data, from conformance issues to code coverage to demographic information.

SMART is also interested in collecting data metrics from as many different sites as possible. This will help us understand the FHIR conformance challenges experienced by different vendors, how widespread certain standardized code systems are, and the general shape of FHIR data in the wild.

### How to generate the data metrics
These metrics are designed as a 
[Cumulus Library](https://docs.smarthealthit.org/cumulus/library/)
study. They can operate on small amounts of local data, but you will be able to run metrics for your entire population if you set up the entire 
[Cumulus](https://docs.smarthealthit.org/cumulus/) pipeline in AWS. For now, we'll focus on the local data approach.

Assuming you have some exported bulk NDJSON sitting in the folder `./downloads`:

```sh
pipx install cumulus-library
pipx inject cumulus-library cumulus-library-data-metrics

# Compile the metrics
cumulus-library build \
  --db-type duckdb \
  --database metrics.db \
  --target data_metrics \
  --load-ndjson-dir ./downloads

# Export summarized metric reports
cumulus-library export \
  --db-type duckdb \
  --database metrics.db \
  --target data_metrics \
  ./reports
```
This will create a local file `metrics.db` that holds all the calculated metrics based on your data
and a `reports/` directory that holds summaries of the results, in both CSV and parquet forms.

### How to submit your logs to SMART
TODO: Form
