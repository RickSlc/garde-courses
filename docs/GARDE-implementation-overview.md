---
date: "2026-06-09"

---



# GARDE Implementation Overview {-}

## Previous GARDE implementation{-}

GARDE supported the Broadening the Reach, Impact, and Delivery of
Genetic Services [(BRIDGE)](https://pmc.ncbi.nlm.nih.gov/articles/PMC11385050/) randomized controlled trial, which compared
the effectiveness of chatbot-based delivery of cancer genetic services
compared to standard care (SOC) among 3073 unaffected primary care
patients in two large healthcare systems. The study found that
chatbots are equivalent to SOC in terms of uptake of pretest cancer
genetic services and genetic testing. BRIDGE results suggest that
chatbots offer a scalable and secure way to deliver genetic services,
allowing genetic counselors to focus on more specialized care.

## Initial Integration Requirements{-}

The first level of integration required is between GARDE and data from the EHR, typically through the EHR's data warehouse (e.g., Epic's Clarity). When considering eligibility for hereditary cancer genetic testing, GARDE requires demographic (age, sex) and structured family history (relative, diagnosis, age of onset) data to determine who meets criteria. For healthcare systems that use Epic, GARDE provides Clarity queries to extract these data sets in the format GARDE requires (CSV files). For systems that rely on a different EHR, GARDE’s data input format is generic and can be generated using a similar method.

To analyze the population, GARDE’s web-based application is used to select, digest, and analyze the CSV files. Results are then written to another CSV file that includes detailed information about who met criteria for genetic testing and why. GARDE’s results are then available for use. GARDE runs within the institution's firewall, and EHR data used as GARDE inputs and outputs never leave the institution's IT premises.

## IT Requirements{-}

GARDE can be installed locally on premises, or its installation can be virtual/cloud-based (e.g., AWS, Microsoft Azure). GARDE is based on Java and SpringBot and can be deployed as executable jar files or via Docker on most operating systems (Linux, MacOS, Windows). The GARDE platform consists of three servers as follows:

- GARDE Population Coordinator – consists of a web application, REST services, and front-end dashboard/configuration app 
- OpenCDS Hooks – an open-source clinical decision support server accessed via CDS Hooks standards.
- Database (Postgres [default], MySQL, Oracle, MS SQL Server)

While the three servers can run on a single VM (or lightweight laptop), the most secure and performant implementation is to install each on different hardware or virtualized server with adequate memory and CPU, especially when analyzing large datasets with millions of patient records.  

## Steps to deploy{-}


<table style="width:100%; border-collapse:collapse; table-layout:fixed; font-size:18px;">
  <colgroup>
    <col style="width:33%;">
    <col style="width:33%;">
    <col style="width:34%;">
  </colgroup>
  <tr>
    <th style="border:1px solid #000; padding:10px; text-align:center; vertical-align:middle;">Task</th>
    <th style="border:1px solid #000; padding:10px; text-align:center; vertical-align:middle;">Person responsible</th>
    <th style="border:1px solid #000; padding:10px; text-align:center; vertical-align:middle;">Steps necessary to<br>accomplish task</th>
  </tr>
  <tr>
    <td style="border:1px solid #000; padding:10px; vertical-align:middle;">Establish IT System<br>Architecture</td>
    <td style="border:1px solid #000; padding:10px; vertical-align:middle;">Enterprise Architect</td>
    <td style="border:1px solid #000; padding:10px; vertical-align:middle;">Determines where/how to<br>deploy GARDE</td>
  </tr>
  <tr>
    <td style="border:1px solid #000; padding:10px; vertical-align:middle;">Allocate and configure<br>hardware/virtual machine<br>(VM)</td>
    <td style="border:1px solid #000; padding:10px; vertical-align:middle;">System Administrator<br>/Cloud Engineer</td>
    <td style="border:1px solid #000; padding:10px; vertical-align:middle;">Prepare servers for GARDE</td>
  </tr>
  <tr>
    <td style="border:1px solid #000; padding:10px; vertical-align:middle;">Install GARDE</td>
    <td style="border:1px solid #000; padding:10px; vertical-align:middle;">System Administrator<br>/Cloud Engineer</td>
    <td style="border:1px solid #000; padding:10px; vertical-align:middle;">Follow step-by-step GARDE<br>install instructions.</td>
  </tr>
  <tr>
    <td style="border:1px solid #000; padding:10px; vertical-align:middle;">Extract Patient Data</td>
    <td style="border:1px solid #000; padding:10px; vertical-align:middle;">Data Architect with Epic<br>Clarity and/or other EHR<br>data.</td>
    <td style="border:1px solid #000; padding:10px; vertical-align:middle;">Identify the population and<br>extract patient<br>demographics and family<br>history.</td>
  </tr>
  <tr>
    <td style="border:1px solid #000; padding:10px; vertical-align:middle;">Map local EHR Data to<br>GARDE</td>
    <td style="border:1px solid #000; padding:10px; vertical-align:middle;">Informaticist / analyst /<br>research coordinator</td>
    <td style="border:1px solid #000; padding:10px; vertical-align:middle;">Map EHR concept to ~90<br>GARDE concepts.</td>
  </tr>
  <tr>
    <td style="border:1px solid #000; padding:10px; vertical-align:middle;">Test/Refine concept<br>mappings, settings</td>
    <td style="border:1px solid #000; padding:10px; vertical-align:middle;">Informaticist / analyst /<br>research coordinator</td>
    <td style="border:1px solid #000; padding:10px; vertical-align:middle;">Validate GARDE rules are<br>finding the expected<br>patients; adjust the<br>memory and data paging<br>based on performance.</td>
  </tr>
  <tr>
    <td style="border:1px solid #000; padding:10px; vertical-align:middle;">Run GARDE</td>
    <td style="border:1px solid #000; padding:10px; vertical-align:middle;">Informaticist / analyst /<br>research coordinator</td>
    <td style="border:1px solid #000; padding:10px; vertical-align:middle;">Identify patients who are<br>eligible for genetic testing.</td>
  </tr>
</table>

The next steps are based on how you will use GARDE’s outputs. If GARDE is used only to identify a population for additional analysis, then nothing else is required. If GARDE will be used to support population-based outreach, pre-test education, and access to healthcare services, additional GARDE tools and interfaces are available including
1) a chatbot for automated outreach and pre-test education
2) Epic registry interface artifacts and instructions for managing and keeping track of outreach activities with GARDE cohorts
3) REDCap dashboard (coming soon!) as an alternative to Epic for patient outreach within research contexts.

## Contact information {-}

For additional information about any of these options or collaboration opportunities, [email the GARDE team](mailto:GARDE@hci.utah.edu).

