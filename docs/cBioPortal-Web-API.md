[under construction]

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

* `Cancer Types` returns meta information about cancer types
  * `GET /cancer-types` returns all cancer types in the database. In cBioPortal.org, [OncoTree](http://oncotree.mskcc.org) is adopted as the ontology for cancer types. Note: not all cancer types have associated cancer studies.
    * example: http://www.cbioportal.org/beta/api/cancer-types
  * `/cancer-types/{cancerTypeId}` returns meta information about one cancer type.
    * example: http://www.cbioportal.org/beta/api/cancer-types/brca


