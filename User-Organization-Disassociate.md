# Overview

The `user_organization_disassociate` event is published from Andi to the EventStream when a Curator removes a user from an organization. Removing a user from an organization will deny access to private datasets belonging to that organization, unless an appropriate access group is available. This message informs the core services related to determining dataset access that the user is no longer associated to the organization.

# Diagram

![UserOrgDisassociateEvent](https://github.com/UrbanOS-Public/smartcitiesdata/assets/79863335/4678ca95-f331-402c-808a-7db64593235f)



# Schema

| Field Name | Type | Description | Enum Values |
| - | - | - | - |
| type | string | Identifier of the event type. Used to filter and receive events | user:organization:disassociate |
| data | UserOrganizationDisassociate(TODO) | The UserOrganizationDisassociate entity that is derived from the related user and org. | - |
| author | string | The identifier of who sent the event. | - |

Performance: Negligible