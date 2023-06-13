# Overview

The `user_organization_associate` event is published from Andi to the EventStream when a Curator adds a user to an organization. Adding a user to an organization gives that user access to all datasets created from that organization. Therefore, this message informs all core services related to data access that a user has been associated to an organization.

# Diagram


![UserAssociateWithOrgEvent](https://github.com/UrbanOS-Public/smartcitiesdata/assets/79863335/53780bd3-e664-4103-b8e6-0586eeb98ee5)



# Schema

| Field Name | Type | Description | Enum Values |
| - | - | - | - |
| type | string | Identifier of the event type. Used to filter and receive events | user:organization:associate |
| data | UserOrganizationAssociate(TODO) | The UserOrganizationAssociate entity derived from the related org and user. | - |
| author | string | The identifier of who sent the event. | - |

Performance: Negligible