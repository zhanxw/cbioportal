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
  
### `Sample Lists` returns sample lists. A sample list is a list of samples by ID.
  * `GET /studies/{studyId}/sample-lists` returns meta information of all sample list in a study
    * example: http://www.cbioportal.org/api/studies/msk_impact_2017/sample-lists
  * `GET /sample-lists/{sampleListId}` returns meta information of a sample list, including `sampleIds`.
    * example: http://www.cbioportal.org/api/sample-lists/msk_impact_2017_sequenced
  * `GET /sample-lists/{sampleListId}/sample-ids` returns sample IDs in a sample list.
    * example: http://www.cbioportal.org/api/sample-lists/msk_impact_2017_sequenced/sample-ids
  * `GET /sample-lists` returns meta information about all sample lists
    * example: http://www.cbioportal.org/api/sample-lists
  * `POST /sample-lists/fetch` returns meta information about specific sample lists by IDs.
  
### `Clinical Attributes` returns meta data of clinical attributes.
  * `GET /clinical-attributes` returns all clinical attributes.
    * example:  http://www.cbioportal.org/api/clinical-attributes
  * `GET /studies/{studyId}/clinical-attributes` returns all clinical attributes in a study.
    * example: http://www.cbioportal.org/api/studies/msk_impact_2017/clinical-attributes
  * `GET /studies/{studyId}/clinical-attributes/{clinicalAttributeId}` returns a clinical attribute.
    * example: http://www.cbioportal.org/api/studies/msk_impact_2017/clinical-attributes/CANCER_TYPE
  * `POST /clinical-attributes/fetch` returns clinical attributes by `studyId`s.
  
### `Clinical Data` returns clinical data of patients and samples.  Note: clinical data may link to a patient or a sample.
  * `GET /studies/{studyId}/clinical-data` returns all clinical data in a study.
    * example: http://www.cbioportal.org/api/studies/msk_impact_2017/clinical-data
  * `GET /studies/{studyId}/patients/{patientId}/clinical-data` returns all clinical data of a patient.
    * example: http://www.cbioportal.org/api/studies/msk_impact_2017/patients/P-0001104/clinical-data
  * `GET /studies/{studyId}/samples/{sampleId}/clinical-data` returns all clinical data of a sample.
    * example: http://www.cbioportal.org/api/studies/msk_impact_2017/samples/P-0001104-T01-IM3/clinical-data
  * `POST /clinical-data/fetch` return clinical data by `patientId`s or `sampleId`s and `studyId`s
  * `/studies/{studyId}/clinical-data/fetch` return clinical data by `patientId`s or `sampleId`s in a study

### `Molecular Profiles` returns meta data of molecular profiles (e.g. mutations, copy number, and mRNA profiles) in studies
  * `GET /molecular-profiles` returns all molecular profiles.
    * example: http://www.cbioportal.org/api/molecular-profiles
  * `GET /studies/{studyId}/molecular-profiles` return all molecular profile in a study.
    * example: http://www.cbioportal.org/api/studies/brca_tcga/molecular-profiles
  * `GET /molecular-profiles/{molecularProfileId}` returns a molecular profile by ID.
    * example: example: http://www.cbioportal.org/api/molecular-profiles/brca_tcga_rna_seq_v2_mrna
  * `POST /molecular-profiles/fetch` returns molecular profiles by IDs.  

### `Molecular Data` returns molecular data (e.g. copy number, and mRNA data) of samples
  * `GET /molecular-profiles/{molecularProfileId}/molecular-data` returns molecular data of a gene in a profile for a set of samples in a sampleList
    * example: http://www.cbioportal.org/api/molecular-profiles/acc_tcga_rna_seq_v2_mrna/molecular-data?sampleListId=acc_tcga_all&entrezGeneId=7157
  * `POST /molecular-profiles/{molecularProfileId}/molecular-data/fetch` returns molecular data in a profile, filtered by genes and sample IDs.
  * `POST /molecular-data/fetch` returns molecular data in multiple profiles, filtered by genes and sample IDs.

### `Mutations` returns mutational data of samples
  * `GET /molecular-profiles/{molecularProfileId}/mutations` returns mutations in a profile by `sampleListId`.
    * example: http://www.cbioportal.org/api/molecular-profiles/acc_tcga_mutations/mutations?sampleListId=acc_tcga_sequenced
  * `POST /molecular-profiles/{molecularProfileId}/mutations/fetch` returns mutations in a profile, filtered by genes and sample IDs.
  * `POST /mutations/fetch` returns mutations in multiple profiles, filtered by genes and sample IDs.

### `Gene Panels` return gene panel information (e.g. MSK-IMPACT gene panels) to determine whether a sample is sequenced/profiled in a gene
  * `GET /gene-panels/{genePanelId}` returns meta information of a specific gene panel
    * example: http://www.cbioportal.org/api/gene-panels/IMPACT341
  * `GET /gene-panels` returns meta information of all gene panels
    * example: http://www.cbioportal.org/api/gene-panels
  * `POST /gene-panels/fetch` returns meta information of gene panels
  * `POST /molecular-profiles/{molecularProfileId}/gene-panel-data/fetch` returns gene panel data in one profile, i.e. whether a sample is sequenced/profiled in a gene
  * `POST /gene-panels-data/fetch` returns gene panel data in multiple profiles

  
