

# What is GARDE?

GARDE is an open-source software platform, funded by several grants from
the National Cancer Institute, that enables population-level risk
assessment and genetic testing for conditions (e.g., cancer, familial
hypercholesterolemia) for which there are guidelines that support
genetic testing.
GARDE has two main components:

1.   **population-level risk assessment algorithms** that analyze patient data in the electronic
health record (EHR) to identify patients who meet criteria for genetic
testing (Figure 1); and

<figure>
<img alt="GARDE architecture" style="border-width:0" src="resources/images/GARDE_workflow.png" />
<figcaption><p>Figure 1. GARDE workflow.</p></figcaption>
</figure>

2.   an automated **chatbot** for patient outreach, pretest
education, and access to genetic testing (Figure 2).

<figure>
<img alt="GARDE architecture" style="border-width:0" src="resources/images/Chatbot_example.png" />
<figcaption><p>Figure 2. Chatbot example.</p></figcaption>
</figure>

GARDE uses an open architecture, external to the EHR, and is integrated with the EHR
through standards-based Web services.


# GARDE Implementation

## Governance Review and Approval

Most healthcare systems (HCS) have a governance committee that reviews
proposals for implementation of information technology (IT) tools.
Undergoing governance review and obtaining approval is a critical early
step to take once you have decided to implement GARDE. The governance
review and approval processes vary across institutions, but the
overarching goals of governance reviews are typically to:

1.  prioritize implementation/customization of EHR functionality

2.  assess the potential impact of new/customized functionality on
    existing clinical information systems, clinicians and patients

3.  assess the effort needed to implement/customize the EHR
    functionality

4.  ensure there are robust channels for user feedback and the
    dissemination of systems-related information to end users

### Additional information

1.  GARDE is highly customizable. Resources necessary for a successful
implementation will vary depending on implementing HCS factors and
decisions, such as:

    1)  Local IT architecture

    2)  How is Population Health Management function accomplished? (e.g.
Epic? REDCap?)

    3)  How are patient outreach and education accomplished? (e.g.
Chatbot? Clinical staff?)

    4)  How often is the target population evaluated? (e.g. One time?
Weekly?)

    5)  Number of individuals in the target population for GARDE
evaluation (e.g. does the target population include all patients
seen within HCS? All patients who saw a HCS primary care
provider for an outpatient visit within past 36 months? Only
patients who have electronic patient portal account? Patient age
range? Individuals with or without target condition?).

    6)  Condition(s) that GARDE evaluates (e.g. hereditary cancer
syndrome(s)? Familial hypercholesterolemia?)

    7) Local patient population characteristics (e.g. proportion with
commercial vs. government-supported health insurance vs.
uninsured? Proportion with Ashkenazi Jewish ancestry?)

    8)  Tasks necessary to implement GARDE (e.g. are all patients
considering genetic testing referred for genetic counseling?
will the implementation include both PHM algorithms and
chatbot?)

2.  In the BRIDGE trial, research eligibility screening and patient
outreach by a genetic counseling assistant required on average a
couple of minutes per patient.



## What can facilitate governance review?

