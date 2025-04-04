---
title: EHR Vendor Tips & Tricks
parent: Home
hide_hero: true
nav_order: 1
permalink: /tips/
---

# Vendor Tips & Tricks

## Epic
---
<h4>
    <span class="icon has-text-info">
        <i class="icon fas fa-info-circle"></i>
    </span> Connecting to the Sandbox
</h4>

[Community documentation](sandboxes.md#epic)

<br />

<h4>
    <span class="icon has-text-info">
        <i class="icon fas fa-info-circle"></i>
    </span> Defining a Group
</h4>

FHIR Groups are implemented as Epic registries.

Work with your institution's Epic support team to define a registry for your cohort of
patients, and then connect that registry to a FHIR Group and to your application.

<br />

<h4>
    <span class="icon has-text-info">
        <i class="icon fas fa-info-circle"></i>
    </span> Frequency
</h4>

Epic limits how frequently you can perform a bulk export for the same group & client ID.
By default, once every 24 hours.

You may see error messages like:
```
Error processing Bulk Data Kickoff request: Request not allowed: The Client requested this Group too recently.
```

You may need to request that the `FHIR_BULK_CLIENT_REQUEST_WINDOW_TBL` setting be changed.

<br />

<h4>
    <span class="icon has-text-info">
        <i class="icon fas fa-info-circle"></i>
    </span> Export Size
</h4>

Epic recommends no more than 1,000 patients should be exported at once.
Bulk export times can really balloon as the size of the export increases.

Here are some strategies to reduce the amount of data you are exporting:
- Export one FHIR resource at a time
  - Just Conditions: `_type=Condition`
- For resources with a lot of entries (like Observation),
  you might additionally slice by category
  - Just laboratory Observations: a type filter of `Observation?category=laboratory`
  - Or in full and URL-encoded: `_type=Observation&_typeFilter=Observation%3Fcategory%3Dlaboratory`
- Export only a window of time for a given resource.
  Most resources have some sort of date-based search parameter you can use.
  - Just 2023 Encounters: a type filter of
  `Encounter?date=ge2023-01-01T00:00:00Z&date=lt2024-01-01T00:00:00Z`
  - Or in full and URL-encoded: `_type=Encounter&_typeFilter=Encounter%3Fdate%3Dge2023-01-01T00:00:00Z%26date%3Dlt2024-01-01T00:00:00Z`

<br />

## Oracle
---
<h4>
    <span class="icon has-text-info">
        <i class="icon fas fa-info-circle"></i>
    </span> Connecting to the Sandbox
</h4>
[Community documentation](sandboxes.md#cerner)

<br />

<h4>
    <span class="icon has-text-info">
        <i class="icon fas fa-info-circle"></i>
    </span> Defining a Group
</h4>

FHIR Groups are defined as a list of patient IDs to include in the group.
These Groups have a maximum of 20,000 patients, at the time of writing. 

Work with your institution's Oracle support team to define the group based off of
your cohort of patients.

<br />

<hr class="mt-6 mb-6">
<a class="button is-info" href="https://github.com/smart-on-fhir/good-neighbor/blob/main/docs/tips.md" target="_blank">Share a Tip</a>