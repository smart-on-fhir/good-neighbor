---
title: Accelerating Clinical Trial Recruitment with Bulk FHIR
parent: Case Studies
hide_hero: true
nav_order: 2
permalink: /case-studies/accelerating-clinical-trial-recruitment-with-bulk-fhir
---
# Accelerating Clinical Trial Recruitment with Bulk FHIR
<hr class="mb-6"/>

### Motivation

<a href="https://www.novel.care/" target="_blank">Novelcare</a> helps healthcare providers offer clinical trials as a treatment option to their patients. New treatment options are constantly under development, and finding patients for these studies can be a challenge for study sponsors. Patients expect their doctors to know about clinical trials, but doctors rarely make referrals because of the amount of work required to do so. Novelcare makes this process simple for doctors. If a nearby study matches a patient’s medical record, Novelcare notifies the doctor and allows them to easily make a referral. 

Novelcare is made possible by Bulk FHIR. Clinical trial criteria can sometimes have upwards of 30 items each patient has to match (“inclusion criteria”) or cannot match (“exclusion criteria”) to be eligible for a study, so this kind of combinatorial query isn’t feasible with the limited FHIR search endpoints and rate limits supported by most ONC Certified EHRs. Instead, Novelcare does a Bulk export to index the medical records for our own matching engine. 

Importantly, a majority of practices using Novelcare are small to medium sized independent outpatient practices. Unlike the hospital market, which is dominated by Epic and Oracle, there are a variety of EHRs used in this market, and Bulk FHIR being a universal standard we can expect across most of these EHRs is what makes Novelcare possible. To date, Novecare has integrations with 9 EHRs, and expects this number to continue to grow.


### Experience with AthenaHealth 

Novelcare’s first EHR integration was on AthenaHealth, and to date AthenaHealth continues to be the most reliable and best performing Bulk FHIR implementation. When we started our business, we knew that interoperability would be make or break for us. We had read the regulation - it sure seemed like we should be able to get the data we needed - but we knew there was a chance for reality to be quite different. Thankfully AthenaHealth’s great implementation worked out of the box for us, and gave us a sense that interoperability was really here.

<br/>

##### Getting started

Poking around AthenaHealth’s docs led us to their helpful <a href="https://docs.athenahealth.com/api/workflows/fhir-bulk-export-workflow" target="_blank">Bulk export workflow page</a>. Even though much of this workflow is already well documented by <a href="https://hl7.org/fhir/uv/bulkdata/export.html" target="_blank">FHIR’s own docs</a>, for a developer new to the FHIR ecosystem, having this workflow laid out clearly in the context of the EHR is a huge help. For example, as required by the ONC, AthenaHealth has a Group export. Many EHRs fail to document how groups are created, but AthenaHealth lays it out plainly:

The first step is to request the export. To do so, you need to know the Practice from which you are requesting data. The `GroupID` to be used in the request is the Practice in the format `a-1.C-{Practice}`
- For example, if requesting data from Practice `195900`, then the GroupID is `a-1.C-195900`
- This provides an export of all patients in the Targeted Practice

Once we had a sense of how Bulk FHIR worked on AthenaHealth, we started developing our integration. Building a Bulk FHIR exporter is simple in theory, but can be complex in practice. Though we have app-specific reasons to have built our own exporter, had we known about SMART-on-FHIR’s <a href="https://github.com/smart-on-fhir/bulk-data-client" target="_blank">bulk-data-client</a>, we would have definitely started here.

<br/>

#### Testing

Once we had a Bulk FHIR client that seemed like it should work, we were ready to start testing. Unfortunately, while AthenaHealth has a handy sandbox environment, it doesn’t support Bulk exports. Thankfully, the SMART team has a publicly accessible <a href="https://bulk-data.smarthealthit.org/" target="_blank">Bulk FHIR reference implementation</a>. This continues to be a huge resource for our team for testing and developing against Bulk FHIR. Using this tool, we were able to ensure our client could handle an end-to-end export efficiently and securely.

<br/>

#### Going to production

Most EHRs have API documentation because it’s required by the ONC. But what many EHRs lack, and AthenaHealth does a great job of, is making it clear how to actually ship an app. Generally, for system clients, this is two steps:
1. Creating the app [in production]
2. Installing the app on the practice’s instance (i.e granting your app access to the practice’s data).

The first step is self explanatory on AthenaHealth. In addition to the clear documentation, there’s a big blue “Go to console” button that takes you to AthenaHealth’s developer portal, where you can follow the prompts to register an app for Sandbox OR production. Many EHRs don’t have a self-serve signup process, so this is a huge help. We’re very excited for the ONC’s recent interest in requiring support for OAuth 2.0 Dynamic Client Registration Protocol (DCRP), which will allow FHIR users like us to programmatically register our app with EHRs that have an obscure registration process (either by intentional obfuscation or by lack of caring).

