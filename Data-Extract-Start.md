# Overview

The `data_extract_start` event is published from Reaper to the EventStream when a scheduled job begins to execute. The scheduled job cadence is based on the ingestion configuration. Currently, Reaper is the only core service that listens to this message. Upon receiving this message, Reaper may send a `data_ingest_start` for other core services to prepare for incoming data. Reaper always begins a data extraction upon receiving this message, which uses the configured ingestion's Extract Steps to pull data from an external source. This data is parsed, chunked, and placed on the raw topic in the data pipeline for subsequent core services to read from and continue processing. Finally, once reaper has completed pulling from the external data source, reaper sends a `data_extract_end` event with the total message count so that other core services can determine when they have received and processed the full amount of messages.

# Diagram


![DataExtractStart](https://github.com/UrbanOS-Public/smartcitiesdata/assets/79863335/ef323fa7-8f09-4ce7-b5ac-d2cb11c5c6f5)



# Schema

| Field Name | Type | Description | Enum Values |
| - | - | - | - |
| type | string | Identifier of the event type. Used to filter and receive events | ingestion:update |
| data | [Ingestion](https://github.com/UrbanOS-Public/smartcitiesdata/wiki/Ingestion) | The Ingestion entity that determines the cadence and extract steps for this event. | - |
| author | string | The identifier of who sent the event. | - |

Performance: Negligible