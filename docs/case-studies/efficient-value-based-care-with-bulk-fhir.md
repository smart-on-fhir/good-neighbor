---
title: Efficient Value Based Care with Bulk FHIR
parent: Case Studies
hide_hero: true
nav_order: 2
permalink: /case-studies/efficient-value-based-care-with-bulk-fhir
---
# Efficient Value Based Care with Bulk FHIR
<hr class="mb-6"/>

<a href="https://www.providence.org/" target="_blank">Providence</a> has more than 150 Value-Based Care (VBC) contracts. To be successful within VBC, it is imperative that we as a provider organization share supplemental clinical quality and coding data with payer partners.

When we evaluated utilizing the output of Epic’s (g)(10) APIs, we saw an approximate 40% increase in quality gap closure when compared to legacy processes that combined data from 27 tables from Epic’s Clarity data warehouse. We implemented a chatty process where data thousands of individual API calls are made, and data for each required FHIR resource is retrieved individually, and then packaged as bulk for export to the payer. This entire process can be simplified with a Bulk API.

Before we decided to pull data out of Epic one patient and resource at a time, we tested Epic’s bulk API for a single patient who was seen by our health system over the course of many years. We found the following:
- A bulk group had to be set up manually using Epic’s frontend application. This involved working with an internal Epic analyst.
    - The other option was to leverage an Epic registry. This did not make sense in the world of VBC as enrolled members can vary month to month and the registry process requires manual interference.
- After we set up the bulk group, the query ran for three days without a response for a single patient. 
- We determined that an individual resource retrieval/chatty process was a better solution than to pull the data using Epic’s bulk API
- We continue to advocate for improvements to Epic bulk functionality.  

<br />

#### Proposal:

- Create a process to utilize the FHIR Member Attribution (ATR) Group representing a contract between a payer/provider  or a liquid parameter where an array of FHIR IDs can be passed through to define the population and to kick-off for a bulk call. This would remove the administrative burden around manually creating groups or using registries and instead involve the following steps:
    1. Payer shares an enrolled/attributed list of patients (ATR FHIR IG) that is then received by the provider
    2. The provider would then perform patient matching and append a FHIR ID
    3. The provider organization would kick off the bulk query. Supporting an array of FHIR IDs to create an ad hoc group would also work.
  This would make the process more fluid and remove the administrative burden around manually creating groups or using registries.
- Create an infrastructure to support faster response times and much larger population sizes.
    - As Clarity is to flat data – Something equivalent needs to exist on the FHIR side  as a baseline for Bulk. This could be a FHIR-native repository or FHIR mapping framework on top of Clarity and perhaps as an extension with the Clarity Dictionary as a way for providers and vendors to transform the data with confidence.

There are many industry initiatives whose success depends on bulk exports from an EHR working:
- NCQA’s Bulk FHIR coalition – If the goal is for payers to have more readily-available access to data for HEDIS measures, then Bulk needs to work. Otherwise, the burden around chart chasing persists.
- Value-Based Care analytics for providers – We know that the data exported from (g)(10) APIs creates a more comprehensive picture of a patient’s chart than legacy information available through back-end databases. The bulk export can be used to support more real-time analytics to promote proactive decision making.
- Provider systems benefit from creating an API marketplace – especially if they are integrating data from ACO participants. This can be done through individual queries – However, it is extremely taxing, and a bulk export coupled with regular delta loads creates an opportunity for everyone in the care network to have access to the <b><u>right</u></b> data to support patient care, outreach, and administrative functions.
- Through the IPPS RFI we saw that CMS is looking to further standardize quality measure reporting. It was asked whether providers see an opportunity to implement FHIR IGs like DEQM (Data Exchange for Quality Measures) to streamline quality measure reporting – And whether it would be feasible within 24 months. If this is truly the future and HPNs (High Performance Networks) and CINs (Clinically Integrated Networks) continue to become more commonplace – Then it is in every provider’s interest for Bulk to work. Long gone are the days where a single instance of an EHR can be trusted as the source of truth.

While we have the ability to use the (g)(10) APIs in a chatty process and still gain from the potential to close additional quality gaps, paint a more comprehensive picture of a patient’s care journey, integrate data more smoothly in a repository, and build an API marketplace to better engage payers and providers in our markets – It is evident that this process is not scalable. There are too many individual components each provider would have to implement just for data acquisition purposes. It is also very taxing and burdensome on providers to build alternative processes for data integration when the burden can be greatly eliminated by optimizing Bulk APIs that adhere to specific SLAs, support easier ways to identify the population, and improving response times. The work put forth in this effort will have a tremendous impact on Value-Based Care success for both payers and providers alike.


<br />
