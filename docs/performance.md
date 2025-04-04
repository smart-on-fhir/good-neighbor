---
title: Performance & Quality
parent: Home
hide_hero: true
nav_order: 1
permalink: /performance/
---

# BulkUP - the Bulk Data User Partnership

The SMART team has previously performed a 
[survey of performance of bulk FHIR](https://pmc.ncbi.nlm.nih.gov/articles/PMC11031206/) 
across five sites and three software implementations. The results showed that bulk FHIR performance varied significantly across implementations. In practice, slow bulk performance can mean that it is impractical to investigate your population-scale data, despite the promise of bulk FHIR of being able to do just that.

But implementations are always changing, and there are certainly more implementations to test. That's where you come in - measure the performance and data quality of your Bulk FHIR interface using the open source CumulusQ tools and share your results with the community!

<hr class="mt-6 mb-6">

<h3>
    <span class="icon has-text-info">
        <i class="icon fas fa-question-circle"></i>
    </span> How to submit your logs to SMART
</h3>
Please use this form to request that your bulk FHIR API experience be seen by the community. Feel free to describe both interoperability issues and success stories. Someone from the community will verify and reach out to the email provided to obtain more information. Verified issues and stories will be listed publicly and can be updated based on vendor engagement.

<a class="button is-info" href="https://docs.google.com/forms/d/e/1FAIpQLSd9GVWs6VyW8nfSLOd2p75HPQNbCpLwd2YP__TaC9BAY1Ukwg/viewform" target="_blank">Submit Your Logs</a>


<hr class="mt-6 mb-6">
<div class="columns is-8">
<div class="column" markdown=1>

## CumulusQ Performance
CumulusQ Performance is an effort to gather bulk data performance logs from as many different
sites and vendors as possible. This will help us understand the challenges and limitations of the current crop of bulk export implementations, in a variety of environments.

SMART bulk export clients generate a 
[standard log format](https://github.com/smart-on-fhir/bulk-data-client/wiki/Bulk-Data-Export-Log-Items)
that has interesting data about the timing of the export and how many resources were exported. (It holds no sensitive or personal data.)

<h3>
    <span class="icon has-text-info">
        <i class="icon fas fa-question-circle"></i>
    </span> How to generate export logs
</h3>
All you have to do is use one of the SMART bulk data tools (read more about them on the [Bulk Data Sandboxes](sandboxes.md) page) and there should be a `log.ndjson` file sitting in the data download folder (or several files with names like `log*.ndjson`), next to your other NDJSON files.

</div>

<div class="column" markdown=1>

## CumulusQ USCDI
CumulusQ USCDI is a collection of data metrics that make it easier to surface interesting trends in your FHIR data, from conformance issues to code coverage to demographic information.

SMART is interested in collecting data metrics from as many different sites as possible.
This will help us understand the FHIR conformance challenges experienced by different vendors, how widespread certain standardized code systems are, and the general shape of FHIR data in the wild.

<h3>
    <span class="icon has-text-info">
        <i class="icon fas fa-question-circle"></i>
    </span> How to generate the data metrics
</h3>
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

</div>
</div>
