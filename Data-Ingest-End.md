# Overview

The `data_ingest_end` event is published from Forklift to the EventStream after the final data message has been persisted to the database. This message is likely unnecessary and is recommended to be removed in future modifications. Currently, this event causes Andi to record the ingested_time, which is not used. It also causes Forklift to end it's data processing subprocesses and remove the dataset state in Redis. The subprocesses need to be terminated, but the remaining behavior is unused.

# Diagram

![DataIngestEnd](https://github.com/UrbanOS-Public/smartcitiesdata/assets/79863335/b60735d9-732d-4975-b6b3-8ec80dd6ef18)



# Schema

| Field Name | Type | Description | Enum Values |
| - | - | - | - |
| type | string | Identifier of the event type. Used to filter and receive events | ingestion:update |
| data | Dataset(TODO) | The Dataset entity that forklift wrote data to. | - |
| author | string | The identifier of who sent the event. | - |

Performance: Negligible