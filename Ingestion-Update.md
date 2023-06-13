# Overview

The `ingestion_update` event is published from Andi to the EventStream when a Curator creates or modifies the configuration for an Ingestion within the Andi interface. This message is how all core services in the system stay in sync regarding Ingestions. Each core service creates a topic and any other initialization needed to be ready to receive extracted data from that ingestion.

# Diagram

![IngestionUpdateEvent](https://github.com/UrbanOS-Public/smartcitiesdata/assets/79863335/777e668c-6f12-4663-b25d-618a5e1abaa6)


# Schema

| Field Name | Type | Description | Enum Values |
| - | - | - | - |
| type | string | Identifier of the event type. Used to filter and receive events | ingestion:update |
| data | [Ingestion](https://github.com/UrbanOS-Public/smartcitiesdata/wiki/Ingestion) | The Ingestion entity that has been published from Andi. | - |
| author | string | The identifier of who sent the event. | - |

Performance: Negligible