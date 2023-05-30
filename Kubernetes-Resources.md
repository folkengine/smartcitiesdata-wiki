# Pods

Pods are containers that host (docker) images, which isolate an environment to contain dependencies needed for an application. Generally, they are managed by a deployment, operator, or stateful set. External information can be connected to pods via environment variables and hard drives (Persistent Volume Claims).

| Name | Type | Release | Description | Safe to delete? | Secrets | Kustomized? | Managed by Operator? | Chart repository | Triage Flow/Troubleshooting Docs |
| - | - | - | - | - | - | - | - | - | - |
| andi-* | Pod | UrbanOS | This pod hosts the ANDI front end  | Yes, it will restart | andi.postgres.password, andi.auth.auth0_client_secret, global.objectStore.accessKey, global.objectStore.accessSecret, global.redis.passwordSecret | No | No | https://github.com/UrbanOS-Public/charts/tree/master/charts/andi | NA |
| discovery-api-* | Pod | UrbanOS | This pod hosts the discovery API microservice. It can be directly queried via API externally or used by DiscoveryUI as a backend  | Yes, it will restart. | discovery-api.secrets.discoveryApiPresignKey, discovery-api.secrets.guardianSecretKey, discovery-api.postgres.password, global.objectStore.accessKey, global.objectStore.accessSecret, global.redis.passwordSecret | No | No | https://github.com/UrbanOS-Public/charts/tree/master/charts/discovery-api | NA |
| discovery-streams-* | Pod | UrbanOS | This pod hosts a websocket service that can retrieve data. It can be directly queried via WS connection externally. | Yes, it will restart. | global.redis.passwordSecret | No | No | https://github.com/UrbanOS-Public/charts/tree/master/charts/discovery-streams | NA |
| discovery-ui-* | Pod | UrbanOS | This pod hosts the discovery front-end application | Yes, it will restart. | global.redis.passwordSecret | No | No | https://github.com/UrbanOS-Public/charts/tree/master/charts/discovery-streams | NA |
| = | = | = | ============================= | =============== | = | = | = | = | = |

