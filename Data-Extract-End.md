# Overview

The `data_extract_end` event is published from Reaper to the EventStream once Reaper has written the final data message to the raw topic. This event signals to forklift the total number of messages written to the data pipeline (raw topic) so that forklift can determine when it has handled all incoming messages and is safe to compact the data into the permanent table instead of the temporary JSON staging table.


# Diagram

![DataExtractEnd](https://github.com/UrbanOS-Public/smartcitiesdata/assets/79863335/d5a84c8e-befd-47f8-ae39-98e532da07ff)


# Schema

| Field Name | Type | Description | Enum Values |
| - | - | - | - |
| type | string | Identifier of the event type. Used to filter and receive events | data:extract:end |
| data | DataExtractEndMessage(TODO) | The data object containing information needed to determine when a core service has received all data messages. | - |
| author | string | The identifier of who sent the event. | - |

Performance: Negligible