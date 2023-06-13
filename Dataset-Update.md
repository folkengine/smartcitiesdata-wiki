# Overview

The `dataset_update` event is published from Andi to the EventStream when a Curator creates or modifies the configuration for a Dataset within the Andi interface. This message is how all core services in the system stay in sync regarding Datasets. Additionally, Forklift creates a table based on the Dataset schema and Discovery_API adds the dataset information to the search index so that it's visible in DiscoveryUI.

# Diagram

![DatasetUpdateEvent](https://github.com/UrbanOS-Public/smartcitiesdata/assets/79863335/2b911a35-6c95-47cb-ad0d-469bfc1800ce)


# Schema

| Field Name | Type | Description | Enum Values |
| - | - | - | - |
| type | string | Identifier of the event type. Used to filter and receive events | dataset:update |
| data | Dataset(TODO) | The Dataset entity that has been published from Andi. | - |
| author | string | The identifier of who sent the event. | - |

Performance: Negligible