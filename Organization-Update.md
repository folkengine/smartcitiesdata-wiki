# Overview

The `organization_update` event is published from Andi to the EventStream when a Curator creates or modifies the configuration for an Organization within the Andi interface. This message is how core services in the system stay in sync regarding Organizations. Each core service creates a topic and any other initialization needed to be ready to receive extracted data from that ingestion.

# Diagram

![OrganizationUpdateEvent](https://github.com/UrbanOS-Public/smartcitiesdata/assets/79863335/13f200aa-c6fb-410c-8f96-b3d459564c0b)



# Schema

| Field Name | Type | Description | Enum Values |
| - | - | - | - |
| type | string | Identifier of the event type. Used to filter and receive events | ingestion:update |
| data | Organization(TODO) | The Organization entity that has been published from Andi. | - |
| author | string | The identifier of who sent the event. | - |

Performance: Negligible