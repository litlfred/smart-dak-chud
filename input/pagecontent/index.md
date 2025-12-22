
<!---Note: Please keep the div below in the main branch and when publishing the pre-release version of the DAK (v0.9.9). For official published versions, please delete the div from the file.-->
<div>
<p> This SMART Guidelines L2 specifications and set of implementation tools are still undergoing development. </p>
<p> Content is for demonstration purposes only. </p>
</div>{:.stu-note}


### Introduction 
Vector-borne diseases like dengue fever represent a significant and growing public health threat, heavily influenced by climate variability such as changes in temperature and rainfall. While public health officials recognize that outbreaks are intrinsically linked to these climate factors, they face a foundational challenge: epidemiological health data and meteorological climate data are locked in separate, fragmented systems that do not "speak the same language".


Prevention and preparedness require a portfolio of actions that bridge this divide. To transition from reactive response to proactive prevention, health systems must be able to anticipate outbreaks before they peak. This requires the integration of national surveillance data with reliable climate data to feed predictive models. However, this is not merely a technical challenge of moving data; it is a normative challenge of standardizing how disparate sectors collaborate.

#### Purpose of this IG
This Implementation Guide (IG) lays out the business specifications for the Climate and Health Data Unification for Dengue Surveillance (CHUD) system. Its primary purpose is to define a clear, standards-based approach for integrating epidemiological and climate data to enable national and sub-national early warning systems. Rather than building new proprietary tools, this IG focuses on the "rules of the road" that allow existing investments, such as national surveillance systems and WMO climate services to interoperate effectively This document is software-agnostic and provides a starting point for Member States to design, develop, and deploy the data unification layer necessary for climate-informed health surveillance, leveraging Digital Public Infrastructure (DPI) where available.

#### Target audience

The primary target audience of this document comprises national authorities responsible for overseeing national disease surveillance and vector control programs, particularly those establishing data integration pipelines to build and calibrate dengue predictive models. It is also designed for National Meteorological Agencies and climate data publishers tasked with producing and publishing standardized climate data. Additionally, this document serves government partners- including academic institutions, international organizations, NGOs, and technical partners - supporting Member States in the development and deployment of CHUD solutions.

#### Scope
##### In Scope
This IG specifically focuses on the data flow, standards, and harmonization processes that feed a district-level dengue prediction model. This document describes a specification for:
- Data Journey & Unification: Documenting the automated process of ingesting data from health surveillance systems and climate data sources (WIS 2.0) to a central point of unification.
- Core Data Elements: Defining the minimum essential data required, specifically epidemiological counts and environmental factors such as total rainfall and mean air temperature in Celsius.
- Harmonization Standards: Detailing the transformation of data to match temporally (converting daily climate data to epidemiological weeks) and spatially (aggregating climate data to district boundaries).
- Interoperability Standards: Recommending open standards, specifically HL7 FHIR for health data exchange and WMO’s WIS 2.0 for climate data access.


##### Out of Scope
Aspects that are considered out of the scope of this initial documentation are:
-	Predictive Model Development: The mathematical development, algorithm selection, or calibration of predictive models.
-	Visualization Tools: Documenting specific software for reporting, dashboards, or decision support interfaces.
- Entomological and other sources of data: While several sources of data are recognized as being also important for district-level modeling, such as mosquito indices and vector data, these have been excluded from this initial phase. 
- Clinical Protocols: The specific clinical care protocols for treating dengue patients once an outbreak is detected.
- Considerations for monitoring and evaluation of CHUD roll-out and use.


#### Assumptions

##### Meteorological data assumptions:

###### General Notes:

* The World Meteorological Organization (WMO) Information System (WIS) is the global system through which WMO Members exchange meteorological, hydrological, and climate data in real-time or near-real-time. It ensures interoperability through standardized formats and a unified data catalogue. WIS (including WIS 2.0) enables observations, forecasts, and warnings to circulate seamlessly and consistently worldwide.

* Real-time or near-real-time data is the range of data mandated for use with WIS2.0. Depending on the needs of the prediction model being used or developed and the data available through WIS2.0 in the specific context, this may require data storage by the model to achieve the desired data elements. Longer time ranges, such as historic data, may be accessible via WIS2.0; however, it is on a case-by-case basis. If historical data is required and not available via WIS2.0, this data should be sought from the nationally designated institutions or other authoritative data providers.

* Beyond the WMO climate and weather data exchange, international data centers such as NCEI (Data Access; National Centers for Environmental Information (NCEI)) and C3S (Climate Data Store), provide access to internationally exchanged data. If the required data is not accessible via WIS2 or a denser spatial scale of the model or product is required – working directly with nationally designated institutions or other data providers for data access should be considered.

* For this use-case, it is assumed that the required data for dengue prediction is accessible via WIS2.0 (or WIS2.0 Node).

##### Epidemiological data assumptions:

* The definition of a week should include 7 days and should align with the same standards as used by the epidemiological indicators, known as the epidemiological week.

* It is assumed that upstream data entering from health facilities contains manual entry errors (e.g., incorrect province names, misspellings). While basic validation is assumed at the source, the unification system acts on the assumption that a secondary automated cleaning phase is required to flag gross inconsistencies before harmonization.

