# Two hard things in computer science...
Like any good software project, the Smart Cities Data platform is built on a healthy amount of memes. One side effect is that the names of the various sub-projects can be a little obscure.

What follows is an explanation of the library and microservice names that make up the project.

## [Andi](https://github.com/smartcitiesdata/smartcitiesdata/tree/master/apps/andi)
Andi is the microservice providing a RESTful API for administrative management of the data and organizations stored within the platform. Andi is an acronym, standing for "Administrative New Data Interface" and was chosen because of the original project's user persona developed for a theoretical system administrator, named Andy.

## Carpenter (deprecated)
Carpenter is a microservice that creates tables in the database.  _*rimshot*_

## Discovery [API](https://github.com/smartcitiesdata/discovery_api)/[Streams](https://github.com/smartcitiesdata/discovery_streams)/[UI](https://github.com/smartcitiesdata/react_discovery_ui)
The three complementary microservices providing end users with access to interact with the data in the platform. They allow programmatic access via a RESTful API, provide persistent web sockets for streaming data, and a React-based graphical web interface respectively, allowing users to discover the data and insights from it (this one's pretty straightforward).

## [Divo](https://github.com/smartcitiesdata/divo)
An acronym standing for "Docker Integration and Validation Orchestrator"; Divo is a library for providing integration testing functionality of Elixir applications via the Docker and Docker-Compose tools and orchestrating them natively within the Elixir Mix tool.

## [Flair](https://github.com/smartcitiesdata/smartcitiesdata/tree/master/apps/flair)
Flair is a microservice that analyzes dataset ingestion operations and statistically profiles them before writing the results to a metadata table within the platform storage backend. It is named for the WWE star Ric Flair because it "styles and profiles" the data.

## [Forklift](https://github.com/smartcitiesdata/smartcitiesdata/tree/master/apps/forklift)
Forklift is a microservice that lifts batches of data messages off the ingestion pipeline and into the S3 storage backend via the PrestoDB SQL engine, much as a forklift lifts pallets of materials into place.

## Genesis (deprecated)
Genesis is the "creator" of Kafka topics for publicizing streaming datasets to a web socket. It is named for the 80's British Rock band of the same name.

## [KDP](https://github.com/smartcitiesdata/kdp)
A Kubernetes deployment of the microservices and software components that make up the platform's storage backend (PrestoDB coordinator and workers, Hive Metastore, PostgreSQL database, and Minio S3 object store). It is an acronym standing for _Kubernetes Data Platform_ and was named in response to the _Hortonworks Data Platform_ it replaced on the project when the team pivoted to a more cloud native implementation of a big data storage engine.

## [Odo](https://github.com/smartcitiesdata/odo)
Odo is a microservice that converts shapefiles (a common geospatial data format) into geojson (another common geospatial data format, but easier to work with). Because the service changes the shape of files, it is named for the shapeshifting alien of the same name from the television series Star Trek: Deep Space 9

## Prestige
A library providing an Elixir driver for the PrestoDB SQL engine. It is named for the word's similarity to the Presto name and for the 206 movie of the same name staring Hugh Jackman and Christian Bale (itself a novel by Christopher Priest and a reference to the "third act of every magic trick")

## Reaper
Reaper is the microservice that sits at the outer edge of the platform and retrieves data from various sources and formats, initially downloading it and writing it into the ingestion pipeline. It is named for the farming and harvesting term meaning "to gather" the data for consumption.

## Smart City
A central library for standardizing logic specific to the domain of the platform across its many microservices.

## Smart City Data
A central library for standardizing the logic and structure around the data messages that flow through the platform ingestion pipeline. From a period of early rapid iteration and experimentation when overly centralizing library logic caused the team to block itself updating the same shared code concurrently.

## Smart City Test
A library developed specifically for standardizing the testing process of the various platform microservices.

## Streaming Metrics
A library developed to facilitate the gathering of internal data points for health and performance monitoring of various platform components, standardized across the Kubernetes containerized workloads and the cloud provider's infrastructure. Not especially esoteric but it does carry the "streaming" name from the period of the platform's evolution when streaming data processing was still disconnected from legacy handling of static datasets.

## Streisand
Streisand is a microservice that reads messages specifically from streaming datasets off the end of the ingestion pipeline (just prior to their being uploaded to the storage backend) and sends them to be streamed back out of the system via web socket. Because it "publicizes" data, it is ironically named for the [Streisand Effect](https://en.wikipedia.org/wiki/Streisand_effect), or the tendency for information on the Internet desired to be hidden or removed to be more actively shared and publicized as a result of attempted obfuscation.

## Valkyrie
Valkyrie is a microservice that processes data messages and validates or normalizes them according to the schema that was supplied as part of their dataset's definition. Messages that are too distorted from their expected format are discarded. The service was named for the Norse mythological female warriors who judged the dead for worthiness to be accepted into the glorious afterlife of warriors, Valhalla.

## Voltron
Voltron is a microservice used to apply arbitrary transformations against dataset messages based on a list of transformation functions supplied with the dataset definition for those messages. Like the eponymous mecha from the classic Japanese cartoon of the same name, it is an aggregation of "transformers".

## Yeet
A library for standardizing platform logic around sending data messages that cannot be properly processed to a "dead letter queue". A totally safe search on UrbanDictionary.com for the term yields the definition: "to discard an item at high velocity".