To help you prepare for governance review within your HCS, following is information that may help you, including a description of [GARDE architecture](#garde-architecture), [GARDE deployment requirements](#deployment-requirements) and an estimate of the [IT](#it-human-resources) and [Clinical](#clinical-human-resources) Human Resources that may facilitate GARDE implementation.

### Identify key stakeholders and champions for your project

We recommend that you consider identifying representatives from the
  following stakeholder groups to support your review.

  - Relevant clinical stakeholders, such as:

    - Providers (e.g., primary care) whose patients will be evaluated by
      GARDE

    - Cancer genetics/genetic counseling specialists

    - If available, clinicians who care for patients with hereditary
      cancer predisposition syndromes

  - IT: EHR/Health IT, information security

  - Patient communication committee (this may be a part of EHR/IT)



### GARDE Architecture

Overall, the GARDE architecture contains four components: *OpenCDS*,
*Population Coordinator*, EHR *Patient Data Repository* (e.g., Epic’s
Clarity or other patient data repository), and Population Health
Management (PHM) tools (e.g., Epic’s Healthy Planet, REDCap) (see Figure
3). [OpenCDS](https://www.opencds.org/) is an open-source [CDS
Hooks](https://cds-hooks.org/)-compliant server that computes patient
eligibility for PHM cohorts. Population Coordinator is the application
endpoint and service choreographer that receives platform requests,
processes population data (transforms to/from Fast Healthcare
Interoperability Resources ([FHIR](https://www.hl7.org/fhir/))), and
sends evaluation conclusions to the PHM system. EHR Patient Data
Repository is the source for patient data used by the GARDE logic. EHR
PHM Tools include a registry where patients who met PHM criteria are
tracked and a dashboard that clinical staff (in Figure 1, an assistant)
use to navigate the registry, review individual patient data, and
perform patient outreach functions.

<figure>
<img alt="GARDE architecture" style="border-width:0" src="resources/images/GARDE-architecture.png" />
<figcaption><p>Figure 3. GARDE architecture. EHR or REDCap can perform
Population Health Management.</p></figcaption>
</figure>

### Deployment Requirements

The GARDE components that need to be deployed are the Population
Coordinator, OpenCDS (Figure 3), and FactDB (not shown). FactDB is a
central data store that serves multiple purposes: (1) provides a
persistent mechanism for GARDE tracking and managing patient cohorts,
patient facts, and data provenance; (2) supports interoperability by
using FHIR data elements and terminology; and (3) serves as a staging
area for intermediate data to improve performance.

Two deployment hosting strategies are supported:

1.  On premises — GARDE components are installed on the implementing
    site’s servers, typically Virtual Machines (VMs).

2.  Cloud — GARDE components are installed on an implementing site’s
    cloud-based solution (via [Docker](https://www.docker.com/) or
    [Kubernetes](https://kubernetes.io/)). Current cloud-based solutions
    include [AWS](https://aws.amazon.com/) and
    [Azure](https://azure.microsoft.com/en-us).



Detailed instructions, including the source code, for how to deploy
GARDE using Docker can be found
[here](https://bitbucket.org/RickSlc/garde-docker/src/main/README.md).

Once deployed, GARDE requires terminology mappings between the
implementing site’s family history codes and GARDE’s terminologies,
which use standards such as ICD 9, [ICD
10](https://www.cdc.gov/nchs/icd/icd-10/index.html),
[SNOMED](https://www.snomed.org/), [HL7](https://www.hl7.org/index.cfm),
and [SEER](https://seer.cancer.gov/). Mappings are created by data
analysts for each deployment site with help from tools provided by our
team. Once completed, mappings are then loaded into GARDE where they are
used to interpret family history data.

Relevant patient data, including patient family history data, are
extracted from the site’s EHR as input for GARDE evaluations. The
Population Coordinator executes an Extract, Transform, and Load (ETL)
pattern to identify and retrieve the screening population. GARDE
provides query specifications for these data, and, for Epic customers,
query templates. GARDE evaluations export results conducive for loading
into the PHM system. Two options are available, via secure structured
text file sharing, or via EHR web services APIs. Additional information
about GARDE’s architecture and deployment are available elsewhere.[^2]

## IT Human Resources

Based on GARDE implementation in BRIDGE, below is an estimate of the IT
human resources necessary for its successful implementation over the
initial 8 months. Some tasks may require \>1 individual. The exact
number of hours necessary will vary depending on how GARDE is
implemented.

### Planning

<u>Overall</u>: ~3 months of weekly or biweekly team
planning/coordination meetings (will likely include non-IT personnel,
only IT personnel tasks below)

| **Task** | **Hours** |
|----|---:|
| Requirements gathering about creating HCS compliant interface between GARDE and Epic, elicit additional code specific requirements identified while building/coding software (1 hour/week/person) | 54 |
| Manage project (1 hour/week/person) | 6 |
| **TOTAL** | **60** |


### Deployment

(only one deployment option (virtual machine OR cloud-based) is
necessary)

<table>
<colgroup>
<col style="width: 42%" />
<col style="width: 8%" />
<col style="width: 2%" />
<col style="width: 39%" />
<col style="width: 8%" />
</colgroup>
<thead>
<tr>
<th><u>On premise server</u></th>
<th></th>
<th></th>
<th><u>Institution-selected cloud service (e.g. AWS, Azure)</u></th>
<th></th>
</tr>
</thead>
<tbody>
<tr>
<td><strong>Task</strong></td>
<td><strong>Hours</strong></td>
<td></td>
<td><strong>Task</strong></td>
<td><strong>Hours</strong></td>
</tr>
<tr>
<td><p>Security review, design GARDE implementation plan, provide</p>
<p>approvals/oversight</p></td>
<td style="text-align: right;">10</td>
<td></td>
<td>Security review, design GARDE implementation plan, provide
approvals/oversight</td>
<td style="text-align: right;">10</td>
</tr>
<tr>
<td>Request virtual machine, install GARDE, configure, test</td>
<td style="text-align: right;">16</td>
<td></td>
<td>Develop GARDE system architecture appropriate for HCS</td>
<td style="text-align: right;">24</td>
</tr>
<tr>
<td>Enterprise data warehouse (EDW) project approval/oversight</td>
<td style="text-align: right;">2</td>
<td></td>
<td>Create virtual private cloud</td>
<td style="text-align: right;">24</td>
</tr>
<tr>
<td>Build EDW schema, grant access, review design</td>
<td style="text-align: right;">8</td>
<td></td>
<td>Configure queries to extract data for GARDE input</td>
<td style="text-align: right;">16</td>
</tr>
<tr>
<td></td>
<td style="text-align: right;"></td>
<td></td>
<td>ETL Process - move GARDE input data to cloud</td>
<td style="text-align: right;">12</td>
</tr>
<tr>
<td></td>
<td style="text-align: right;"></td>
<td></td>
<td>Adapt GARDE to run and input/output on cloud</td>
<td style="text-align: right;">TBD*</td>
</tr>
<tr>
<td>Map EHR codes to GARDE codes</td>
<td style="text-align: right;">8</td>
<td></td>
<td>Map EHR codes to GARDE codes</td>
<td style="text-align: right;">8</td>
</tr>
<tr>
<td><strong>TOTAL</strong></td>
<td style="text-align: right;"><strong>44</strong></td>
<td></td>
<td><strong>TOTAL</strong></td>
<td style="text-align: right;"><strong>106</strong></td>
</tr>
</tbody>
</table>

\* Amount of time necessary will depend on HCS ability to support
GARDE-established cloud deployments (AWS or Azure) and staff experience.

### Epic Integration

<table>
<colgroup>
<col style="width: 87%" />
<col style="width: 12%" />
</colgroup>
<thead>
<tr>
<th><strong>Task</strong></th>
<th style="text-align: right;"><strong>Hours</strong></th>
</tr>
</thead>
<tbody>
<tr>
<td><blockquote>
<p>Health Planet Registry (HPR) build</p>
</blockquote></td>
<td style="text-align: right;"><blockquote>
<p>60</p>
</blockquote></td>
</tr>
<tr>
<td><blockquote>
<p>ETL to populate HPR</p>
</blockquote></td>
<td style="text-align: right;"><blockquote>
<p>32</p>
</blockquote></td>
</tr>
<tr>
<td><blockquote>
<p>Smart Data Element (SDE) read/write interface</p>
</blockquote></td>
<td style="text-align: right;"><blockquote>
<p>32</p>
</blockquote></td>
</tr>
<tr>
<td><blockquote>
<p>GARDE interface to Epic</p>
</blockquote></td>
<td style="text-align: right;"><blockquote>
<p>16</p>
</blockquote></td>
</tr>
<tr>
<td><strong>TOTAL</strong></td>
<td style="text-align: right;"><strong>140</strong></td>
</tr>
</tbody>
</table>

### Chatbot integration and deployment

| **Task** | **Hours** |
|----|---:|
| Install GARDE chatbot, ETL chatbot data/ states to SDE, ETL chatbot links to EHR | 40 |
| **TOTAL** | **40** |

### Operations & Maintenance

| <u>On premise server</u> |  |  | <u>Institution-selected cloud service (e.g. AWS, Azure)</u> |  |
|----|----|----|----|----|
| **Task** | **Hours** |  | **Task** | **Hours** |
| Oversee GARDE operations. Apply fixes/security patches | 12 |  | Oversee GARDE operations. Apply fixes/security patches | ??\*\* |
| **TOTAL** | **12** |  | **TOTAL** | **??\*\*** |

\*\*Amount of time necessary will depend on complexities of cloud
deployment and amount of trouble-shooting necessary. Cloud-based
solutions other than AWS or Azure will require more time.

## Clinical Human Resources

Based on GARDE implementation in BRIDGE, below is an estimate of the
clinical human resources necessary for its successful implementation
over the initial 3 months. The exact number of hours necessary will vary
depending on how GARDE is implemented.

### Planning

<u>Overall: ~3 months of weekly or biweekly team planning/coordination
meetings (will likely</u>

<u>include non-clinical personnel, only clinical personnel tasks
below)</u>

<table>
<colgroup>
<col style="width: 91%" />
<col style="width: 8%" />
</colgroup>
<thead>
<tr>
<th><strong>Task</strong></th>
<th style="text-align: right;"><strong>Hours</strong></th>
</tr>
</thead>
<tbody>
<tr>
<td>Clarify genetic counseling team needs / workflow and agreement with
GARDE implementation (0.75 hour/week/person)</td>
<td style="text-align: right;"><blockquote>
<p>4.5</p>
</blockquote></td>
</tr>
<tr>
<td>Provide insights into workflow for contacting patients / charting
needs and requirements, provide feedback on running trial. (1 hour /
week / person)</td>
<td style="text-align: right;"><blockquote>
<p>6</p>
</blockquote></td>
</tr>
<tr>
<td>Provide insights into primary care teams’ and patients' needs,
current staff workflow, provide guidance for how to notify primary care
team. (0.5 hour/week/person)</td>
<td style="text-align: right;"><blockquote>
<p>6</p>
</blockquote></td>
</tr>
<tr>
<td>Develop and revise chatbot messages for all GARDE sub-populations (1
hour/week/person)</td>
<td style="text-align: right;"><blockquote>
<p>6</p>
</blockquote></td>
</tr>
<tr>
<td><strong>TOTAL</strong></td>
<td style="text-align: right;"><strong>22.5</strong></td>
</tr>
</tbody>
</table>



[^1]: Kaphingst KA, Kohlmann WK, Lorenz Chambers R, et al. Uptake of
    Cancer Genetic Services for Chatbot vs Standard-of-Care Delivery
    Models: The BRIDGE Randomized Clinical Trial. *JAMA Netw Open*. 2024
    Sep 3;7(9):e2432143. doi: 10.1001/jamanetworkopen.2024.32143. PMID:
    [39250153](https://pubmed.ncbi.nlm.nih.gov/39250153/); PMCID:
    [PMC11385050](https://pubmed.ncbi.nlm.nih.gov/39250153/).

[^2]: Bradshaw RL, Kawamoto K, Kaphingst KA, et al. GARDE: a
    standards-based clinical decision support platform for identifying
    population health management cohorts. *J Am Med Inform Assoc*.
    2022;29(5):928-936. PMID:
    [35224632](https://pubmed.ncbi.nlm.nih.gov/35224632/); PMCID:
    [PMC9006693](https://pmc.ncbi.nlm.nih.gov/articles/PMC9006693/).