Once you’ve registered a production app, on many EHRs, you’ve arrived at the hard part. Installing an app is often an undocumented, obscure process that has to happen over email and requires constant follow ups to make happen. Not so on AthenaHealth. At the time we integrated, it took some digging to find the install process, but now it’s well documented <a href="https://docs.athenahealth.com/api/guides/certified-apis#My_app_is_not_a_Patient_Health_Record_app_but_it_will_exclusively_use_Certified_APIs_for_data_exchange._2" target="_blank">here</a>. With one click from our customer, our app had access and we were ready to begin serving our customers.

<br/>

#### Implementation - data quality and performance

As I mentioned in our introduction, AthenaHealth is the best performing Bulk FHIR implementation we’ve seen. To date we’ve had no problems in production. On average, exports on AthenaHealth take around 5-6 hours. We’ve never had an export fail, take more than 24 hours, or get “stuck” (a common problem on some EHRs). 

As far as data quality, it’s excellent. AthenaHealth gives clinical notes in a very useful format and implements the nuances of FHIR well. For example, a common challenge is differentiating prescriptions and medication history in the MedicationRequest resource. On AthenaHealth, they properly use the `reportedBoolean` property to denote this, something we don’t see often enough on other EHRs (<a href="https://hl7.org/fhir/us/core/STU3.1.1/StructureDefinition-us-core-medicationrequest.html" target="_blank">despite it being a required property</a>). 

<br/>

#### Conclusion

AthenaHealth is, in our opinion, the gold standard for Certified API implementations. Backing up from the details, the spirit of the ONC regulation is to enhance interoperability. The ability for developers to register an app, for a practice to enable that app and for data to flow is the essence of interoperability. Where many EHR developers put a support email in the critical path, AthenaHealth gets out of the way and puts its customers in control. By contrast, every other EHR we’ve worked with has had a kink somewhere in this process:
- Poor documentation
- No self registration (often requiring an email to an address not listed anywhere)
- No self-serve productionalization and installation
- Undocumented fees
- Broken exports that fail or get stuck
- Missing data or broken endpoints
- Requiring an undocumented “data backfill”

At a minimum, we hope that EHRs will, over time, imitate AthenaHealth’s self service process. It serves neither the practices, the app developers, nor the EHRs to require a back and forth over email to get setup. Additionally, the reliability of AthenaHealth’s Bulk Export server is something that we’d love to see more of across other EHRs. 


### Recommendations for other Bulk FHIR users

After 9 live EHR integrations and even more that were started which didn’t make it to production, we have learned a lot about this process. Although AthenaHealth is a great case study of what works well, as I outlined above, it seldom is this easy. Here are a few tips for other would-be Bulk FHIR users:

1. Use the <a href="https://chpl.healthit.gov/#/search" target="_blank">CHPL</a> to find out if Bulk FHIR is even allegedly supported. If you don’t see a g(10) certification criteria, you likely have to find another approach.
2. Don’t get confused by proprietary API programs. Many EHRs hide their Certified API docs and try to lead you to their proprietary APIs, which are often much more expensive and require you to build one-off implementations. If they’re ONC certified, they’ll have FHIR API docs. The links from the CHPL page should take you there.
3. If you can’t find a place to register a FHIR client, try to find an email. Oftentimes the email listed on the CHPL listing will yield results. You can try asking your practice to email the EHR support on your behalf, but these support staff seldom know what FHIR is, so this can be a bit of a rabbit hole.
4. Build with workflow resiliency in mind from the start. While AthenaHealth works great, many EHRs have intermittent failures. Make sure whatever Bulk FHIR client you use can handle retries.
5. Understand the <a href="https://www.healthit.gov/topic/certification-ehrs/conditions-maintenance-certification" target="_blank">Conditions & Maintenance of Certification</a>. These are requirements all ONC Certified EHR developers must abide by to keep their certification. We’ve found many EHRs are responsive to fixing bugs or registering your app when they are reminded of their obligations. If a g(10) certified developer tells you they don’t have Bulk FHIR, remind them that they have to have it!
6. Be persistent. Whether you’re getting slow responses about app registration or trying to get the EHR developer to fix a bug, we’ve found that with enough persistence you can get your implementation live. Just keep sending those follow up emails!
7. Check out the many tools for the SMART team. We’ve found the <a href="https://bulk-data.smarthealthit.org/" target="_blank">Bulk FHIR reference implementation</a> to be quite representative of real workflows, so it’s a great tool to build against.

<br />
