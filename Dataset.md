# Overview

Datasets are a top-level entities that maintain configuration provided from a Curator of the system. Datasets can be thought of as an empty bucket that can hold data that fits into the shape of that bucket. Datasets define a data schema which is used to create the Trino (SQL-like) table. A Dataset configuration does not pull data by itself, but rather, is a bucket for ingestions to feed into. Once an ingestion is configured to feed data into a dataset, the data is stored in the Trino table the dataset configured. Deleting a dataset will delete the Trino table and all the ingested data with it.

# Schema

| Field Name | Type | Description | Enum Values |
| - | - | - | - |
| todo | | | |
