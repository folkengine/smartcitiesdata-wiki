# Overview

The `user_login` event is published from either Andi or DiscoveryUI to the EventStream when a user successfully completes the login authentication flow. If a user completes the login flow, through Auth0 for example, but does not exist in the system, this event allows any core service related to user management to collect and persist information about the user to be used later.

# Diagram

![UserLoginEvent](https://github.com/UrbanOS-Public/smartcitiesdata/assets/79863335/e5daa9cb-20ad-485e-b5d2-8a526431417b)


# Schema

| Field Name | Type | Description | Enum Values |
| - | - | - | - |
| type | string | Identifier of the event type. Used to filter and receive events | user:login |
| data | UserLogin(TODO) | The UserLogin entity built from the successful authentication flow. | - |
| author | string | The identifier of who sent the event. | - |

Performance: Negligible