![scdp architecture diagram](https://github.com/smartcitiesdata/smartcitiesdata/blob/master/scdp_arch.png?raw=true "scdp architecture")

The platform architecture is intended to be cloud native _and_ cloud agnostic. Standards are heavily favored in the design of components and features to ensure that anyone who adopts the system has flexibility in choosing their preferred vendor without fear of lock-in due to incompatibilities elsewhere. Any major public cloud provider or private cloud technology can leverage the technologies on which the Smart Cities Data project is built.

# Compute
All of the platform's compute resources are designed around Kubernetes, which allows the various micro services to scale both horizontally and vertically to meet your needs. Different micro services are built to operate concurrently and split workloads into parallel streams so that the number of service replicas needed can be increased with a single command and a few seconds to boot additional container processes.

For workloads requiring more raw processing power, container resource demands on the system can be adjusted as well to provide greater allotment of CPU and memory up to the limits of the Kubernetes workers.

Because Kubernetes is itself designed to scale, protracted periods of demand or just growth of the deployment over time means requesting more resources for the cluster is simply a matter of allocating additional worker nodes. The control plane continues to rebalance the distribution of container services across the cluster or even automatically scale the number of service replicas if demand thresholds are reached and resources (worker nodes and their associated CPU and memory) are available to do so.

## Micro services
The platform is built as a series of interconnected micro services for several reasons, chiefly centered around flexibility
* Contract: Only deploy the exact services you want or need and adjust the resources your cluster needs accordingly; if you won't use a feature, don't pay a cloud provider to run it
* Extend: All components communicate via message queues and a central event bus broadcasting system events that can be acted on in parallel and independently by different components. All communication is standardized on JSON so any new feature you need to add only has to communicate in a common format over the internal cluster network to plug right in
* Update: Each component is deployable independently meaning that each is more easily testable (with well defined application boundaries and contracts) and deployable. Any component with a ready update can go to prod without waiting on any other

# Storage
In order to store large and perpetually increasing volumes of data while maintaining a comparatively low cost, platform data at rest is stored in an object storage solution that provides the S3-standard interface. While this technology came out of Amazon's foundational cloud storage solution, all major commercial cloud providers have adopted the interface as a standard and even open source solutions for private clouds or on-prem deployments are available such as OpenStack's Swift project or Minio.

Object storage based on the S3 standard means that your storage is only limited by your provider's raw storage resources, which means practically unbounded when considering a cloud provider.

## PrestoDB
In order to provide an easy interface to the data spread across a massive S3 deployment, PrestoDB serves as a distributed SQL query engine that can keep track of platform data in the object store and map it to tables for structured interaction. PrestoDB builds on top of the knowledge gained from the Hadoop Hive sub-project and uses the Hive Metastore (itself backed by PostgreSQL) to maintain the mapping of highly compressed ORC files in S3 to queryable rows and columns.

# Kafka
As the de facto