### L1 Narrative guidelines
This DAK focuses on the following health interventions and recommendations related to a CHUD.

#### WHO universal health coverage interventions referenced in this DAK

The key interventions for a Climate and Health data Unification for Dengue Surveillance (CHUD) system referenced in this DAK, as defined in the WHO universal health coverage list of essential interventions and WHO guidelines and guidance documents, are the following:

TBC

#### Digital health interventions covered in this DAK

Table 2 lists the digital health interventions incorporated into this DAK, extracted from the WHO Classification of Digital Interventions, Services and Applications in Health (3):

#### Table 2. Digital health interventions incorporated into the DAK for a heat-health alert system

| DHI Category | DHIs |
|--------------|------|
| 2.5 Healthcare provider communication | 2.5.3 Transmit routine news and workflow notifications to healthcare provider(s)<br>2.5.4 Transmit non-routine health event alerts to healthcare provider(s) |
| 2.7 Scheduling and activity planning for healthcare providers | 2.7.1 Identify persons in need of services |
| 3.7 Facility management | 3.7.1 List health facilities and related information |
| 4.1 Data Management | 4.1.2 Data storage and aggregation |
| 4.3 Geo spatial information management | 4.3.2 Map location of health event<br>4.3.5 Map health and health indicator data to geographic data |
| 4.4 Data exchange and interoperability | 4.4.2 Standards-compliant interoperability |

#### WHO guidelines, recommendations and guidance

The DAKs are intended to reflect health recommendations and content that has already been published in WHO guidelines and guidance documents. The health content and interventions for this DAK are based on the following WHO documents:

| Document | Document |
|----------|----------|
| [Early Warning and Response System (EWARS) for Dengue Outbreaks](https://iris.who.int/server/api/core/bitstreams/4d13ec19-9669-4c3a-a2d0-9ac649316e8d/content) | [Dengue: Guidelines for Diagnosis, Treatment, Prevention and Control](https://iris.who.int/server/api/core/bitstreams/b1db05d8-0e97-469e-b529-464dca6de540/content) |
| [Early Warning and Response System (EWARS) - Dengue Edition](https://iris.who.int/server/api/core/bitstreams/714eb93b-a435-4ab1-a489-c403568cadda/content) | |

Other, non-WHO, references used in this implementation guide are listed below:
* [WMO-No. 1131](https://library.wmo.int/idurl/4/51447)
* [WMO-No. 1060](https://library.wmo.int/idurl/4/68731)
* Guide to the WMO Information System, Volume II. WMO Information System 2.0. [WMO-No. 1061](https://library.wmo.int/idurl/4/69130)
* Climate Data Management System Specifications. [WMO-No. 1131](https://library.wmo.int/idurl/4/51447)
* [WMO-No. 1281](https://library.wmo.int/records/item/57850-world-meteorological-congress)
* Guide to the WMO Information System, Volume II. WMO Information System 2.0. [WMO-No. 1061](https://library.wmo.int/idurl/4/69130)

### L2 Operational guidelines

- Implementation tools:

   - [Link to the editable files of business processes, in .bpmn format](https://worldhealthorganization.github.io/smart-dak-chud/business-processes.html)
   
   - [Link to core data dictionary](https://worldhealthorganization.github.io/smart-dak-chud/dictionary.html)
 
   - Decision-support logic is not available at the moment. 

   - Scheduling logic is not available at the moment.

   - Indicators are not available at the moment.
 
   - [Link to functional and non functional requirements](https://worldhealthorganization.github.io/smart-dak-chud/system-requirements.html).

### L3 Machine readable guidelines
The L3 FHIR Implementation Guide for the CHUD L2 SMART Guidelines is yet to be published. Links will be published here as soon as they're available.

### L4 Executable guidelines
Reference implementations representing the L4 layer for the the CHUD L2 Guidelines are not yet available. Links will be published here as soon as they're available.

### L5 Dynamic guidelines
Content representing the L5 layer for the CHUD L2 SMART Guidelines are not yet available. Links will be published here as soon as they're available.

### Contact Us
<p>Please let us know about your experience in using this L2 specifications and questions you may have by contacting us at <a href= "mailto:SMART@who.int?subject = HHAS L2 Feedback">SMART@who.int</a></p>

### License
This work is licensed under a
[Creative Commons Attribution-NonCommercial-ShareAlike 3.0 IGO License][cc-by].

[![CC BY 3.0][cc-by-image]][cc-by]

[cc-by]: http://creativecommons.org/licenses/by-nc-sa/3.0/igo/
[cc-by-image]: https://i.creativecommons.org/l/by-nc-sa/3.0/igo/88x31.png
[cc-by-shield]: https://img.shields.io/badge/License-CC%20BY%203.0-lightgrey.svg

For more license details please see the [license page](https://worldhealthorganization.github.io/smart-dak-chud/license.html).

### Providing Feedback
{% include feedback.md %}

<!---Note: Please keep the dsiclaimer note below in the main branch and when publishing the pre-release version of the DAK (v0.9.9). For official published versions, please delete the note from the file.-->
### Disclaimer
The specification herewith documented is a demo working specification and may not be used for any implementation purposes. This draft is provided without warranty of completeness or consistency and the official publication supersedes this draft. No liability can be inferred from the use or misuse of this specification or its consequences.