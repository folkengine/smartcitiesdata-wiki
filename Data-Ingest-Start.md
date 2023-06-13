# Overview

The `data_ingest_start` event is published from Reaper to the EventStream after it has started a data extraction. This event lets the remaining core services know to be ready for incoming data by ensuring data pipeline topics are created and sub-processes are started to listen and handle new messages on those topics. Each service handles the messages based on the purpose of that service. Once forklift determines it has fully received and persisted all data messages, it sends a data_ingest_end message to the EventStream so that core services can end their data processing subprocesses.

# Diagram

![DataIngestStart](https://github.com/UrbanOS-Public/smartcitiesdata/assets/79863335/3652da7b-017e-4b85-bd6a-4aa06de3d4ea)


# Schema

| Field Name | Type | Description | Enum Values |
| - | - | - | - |
| type | string | Identifier of the event type. Used to filter and receive events | data:ingest:start |
| data | [Ingestion](https://github.com/UrbanOS-Public/smartcitiesdata/wiki/Ingestion) | The Ingestion entity responsible for this data extraction. | - |
| author | string | The identifier of who sent the event. | - |

Performance: Negligible