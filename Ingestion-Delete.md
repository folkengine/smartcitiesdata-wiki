# Overview

The `ingestion_delete` event is published from Andi to the EventStream when a Curator deletes the configuration for an Ingestion within the Andi interface. This message instructs some core services in the system to cancel processes related to the ingestion and delete associated topics.

# Diagram

![IngestionDeleteEvent](https://github.com/UrbanOS-Public/smartcitiesdata/assets/79863335/986bb803-6714-4590-89e4-91b3b3943b54)


# Schema

| Field Name | Type | Description | Enum Values |
| - | - | - | - |
| type | string | Identifier of the event type. Used to filter and receive events | ingestion:delete |
| data | [Ingestion](https://github.com/UrbanOS-Public/smartcitiesdata/wiki/Ingestion) | The Ingestion entity that has been deleted from Andi. | - |
| author | string | The identifier of who sent the event. | - |

Performance: Negligible