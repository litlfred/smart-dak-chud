This page lists and describes digital services relevant for a CHUD system, derived from the business requirements defined at the operational level (L2). For more details about end-users and related stakeholders, see the Generic Personas.


Digital Services for a CHUD
| Digital Service | Description |
|-----------------|-------------|
| Climate Data Service (WIS 2.0) | A standardized service (e.g., WMO Information System) that provides discoverable, accessible, and authoritative meteorological data (historical and forecast). Acts as the primary "Data Publisher" for environmental variables. |
| Surveillance Data Repository | The existing national health information system that functions as the source of truth for epidemiological data, receiving case reports from healthcare workers and verifying them via surveillance officers. |
| Data Integration Service | A middleware component or central hub responsible for the automated ingestion of raw data streams from both the health repository and the climate service. It handles initial validation and formatting. |
| Harmonization Engine | A specialized service that transforms disparate data to match temporally and spatially. It aggregates daily climate readings into Epidemiological Weeks and maps meteorological stations to health district boundaries or sentinel sites. |
| Unified Data Store | A database or repository that stores the final, cleaned, and merged "Unified Data Record". This data is "analysis-ready" for ingestion by predictive models. |