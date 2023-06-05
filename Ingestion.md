# Overview

Ingestions are a top-level entities that maintain configuration provided from a Curator of the system. Ingestions schedule extractions of data to be performed at the specified cadence. Ingestions maintain any information needed to pull data, such as Authentication headers through the Secret Extract Step or the connection URL through the HTTP Extract Step. Every ingestion feeds into one or more Datasets, which is how the data is stored and hosted.

# Schema

| Field Name | Type | Description | Enum Values |
| - | - | - | - |
| id | string | Internal system ID | - |
| name | string | Display name | - |
| allow_duplicates | boolean | Likely deprecated code in Reaper | - |
| cadence | string | String that indicates when the ingestion should start an extraction | once, never, {cron string} |
| extractSteps | list(ExtractStep) | Extract steps are processes that need to be performed when ingesting from an external data source. The most common type is an HTTP extract step. Configured by the Curator in Andi. | - |
| schema | list(DictionaryField) | An ingestion schema represents the data structure that will be ingested from the external data source. This schema needs to match the external data source structure, but can differ from the final Dataset schema that will hosted through the UrbanOS platform. Transformations can be used as a bridge between Ingestion Schema and Dataset Schema | - |
| sourceFormat | string | Indicates the format of the External Data Source. Used to know how to parse incoming data. | text/csv, text/plain, application/json, text/xml, application/geo+json, application/zip, application/gtfs+protobuf |
| targetDatasets | list(string) | A list of datasets that the ingestion will feed into. Can use transformations to conform to multiple target Dataset schemas if naming varies. | - |
| topLevelSelector | string | An xPath selector that indicates the desired top-level field from an external data source. Can be used to discard unwanted metadata or only select a specific part of an external data source. | - |
| transformations | list(Transformation) | Transformations are processes that need to be performed after extracting data from an external data source. Transformations include a wide variety of operations ranging from mathematical operations to regex parsing. Curators configure transformations in the Andi interface. | - |
| publishedStatus | string | Enum that indicates if the ingestion is in published state or draft. | published, draft |
