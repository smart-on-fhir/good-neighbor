---
title: Re-imagining Public Health Monitoring with Bulk FHIR
parent: Case Studies
hide_hero: true
nav_order: 2
permalink: /case-studies/re-imagining-public-health-monitoring-with-bulk-fhir
---
# Re-imagining Public Health Monitoring with Bulk FHIR
<hr class="mb-6"/>

In early 2023, as part of the <a href="https://academic.oup.com/jamia/article-abstract/31/8/1638/7690920" target="_blank">SMART Cumulus project</a>, a federally-funded initiative aimed at advancing healthcare interoperability by creating scalable, standards-based data-sharing frameworks—Washington University School of Medicine began implementing Bulk FHIR to demonstrate enhanced, automated public health monitoring capabilities. The SMART Cumulus platform leverages advanced health information technology standards, including FHIR and associated APIs, to support federated analytics across multiple institutions. Federated monitoring, facilitated by distributed queries through standardized interfaces, promised substantial improvements over traditional methods, especially addressing challenges encountered in broad surveillance efforts during the COVID-19 pandemic. Bulk FHIR appeared ideally suited to this task, given its capacity to handle population-level data efficiently and support timely data queries critical for public health surveillance and AI-driven analytics. However, practical experience with our Bulk FHIR implementation revealed significant technical limitations stemming from the current implementations within an electronic health record (EHR) system, hindering its immediate utility and necessitating interim workarounds.

<br/>
##### Experience with Bulk FHIR Implementation
The Washington University Epic EHR includes Bulk FHIR capability to meet certification requirements specified in the ASTP/ONC 21st Century Cures Act Final Rule. The IT team sought to use this, and encountered technical difficulties which forced us to pursue workarounds until they were eventually fixed in a subsequent update. Even then, there were significant performance and reliability issues and we ended up using batched FHIR calls for individual patients instead of bulk export for the project. Normally, the creation of a registry with standard data types is straightforward, but requires some configuration and validation. This classic problem was comparatively easy, but the issues with Bulk FHIR made it difficult to validate. Our experience highlighted the need for more robust and performant Bulk FHIR implementations to prevent institutions from engineering inefficient solutions for a defined standard.

<br/>
##### Practical Challenges and Workarounds
Numerous obstacles were faced due to the limitations of the EHR's Bulk FHIR implementation. Specifically:
- **Functionality**: The Bulk FHIR technology did not work “out of the box”, and required both substantial configuration and updates to finally function. 
  - Configuration for the initial Bulk FHIR request took 119 calendar days to start producing production data.
  - A second cohort test was initiated at that time, which took 53 days to produce a subset of the requested data.
- **Performance**: The EHR-implemented Bulk FHIR technology struggled to handle large datasets efficiently. 
  - For a population of around 1,200 patients, the relatively small number of medications and diagnoses were able to be exported in a few hours. 
  - However, the server took over a day to provide the patients’ 3 million lab measurements, and was unable to provide any clinical notes or FHIR “Encounter” data models, both of which contain information of great interest to public health. 
  - Tests on the second, larger cohort of 4,200 patients indicated a linear scaling of export times with the number of patients, introducing a limiting factor for rapid population health analyses.
- **Workaround Necessity**: Instability and performance challenges required us to use substantial workarounds. 
  - Of note, many of the errors encountered were not factors on comparison tests using the single-patient SMART on FHIR (REST) API, which–exporting data one patient at a time–was able to make complete exports on the same populations in a quarter of the time, despite the inefficiency of making a multitude of separate HTTP requests.

Despite these challenges, a functional, though suboptimal, solution was implemented. However, this experience underscored the critical need for more mature and performant Bulk FHIR implementations by EHR vendors.
 
<br/>
##### Future Directions for Bulk FHIR and AI Integration
Looking ahead, the necessity for efficient population-level data retrieval from EHRs via Bulk FHIR is becoming increasingly critical, particularly with the growing adoption of AI solutionsInstances were encountered where AI applications demanded analysis of datasets exceeding the capacity of current internal EHR AI capabilities, involving thousands of variables. As further AI opportunities are explored and the automation of AI tools and model drift monitoring is sought, the need for large-scale data access will only increase.

Ultimately, a mature and well-performing Bulk FHIR interface is viewed as a vital approach to significantly improving data efficiency when interacting with the Washington University EHR system. Efficient population-level data retrieval via Bulk FHIR is essential for supporting advanced AI applications and enhancing public health monitoring at WashU Medicine.


<br />