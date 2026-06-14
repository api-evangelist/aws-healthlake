# AWS HealthLake GraphQL Schema

## Overview

AWS HealthLake is a HIPAA-eligible, FHIR R4-compliant managed service for importing, transforming, storing, and querying health data from patients and clinical systems. This conceptual GraphQL schema covers both the HealthLake management plane (data stores, import/export jobs, tagging) and the full FHIR R4 resource model exposed through the HealthLake transactional FHIR server.

- Schema file: [aws-healthlake-schema.graphql](aws-healthlake-schema.graphql)
- Provider documentation: https://docs.aws.amazon.com/healthlake/latest/devguide/
- API reference: https://docs.aws.amazon.com/healthlake/latest/APIReference/Welcome.html
- FHIR R4 specification: https://hl7.org/fhir/R4/

## Schema Source

Derived from:

- AWS HealthLake API reference (data store management, import/export, tagging operations)
- HL7 FHIR R4 specification resource definitions
- AWS HealthLake FHIR R4 server capabilities documentation
- US Core Implementation Guide profiles

## Type Categories

### HealthLake Management Types

Types representing the AWS HealthLake control plane for managing FHIR data stores and bulk operations:

| Type | Description |
|------|-------------|
| `DataStore` | A HealthLake FHIR R4 data store with endpoint, encryption, and identity configuration |
| `DataStoreStatus` | Status response when creating or deleting a data store |
| `DataStoreProperties` | Detailed properties of a HealthLake data store |
| `DatastoreFilter` | Filter criteria for listing data stores |
| `ImportJobProperties` | Properties of a FHIR bulk import job from Amazon S3 |
| `ExportJobProperties` | Properties of a FHIR bulk export job to Amazon S3 |
| `FHIRVersion` | Enum for supported FHIR versions (R4) |
| `Tag` | Key-value tag applied to a HealthLake resource |
| `TagResource` | Tags associated with an ARN-identified HealthLake resource |
| `S3Configuration` | S3 URI and optional KMS key for import/export data locations |
| `KmsEncryptionConfig` | Customer-managed or AWS-owned KMS key encryption configuration |
| `SseConfiguration` | Server-side encryption configuration wrapping KMS config |
| `PreloadDataConfig` | Optional Synthea synthetic data preload configuration |
| `IdentityProviderConfiguration` | SMART on FHIR identity provider and authorization configuration |

### FHIR R4 Clinical Resource Types

Core clinical FHIR R4 resources supported in HealthLake data stores:

| Type | Description |
|------|-------------|
| `Patient` | Demographics and administrative information about a person receiving care |
| `Practitioner` | A healthcare professional involved in care provision |
| `PractitionerRole` | Roles, locations, and availability of a practitioner at an organization |
| `Organization` | A formally or informally recognized grouping of people providing healthcare |
| `RelatedPerson` | A person related to a patient with involvement in their care |
| `Encounter` | An interaction between a patient and healthcare provider |
| `Condition` | A clinical condition, problem, or diagnosis |
| `Observation` | Measurements and simple assertions about a patient or device |
| `MedicationRequest` | An order or request for medication supply and administration |
| `MedicationAdministration` | Describes the event of a patient consuming medication |
| `DiagnosticReport` | Findings and interpretation of diagnostic tests |
| `Procedure` | An action performed on or for a patient |
| `Immunization` | Administration of a vaccine |
| `AllergyIntolerance` | Risk of harmful or undesirable physiological response to a substance |
| `CarePlan` | Healthcare plan for a patient addressing conditions and goals |
| `CareTeam` | Planned participants in the coordination of patient care |

### FHIR R4 Financial Resource Types

Financial and claims-related FHIR R4 resources:

| Type | Description |
|------|-------------|
| `Coverage` | Insurance or medical plan and payment details |
| `Claim` | A provider request for reimbursement for products/services |
| `ExplanationOfBenefit` | Full adjudication response combining claim and coverage details |

### FHIR R4 Workflow and Infrastructure Types

| Type | Description |
|------|-------------|
| `DocumentReference` | A reference to a clinical document of any kind |
| `ServiceRequest` | A record of a request for a service such as a procedure or diagnostic |
| `Communication` | A record of an exchange of information |
| `Task` | A task to be performed in the context of a care workflow |
| `Consent` | A patient's agreement to or restrictions on healthcare activities |
| `NutritionOrder` | A request for dietary or nutritional intake |
| `Device` | A medical device used in care delivery |
| `Substance` | A homogeneous material with definite composition |

### FHIR R4 Quality and Reporting Types

