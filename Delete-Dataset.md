# Overview

The `dataset_delete` event is published from Andi to the EventStream when a Curator deletes a Dataset configuration within the Andi interface. This message is how all core services know to stop processing data related to a dataset and remove the dataset from state. Additionally, Andi finds any ingestion that are configured to feed into the dataset and republishes the ingestion without the dataset, forklift creates a backup of the current data and removes the primary table, and discovery API clears state in Elasticsearch.

# Diagram

![DatasetDeleteEvent](https://github.com/UrbanOS-Public/smartcitiesdata/assets/79863335/a6aa23f1-bf7f-4aff-a663-0e6a4b7e077d)


# Schema

| Field Name | Type | Description | Enum Values |
| - | - | - | - |
| type | string | Identifier of the event type. Used to filter and receive events | dataset:delete |
| data | Dataset(TODO) | The Dataset entity that has been deleted in Andi. | - |
| author | string | The identifier of who sent the event. | - |

Performance: Negligible