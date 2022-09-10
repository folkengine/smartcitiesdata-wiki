# Running Multiple Microservices Locally:

### Notes

- Be sure you've setup elixir, erlang and docker prior to following this guide
  illustrating how to run application code. [Setup link](https://github.com/UrbanOS-Public/smartcitiesdata/wiki/Setup)

- A reminder that an architecture diagram is available [here](https://github.com/UrbanOS-Public/smartcitiesdata/blob/master/scdp_arch.png)

- When running multiple UrbanOS dependency containers, it's sometimes helpful to
  give the system docker service additional memory. My machine defaulted to 2GB,
  and upping to 6GB resulted in significantly less issues. 4GB is probably also
  reasonable. [Raising memory available to docker with Docker Desktop](https://forums.docker.com/t/how-to-increase-memory-size-that-is-available-for-a-docker-container/78483/2)

### Running Multiple Services

From the root of the repository, run:

1. `mix deps.get` to install elixir dependencies for all of the applications
   in the umbrella.

1. `docker-compose -f apps/e2e/test/whole-system.yml up` to startup all
   docker containers required by the UrbanOS Suite. (Kafka, Postgres, Elasticsearch, etc)

   - Reminder: If you'd like to run a single microservice instead of multiple,
     refer to that microservice README.md in `smarcitiesdata/apps/{microservice or library}/README.md`. You'll need significantly less containers running to test only
     one microservice.

1. With the docker containers now running, start the elixir apps by following
   the startup commands listed in each app's readme. Remember to skip any steps
   in application readmes that mention starting Docker, because we did that in the
   previous step for all apps.

   - The core 4 services at time of writing are: `Reaper`, `Alchemist`, `Valkyrie`
     and `Forklift`. These are the recommended core services to startup individually.

1. To setup datasets and ingestions for UrbanOS to fetch and store data, you'll
   need to also startup `Andi`. You'll need an Auth0 tenant configured for UrbanOS
   in order to run `Andi`. ([Setting up an Urban Auth0 Tenant](https://github.com/UrbanOS-Public/auth0-setup))

### Running Andi to describe datasets to UrbanOS

Now that UrbanOS core services are running, we'll use Andi to configure an
organzation, dataset, and ingestion. By the end of this section, data
from a remote source will be stored on a database in your system,
processed by UrbanOS.

We'll ingest [this piece of json data](https://github.com/bmitchinson/json-endpoint/blob/main/meters_ingestionA.json)
as an example, but feel free to configure an ingest for your own remote source.

Note: Chromium browsers work significantly better with Andi than webkit at time of writing.

[Andi User Guide](<https://github.com/UrbanOS-Public/smartcitiesdata/wiki/Data-Curator-Interface-(ANDI)-User-Manual>)

1. From the `smartcitiesdata/apps/andi/assets` directory, run `npm install`.

   - Node should have been installed as part of the [setup](https://github.com/UrbanOS-Public/smartcitiesdata/wiki/Setup)

1. Afterwards, Andi can be started with the startup command listed in the readme,
   that should look something like this: `AUTH0_CLIENT_SECRET="<auth_client_secret>" MIX_ENV=integration iex -S mix start`

   - auth_client_secret is the Client Secret in the Settings of the corresponding tenant configuration for the Andi application in Auth0.

1. Open a browser and visit Andi by going to https://127.0.0.1.xip.io:4443/datasets.
   Choose login in the top corner, and create a user with the Auth0
   dialog that pops up. Each time Andi is started, it's recommended that you logout
   and login again at application launch.

1. you'll notice the datasets, ingestions, and organizations index pages are empty.

1. Before you can create a dataset you need to [create an organization](<https://github.com/UrbanOS-Public/smartcitiesdata/wiki/Data-Curator-Interface-(ANDI)-User-Manual#managing-organizations>). You see your organization in
   the list at `/organizations` once saved.

1. With an organization, you're now able to create a dataset! Section 1 of the
   form will define it's metadata, and Section 2 will define the dataset schema. The
   dataset schema will align with the schema of the data you're fetching from a
   remote source, unless you opt to configure additional transformations.

   - [Help configuring attributes of a dataset in the Andi guide](<https://github.com/UrbanOS-Public/smartcitiesdata/wiki/Data-Curator-Interface-(ANDI)-User-Manual#creating--modifying-datasets>)
   - You should now see your dataset in the list!

1. The last Andi resource required to start to ingest data, is an "Ingestion". At
   time of writing, section 3 (transformations) of the create ingestion form is
   not yet complete. Fill out sections 1 2 and 4 to create an ingestion.
   [Guide on creating ingestions](<https://github.com/UrbanOS-Public/smartcitiesdata/wiki/Data-Curator-Interface-(ANDI)-User-Manual#creating--modifying-ingestions>)
   The simplest ingestion for getting started would be one with a single HTTP extract
   step, set as some static json.
   [Example URL](https://raw.githubusercontent.com/bmitchinson/json-endpoint/main/meters_ingestionA.json)

   - Section 4 configures the ingestion cadence. UrbanOS supports ingestion cadences
     around a maximum of every 2 seconds at time of writing, but something like 30
     seconds might be best for testing purposes to avoid filling your local database.
   - https://crontab.guru/ is good for debugging cadence declarations.
     - `*/30 * * * * *` represents every 30 seconds

Your configured data should now be flowing through the system, and being stored in an
internal database. In the next section, we'll show how to access this ingested
data from a front end website.

### Accessing ingested data from Discovery UI

todo