| Type | Description |
|------|-------------|
| `Measure` | A computable clinical quality measure definition |
| `MeasureReport` | Results of the calculation of a clinical quality measure |
| `Questionnaire` | A structured set of questions |
| `QuestionnaireResponse` | Answers to a set of questionnaire questions |

### FHIR R4 Infrastructure and Provenance Types

| Type | Description |
|------|-------------|
| `Bundle` | A container for a collection of FHIR resources |
| `CapabilityStatement` | A statement of the FHIR server's capabilities |
| `OperationOutcome` | Information about the outcome of a FHIR operation |
| `Provenance` | Who, what, when, where, and why for a set of resources |
| `AuditEvent` | A record of an event made for security and audit purposes |

### Location and Healthcare Delivery Types

| Type | Description |
|------|-------------|
| `Location` | Details and position information for a physical place |
| `HealthcareService` | Details of a healthcare service available at a location |

### FHIR Common Data Types

Shared structures used across FHIR resources:

`Coding`, `CodeableConcept`, `Identifier`, `Reference`, `Period`, `Quantity`, `Range`, `Ratio`, `HumanName`, `Address`, `ContactPoint`, `Annotation`, `Attachment`, `Timing`, `Dosage`, `Narrative`, `Meta`, `Money`, `Expression`, `ContactDetail`, `UsageContext`, `RelatedArtifact`

## Operations

### Queries

- `getDataStore` / `listDataStores` / `describeDataStore` — data store lookup and listing
- `describeFHIRImportJob` / `listFHIRImportJobs` — import job status and history
- `describeFHIRExportJob` / `listFHIRExportJobs` — export job status and history
- `listTagsForResource` — retrieve tags on a HealthLake resource ARN
- Per-resource getters for all 30+ FHIR resource types
- `listPatients`, `listEncounters`, `listConditions`, `listObservations` with FHIR search parameters
- `getCapabilityStatement` — retrieve the FHIR server capability statement for a data store

### Mutations

- `createDataStore` / `deleteDataStore` — data store lifecycle management
- `startFHIRImportJob` / `startFHIRExportJob` / `cancelFHIRExportJob` — bulk data operations
- `tagResource` / `untagResource` — resource tagging
- FHIR create, update, and delete operations for Patient, Encounter, Condition, Observation, MedicationRequest, DiagnosticReport, Procedure, Immunization, Task, Consent, and Bundle

## Enums

| Enum | Values |
|------|--------|
| `FHIRVersion` | R4 |
| `DataStoreStatus` | CREATING, ACTIVE, DELETING, DELETED, CREATE_FAILED, DELETE_FAILED |
| `JobStatus` | SUBMITTED, IN_PROGRESS, COMPLETED_WITH_ERRORS, COMPLETED, FAILED, CANCEL_SUBMITTED, CANCEL_IN_PROGRESS, CANCEL_COMPLETED, CANCEL_FAILED |
| `BundleType` | DOCUMENT, MESSAGE, TRANSACTION, TRANSACTION_RESPONSE, BATCH, BATCH_RESPONSE, HISTORY, SEARCHSET, COLLECTION |
| `AdministrativeGender` | MALE, FEMALE, OTHER, UNKNOWN |
| `ObservationStatus` | REGISTERED, PRELIMINARY, FINAL, AMENDED, CORRECTED, CANCELLED, ENTERED_IN_ERROR, UNKNOWN |
| `EncounterStatus` | PLANNED, ARRIVED, TRIAGED, IN_PROGRESS, ON_LEAVE, FINISHED, CANCELLED, ENTERED_IN_ERROR, UNKNOWN |
| `TaskStatus` | DRAFT, REQUESTED, RECEIVED, ACCEPTED, REJECTED, READY, CANCELLED, IN_PROGRESS, ON_HOLD, FAILED, COMPLETED, ENTERED_IN_ERROR |
| `ConsentStatus` | DRAFT, PROPOSED, ACTIVE, REJECTED, INACTIVE, ENTERED_IN_ERROR |

## Notes

This schema is conceptual. AWS HealthLake does not natively expose a GraphQL API; it exposes a REST-based FHIR R4 server and a management REST API via AWS SDK. This schema represents the domain model as it would appear in a GraphQL layer built on top of HealthLake, useful for API design, federation planning, or tooling integration.

The FHIR resource fields follow HL7 FHIR R4 (4.0.1) definitions. Search and filter parameters align with the FHIR search specification and HealthLake's supported search parameters as documented in the AWS HealthLake Developer Guide.
