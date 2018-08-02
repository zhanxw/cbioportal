[UNDER CONSTRUCTION]

## Introduction

The cBioPortal web application programming interface (API) provides direct programmatic access to all genomic data stored 
within the server. Following [the OpenAPI Specification](https://www.openapis.org/), the cBioPortal web API is RESTful and JSON-based. 
This enables you to easily access data from your favorite programming language, such as R, Python, Java, and JavaScript. 
Clients of this API can be generated with [Swagger Codegen tool](https://swagger.io/tools/swagger-codegen/).

Clients of the web API can issue the following types of queries:

* What cancer studies are stored on the server?
* What genetic profile types are available for cancer study X?  For example, does 
the server store mutation and copy number data for the TCGA Glioblastoma data?
* What case sets are available for cancer study X?  For example, what case sets are available for TCGA Glioblastoma?

Additionally, clients can easily retrieve "slices" of genomic data. For example, a client can retrieve all mutations
from PTEN and EGFR in the TCGA Glioblastoma data.

Please note that the example queries below are accurate, but they are not guaranteed to return data, 
as our database is constantly being updated. Please report issues to cbioportal@googlegroups.com.

## Previous Web API

The [previous Web API](http://www.cbioportal.org/web_api.jsp) offers services through the servlet `webservice.do` is no longer under active development and will be phasing out once this new API is released publicly.

## API content and structure

The API endpoints are documented at http://www.cbioportal.org/api/. Here, we provide some additional information about the content and structure of the API with examples.

### `Cancer Types` endpoint returns meta information about cancer types.
  * `GET /cancer-types` returns all cancer types in the database. In cBioPortal.org, [OncoTree](http://oncotree.mskcc.org) is being used as the ontology for cancer types. Note: not all cancer types have associated cancer studies.
    * example: http://www.cbioportal.org/beta/api/cancer-types
  * `GET /cancer-types/{cancerTypeId}` returns meta information about one cancer type.
    * example: http://www.cbioportal.org/beta/api/cancer-types/brca

### `Studies` endpoint returns meta information about cancer studies
  * `GET /studies` returns all studies.
    * example: http://www.cbioportal.org/api/studies
  * `GET /studies/{studyId}` returns one study by `studyId`.
    * example: http://www.cbioportal.org/api/studies/msk_impact_2017
  * `POST /studies/fetch` returns studies by `studyIds`.
    * example: `curl -X POST "http://www.cbioportal.org/api/studies/fetch?projection=SUMMARY" -H "accept: application/json" -H "Content-Type: application/json" -d "[\"brca_tcga\",\"ucec_tcga\"]"`
    
### `Patient` endpoint returns patient IDs and the study IDs that the patients belong to.
  * `GET /studies/{studyId}/patients/{patientId}` returns a patient by `patientId` and `studyId`. Note: a patient is uniquely identified by both a `patientId` and a `studyId`.
    * example: http://www.cbioportal.org/api/studies/msk_impact_2017/patients/P-0000004
  * `GET /studies/{studyId}/patients` returns all patients in a study.
    * example: http://www.cbioportal.org/api/studies/msk_impact_2017/patients
  * `POST /patients/fetch` returns patients by `patientId`s and `studyId`s.
  
### `Sample` endpoint returns sample IDs and patient IDs that the samples belong to (and study IDs).
  * `GET /studies/{studyId}/patients/{patientId}/samples` return all samples of a patient
    * example: http://www.cbioportal.org/api/studies/msk_impact_2017/patients/P-0001104/samples
  * `GET /studies/{studyId}/samples` returns all samples in a study
    * example: http://www.cbioportal.org/api/studies/msk_impact_2017/samples
  * `GET /studies/{studyId}/samples/{sampleId}` returns a sample by `sampleId` and `studyId`. Note: a sample is uniquely identified by both a `sampleId` and a `studyId`.
    * example: http://www.cbioportal.org/api/studies/msk_impact_2017/samples/P-0001104-T01-IM3
  * `POST /samples/fetch` returns samples by `patientId`s and `studyId`s, or `sampleListIds`.
  
### `Clinical Attributes` returns meta data of clinical attributes

### `Clinical Data` returns clinical data of patients and samples

### `Molecular Profiles` returns meta data of molecular profiles (e.g. mutations, copy number, and mRNA profiles) in studies

### `Molecular Data` returns molecular data (e.g. copy number, and mRNA data) of samples

### `Mutations` returns mutational data of samples

### `Gene Panels` return gene panel information (e.g. MSK-IMPACT gene panels) to determine whether a sample is sequenced/profiled in a gene
  
  
