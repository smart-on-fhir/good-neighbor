---
title: Scaling Research Data Access Statewide with Bulk FHIR
parent: Case Studies
hide_hero: true
nav_order: 2
permalink: /case-studies/scaling-research-data-access-statewide-with-bulk-fhir
---
# Scaling Research Data Access Statewide with Bulk FHIR
<hr class="mb-6"/>

<a href="https://www.regenstrief.org/" target="_blank">Regenstrief  Institute’s</a>  Data Services team provides access for research to the Indiana Network for Patient Care, which contains 16 billion data elements from more than 100 hospital systems and tens of thousands of providers across the state. To support research projects with standards-based data models and access, the organization implemented a Bulk FHIR interface on top of a local relational database populated by HL7 V2 messages. Following the approach defined in the Bulk FHIR Access Implementation Guide, upon request the service reads rows from its relational database in bulk, converts them into FHIR, writes them into NDJSON files, and returns URLs for downloading them.

The  implementation was able to leverage existing code mapping the data to a portion of the US Core FHIR profiles and it took only a few weeks to design and implement additional FHIR mappings and a Bulk interface.  The efficient database architecture behind the FHIR interface supports high performance exports, substantially outperforming deployments of the g(10) certified Bulk FHIR interfaces built by Epic and Oracle Cerner.

As seen in Figure 1, the custom-built Regenstrief FHIR Service performed very well, averaging over 11,000 resources per minute over its export, and the Oracle Cerner site averaged over 8,000. On these 3 early Epic Bulk FHIR implementations, export speeds were highly variable on the smaller tests and were observed to approach 2,500 resources per minute on groups larger than 1,000 patients. See more detail in “<a href="https://academic.oup.com/jamia/article/31/5/1144/7623452?guestAccessKey=958af03d-1f2f-4a64-bba8-3387f8d934f9&utm_source=authortollfreelink&utm_campaign=jamia&utm_medium=email&login=true" target="_blank">Real world performance of the 21st Century Cures Act population-level application programming interface</a>”.

<div class="columns is-8 mt-6 mb-1">
    <div class="column is-two-thirds">
        <img src="{{ site.baseurl }}/assets/screenshot1.png" />
    </div>
    <div class="column" markdown=1>

**Figure 1.** Performance scaling differences between vendors. (A) Measured export speed plotted against total number of resources in each benchmark. (B) Total count of resources exported at each site divided by total export time. Performance is grouped by API service provider. R = total number of resources.
</div>
</div>

<br clear="all" />

The success of the Regenstrief Bulk FHIR interface highlights the ability of sophisticated organizations to build on top of open standards to share data. Taking advantage of standards provided by data partners with a well defined <a href="https://hl7.org/fhir/resourcelist.html" target="_blank">data dictionary</a>, standard <a href="https://github.com/smart-on-fhir/cumulus-library-data-metric" target="_blank">data quality and characterization tooling</a>, standard <a href="https://github.com/microsoft/Tools-for-Health-Data-Anonymization" target="_blank">data anonymization tooling</a>, pre-written <a href="https://docs.smarthealthit.org/" target="_blank">software libraries</a> in many languages, and the ability to easily integrate data from diverse sources. The Bulk FHIR export performance that Regenstrief achieved with a relatively small investment in software development highlights the opportunity for EHR vendors with less performant interfaces to improve the FHIR experience for their customers.The Regenstrief Bulk FHIR interface enables interoperability of high quality real-world data from across multiple EHR vendors to advance population health research. 


<br />
