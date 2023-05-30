The UrbanOS platform uses kubernetes to host both the core microservice applications as well as external applications, such as Redis. The core applications can optionally be configured to connect to managed cloud services for external dependencies. If external dependencies are enabled and configured as part of the UrbanOS helm release, they will be managed under helm and hosted on the kubernetes cluster instead of a managed cloud service.

Core Microservice applications can be found here: https://github.com/UrbanOS-Public/smartcitiesdata
UrbanOS chart configuration can be found here: https://github.com/UrbanOS-Public/charts

This document aims to provide a summary of most kubernetes resources with a strong focus on dependencies between resources and how they are managed. These resources will differ based on how the platform is configured, so this document assumes all resources are hosted on the kubernetes cluster.

## Applications

This is a list of the various applications used in the UrbanOS platform. External applications can be managed via cloud or deployed along-side the core applications in the kubernetes cluster.

| Name | Description | Core/External | Required |
| - | - | - | - |
| Andi | Microservice used to host the front-end for adding and configuring datasets. Used by admin/curators of the system. | Core | Yes |
| Discovery-API | Microservice used to access information about datasets as well as the data itself | Core | Yes |
| Discovery-Streams | Microservice used to stream processed data over a websocket for subscription-based data consumption | Core | Yes |
| Discovery-UI | Microservice used to host the front-end for the data access portion of the system. Used by the end-user. | Core | Yes |
| Elastic Search | Search engine used for finding datasets in discovery | External | Yes |
| Forklift | Microservice used to write proceeded data from the kafka data layer into the persisted data stack | Core | Yes |
| Hive | A distributed, fault-tolerant data warehouse system that enables analytics at a massive scale | External | Yes |
| Kafka | An event streaming platform used to communicate domain-level events over an "event-stream" topic as well as distribute processed data between the core microservices. | External | Yes |
| Minio | A high-performance, S3 compatible object store. It is used as a kubernetes alternative to cloud buckets to store data. | External | No, but some S3 compatible interface is required |
| = |  ============================= | = | = |



## Pods

Pods are containers that host (docker) images, which isolate an environment to contain dependencies needed for an application. Generally, they are managed by a deployment, operator, or stateful set. External information can be connected to pods via environment variables and hard drives (Persistent Volume Claims).

| Name | Kubernetes Only? | Description | Associated Resources | Safe to delete? | Kustomized? | Managed by Operator? | Chart repository | Triage Flow/Troubleshooting Docs |
| - | - | - | - | - | - | - | - | - |
| andi-* | Yes | This pod hosts the ANDI front end  | **Redis:** Maintains entity state. <br> **Postgres:** Stores front-end UI state. <br> **Kafka:** Places messages on the event-stream to be read downstream. Also reads from the event stream. <br> **Auth0**: Connects to external Auth0 tenant for authentication. | Yes, it will restart | No | No | https://github.com/UrbanOS-Public/charts/tree/master/charts/andi | NA |
| discovery-api-* | Yes | This pod hosts the discovery API microservice. It can be directly queried via API externally or used by DiscoveryUI as a backend | **Kafka:** Reads from the event-stream to receive entity updates. <br> **Redis:** Maintains entity state. <br> **Auth0**: Connects to external Auth0 tenant for authentication. <br> **Elasticsearch**: Used to search for datasets. <br> **Presto**: Used to query data already saved to the system. | Yes, it will restart. | No | No | https://github.com/UrbanOS-Public/charts/tree/master/charts/discovery-api | NA |
| discovery-streams-* | Yes | This pod hosts a websocket service that can retrieve data. It can be directly queried via WS connection externally. | **Redis:** Stores entity state in viewstore. <br> **Kafka:** Used to receive the latest processed data to publish externally. | Yes, it will restart. | No | No | https://github.com/UrbanOS-Public/charts/tree/master/charts/discovery-streams | NA |
| discovery-ui-* | Yes | This pod hosts the discovery front-end application | **Redis:** Maintains entity state. <br> | Yes, it will restart. | No | No | https://github.com/UrbanOS-Public/charts/tree/master/charts/discovery-streams | NA |
| elasticsearch-master-* | No, can be cloud managed | This pod is at least one of the high-availability pods that host the elasticsearch server. | **discovery-api:** Service that connects to elasticsearch to search datasets. <br> | Yes, it will restart. | No | No | https://github.com/elastic/helm-charts | NA |
| forklift-* | Yes | This pod "lifts" processed data from kafka data layer into the data stack (Trino/Hive/Presto/Minio). | **Redis:** Maintains entity state. <br> **Kafka:** Reads from both the main event-stream for entity updates and the data layer for processed data. <br> **Presto:** Used to store processed data. <br> **Minio:** Used to manage the buckets that the data will be stored in. <br> | Yes, it will restart. | No | No | https://github.com/UrbanOS-Public/charts/tree/master/charts/forklift | NA |
| hive-metastore-* | No, can be cloud managed | This pod hosts the hive portion of the datastack (Trino/Hive/Presto/Minio). | **Minio(Optional):** Can be used as the required s3 connection that hive needs to manage data storage. <br> **Postgres**: Used to save metadata about trino. <br> | Yes, it will restart. | No | No | https://github.com/trinodb/charts | NA |
| kafka-exporter-* | No, can be cloud managed | This pod exports kafka metrics for prometheus. It is managed by the strimzi-kafka-operator. | **Kafka:** Monitors all kafka topics and consumers. | Yes, it will restart. | No | Yes: strimzi-kafka-operator | https://github.com/strimzi/strimzi-kafka-operator/tree/main/helm-charts/helm3/strimzi-kafka-operator | NA |
| kafka-scraper-cron-* | Yes | This pod executes a quick cronjob that logs the primary Kafka metrics. This is a stand-in for a cluster that does not have prometheus installed, but wants to monitor logs to determine errors. Since kafka metrics can indicate a backup of messages, this cronjob can serve as a way to unify application metrics with a log aggregator. | **Kafka Exporter:** Queries the exporter's endpoint to retrieve and log the current metrics. | Yes, but it will skip that cronjob | No | No | https://github.com/UrbanOS-Public/charts/tree/master/charts/kafka | NA |
| Minio-pool-* | No, any s3 compatible service can be used | This pod is a controller for the primary data storage layer. It is managed by the minio-operator and configured by the Minio Tenant. | **Trino:** Used by trino/hive to store data. <br> **Forklift**: Indirectly used by forklift (through Trino) to read/write data. <br> **Discovery-API:** Indirectly used by discovery-api (through Trino) to read data. <br> | Yes, it will restart. | No | Yes: minio-operator | https://github.com/minio/operator/tree/master/helm <br> | NA |
| Minio-operator-* | Yes | This pod hosts the minio operator, which uses the Tenant configuration to manage minio related resources and operations, such as establishing SSL certs, creating initial user and buckets, and creating secrets. | **Tenant:** Detects installed tenants and manages based on their configuration. | Yes, it will restart. | No | No, it is the operator. | https://github.com/minio/operator/tree/master/helm | NA |
| pipeline-entity-operator-* | Yes | This pod hosts the kafka entity operator, which monitors the cluster state to manage kafka topics and users. It is responsible for creating topics and users as configured in the chart. | **Kafka:** Ensures configured kafka topics are created and configured based on chart values. | Yes, it will restart. | No | No, it is the operator. | https://github.com/strimzi/strimzi-kafka-operator/tree/main/helm-charts/helm3/strimzi-kafka-operator | NA |
| = | = | ============================= | =============================== | = | = | = | = | = | = |

