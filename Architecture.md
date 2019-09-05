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
As one of the leaders in distributed message queue technology, Apache Kafka is the workhorse sending messages from one component of the Smart Cities Data platform to another. Kafka allows for relatively simple cluster setup compared to alternatives and has high throughput capacity with numerous avenues for parallel processing available between various configuration options. Because of its comparative cluster simplicity and topology, the Kafka cluster can also scale and contract more gracefully as the needs of the platform change.

# Services
The primary business logic of the Smart Cities Data platform is entirely homegrown and each micro service is written in the functional Elixir programming language. Elixir was chosen because of the capabilities of the underlying Erlang Virtual Machine for high concurrency and reliability, meaning the platform, once stood up, would require less maintenance and fewer hands to support by an operations team. As a functional language, Elixir also provides a wide array of built-in and third party libraries for transforming and processing data, which is a perfect fit for the needs of the platform.

## Administer and Manage
The administration application is a web application built in Elixir's Phoenix web framework. The admin application allows for the creation and updating of datasets and the organizations that provide and maintain them via a RESTful API.

New datasets and organizations posted to the administrative interface fire off the creation of events in the central event stream for other components of the system to perform ingestion and user display tasks.

See the [repo](https://github.com/smartcitiesdata/andi) or [andi](https://github.com/smartcitiesdata/smartcitiesdata/wiki/Names#andi) in the application names section for an explanation.

## Gather and Ingest
Once a dataset has been defined by the administrative interface, it must be gathered and parsed onto a message queue. The [Reaper](https://github.com/smartcitiesdata/reaper) application reads the cadence by which data is to be retrieved from a given dataset, as well as the format and transfer protocol to expect the data over and then downloads the data or reads it from the remote source on that scheduled cadence.

At the appointed interval, Reaper broadcasts an event into the event stream that an ingestion is starting for a given dataset and writes the data, one record per message, to a dedicated message queue. Once all data for that interval has been consumed, another event is broadcast to signal ingestion has completed.

See [reaper name](https://github.com/smartcitiesdata/smartcitiesdata/wiki/Names#reaper) explained.

## Normalize and Validate
Data that has been read into the system in its raw form must then be validated against the expected schema defined in its definition record and any data that deviates from that schema must be normalized or discarded. The [Valkyrie](https://github.com/smartcitiesdata/valkyrie) component sits behind the ingestion component and performs these tasks. If a given message contains data that does not match the form indicated in the definition schema, but that data can be reasonably coerced to the proper form (a `"1"` (string) was written when the schema indicated a `1` (integer) was expected), Valkyrie takes care of this before passing the message onto another dedicated message queue.

The [valkyrie name](https://github.com/smartcitiesdata/smartcitiesdata/wiki/Names#valkyrie) explained.

## Custom Transformations
When individual datasets need custom transformations applied to them, [Voltron](https://github.com/smartcitiesdata/voltron) applies these transformations by reading a list of transformation functions off the dataset definition and then applying them in order against each message of the dataset coming across the message queue. These transformations can be internally pre-defined or indicate external lambda-style functions provided as a service. Transformed messages are once again written to a queue dedicated to their specific dataset after appending basic performance (timing) metrics from the transformation operations performed.

The [voltron name](https://github.com/smartcitiesdata/smartcitiesdata/wiki/Names#voltron) explained.

## Persist
Messages finished with the ingestion, validation, and transformation process are then persisted to the S3-backed Presto datastore by [Forklift](https://github.com/smartcitiesdata/forklift) via SQL insertions of batched records.

Forklift gathers messages into 1 MB chunks and then inserts them into Presto, periodically halting insertion for datasets in order to compact their underlying ORC files for search and storage optimization; creating a smaller number of larger files.

The [forklift name](https://github.com/smartcitiesdata/smartcitiesdata/wiki/Names#forklift) explained.

## Socket API
While all records are inserted into the data store, streaming records are also re-published to a web socket for external consumers to take advantage of the real time updates to the data once they've been sanitized or transformed by the platform. This is done by another Elixir Phoenix web application, [Discovery Streams](https://github.com/smartcitiesdata/discovery_streams), taking advantage of Phoenix's native first-class support for web sockets. The data messages are read off the same queue Forklift draws from and pushes them to a dedicated web socket.

The [discovery application names](https://github.com/smartcitiesdata/smartcitiesdata/wiki/Names#discovery-apistreamsui) explained.

## Profile
The overall platform itself generates valuable insights about the data it processes just in the course of that processing and this is calculated by [Flair](https://github.com/smartcitiesdata/flair) based on timestamps and other observations of the data inserted periodically throughout the pipeline.

Flair performs regular inserts of the resulting data into metadata tables also stored in S3 via the PrestoDB engine.

The [flair name](https://github.com/smartcitiesdata/smartcitiesdata/wiki/Names#flair) explained.

## REST API
The primary interface for searching and connecting the data is the [Discovery API](https://github.com/smartcitiesdata/discovery_api) component that provides the backbone for the user web interface. Another Elixir Phoenix web application, it handles the streaming the download of datasets to the user's browser, querying, and routing user requests as well as an authentication and authorization gate for preventing public access to datasets within the system that are restricted to members of the owning organization or other privileged users.

The [discovery api name](https://github.com/smartcitiesdata/smartcitiesdata/wiki/Names#discovery-apistreamsui) is covered with the other related components.

## User Interface
The visual overlay to the REST api the user interacts with. [Discovery UI](https://github.com/smartcitiesdata/discovery_ui) is a React Javascript application and built to be responsive and modular, making heavy use of the React components model for structuring apps. The entirety of the Smart Cities Data platform customized user interface is made up of composable components and integrates common visualization libraries for working with the data returned by the API directly in the user's browser window.

The [discovery ui name](https://github.com/smartcitiesdata/smartcitiesdata/wiki/Names#discovery-apistreamsui) is likewise explained with its related components.

## Extensibility Switchboard
With the data flowing through the system in dedicated queue lanes, available from streaming web sockets and RESTful APIs, not to mention the full scope of the systems internals like the S3 object store, SQL engine, key value stores, and Kubernetes components, the concept of the Extensibility Switchboard is really meant to leave space for anyone to add additional components to the system and interact with the data where and how it makes the most sense.

One such component is the [Odo](https://github.com/smartcitiesdata/odo) application ([name](https://github.com/smartcitiesdata/smartcitiesdata/wiki/Names#odo) explained)which listens to the event stream for notice of Shapefile geospatial data files being loaded into the S3 backend directly (files not in a format that can be broken into discreet messages and loaded into Presto) and immediately converts them to the more flexible GeoJson format that _can_ be loaded as well as queried. Because it acts on event stream notices and interacts directly with the S3 store it sits outside the ETL pipeline.

The sky is really the limit and you can add as many components as your Kubernetes cluster can handle in whatever language you like so long as it can consume event notification messages from Kafka. You may insert another stage into the pipeline to scrub PCI data from your incoming records, or listen for updates to very specific datasets and re-process their raw records based on some algorithm before re-inserting the results into their own table (much as Spark is used in a Hadoop cluster).

## Event Stream
The event stream is the central hub used for all the components of the platform to communicate without having to be directly coupled to each other. Everything subscribes to the event stream message queue and when it takes action in the system, either by user input or external stimulus, broadcasts an event others components are programmed to listen for and respond to.

The event stream is build on top of Kafka as well and uses a Redis backend for each application to be able to store a persistent record of its own "view" of the state of the system based on the events received.