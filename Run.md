# To Run The Application from the root locally (detailed):
  Note: These instructions were written followed on a Mac with smartcitiesdata(e2e:0.1.0, andi:0.64.3, reaper:0.24.9, discovery_api:0.51.4, valkyrie:1.6.5, forklift:0.17.9) and discovery_ui:1.0.0
  * Start the docker micro services needed for all apps in [smartcitiesdata](https://github.com/Datastillery/smartcitiesdata) by going inside the apps/e2e directory in your terminal and run:
    ```bash
    MIX_ENV=integration mix docker.start
    ```
  * Start the needed apps for smartcitiesdata by running these commands as explained in their readme:
    
    Note: I skip the step about starting Docker, because we did that in the previous step for all apps.
    * Open a new tab in your terminal and go inside the apps/andi directory in your terminal and run

      **If it's your first time**
        1. Create an Auth0 account at Auth0.com and create an Application for Andi.
        2. run
        ```bash
        sudo vim /etc/hosts
        ```
        give your password
        and add `127.0.0.1       127.0.0.1.xip.io` to the end of the file.
        ```
        cd assets
        npm i
        ```
        to install your node dependencies.

      **After that just run**
        ```bash
        AUTH0_CLIENT_SECRET="<auth_client_secret>" MIX_ENV=integration iex -S mix start
        ```
        auth_client_secret is the Client Secret in the Settings of the corresponding tenant configuration for the Andi application in Auth0.
    * Open a new tab in your terminal and go inside the apps/reaper directory and run
      ```bash
      MIX_ENV=integration iex -S mix
      ```
    * Open a new tab in your terminal and go inside the apps/discovery_api directory and
      1. Change the port on line 17 of apps/discovery_api/config/config.exs to 4001 since another app is using port 4000
      2. Then run
      ```bash
      MIX_ENV=integration iex -S mix start
      ```
    * Open a new tab in your terminal and go inside the apps/valkyrie directory and run
      ```
      MIX_ENV=integration iex -S mix
      ```
    * Open a new tab in your terminal and go inside the apps/forklift directory and run
      ```
      MIX_ENV=integration iex -S mix
      ```
    * For discovery_ui you might want to open a new terminal since it is a different [repo](https://github.com/SmartColumbusOS/discovery_ui) and run:

      **If it's your first time**
      ```
      npm install
      ```
      **After that just run**
      ```
      npm run start
      ```
      to actually start discovery_ui
  * Now that everything is up to see the operating system in action:
    
    * Open a browser and visit andi by going to https://127.0.0.1.xip.io:4443/datasets. If it is your first time you will need to login as a data curator (create a user in Auth0) and you won't see any datasets.
    * Before you can create a dataset you need to create an organization:
      * click 'ADD ORGANIZATION'
      * For Organization Title type 'City of Columbus'
      * For Logo URL type 'https://s3-us-west-2.amazonaws.com/prod-os-public-data/org-logos/city_of_columbus.png'
      * Organization Name should say 'city_of_columbus'
      * For Description type 'City of Columbus'
      * For Data JSON URL type 'https://opendata.columbus.gov/data.json'
      * Click 'Save'
    * Once you're done creating an organization you can create a dataset for that organization On the datasets page click 'ADD DATASET'
      * Type '2019 COTA Stop Ridership Ranking' For Dataset Title
      * Pick '09/14/2020' for Last Updated
      * Data Name should be '2019_cota_stop_ridership_ranking'
      * Type 'Central Ohio' for Spatial Boundaries
      * Type 'This dataset presents 2019 stop ridership rankings.' for Description
      * Type '2019' for Temporal Boundaries
      * Pick 'CSV' for Source Format
      * Pick 'Ingest' for Source Type
      * Pick 'City of Columbus' for Organization Title
      * Type 'Andrew Merrill' for Maintainer Name
      * Pick 'Public' for Level of Access
      * Type 'merrillaj@cota.com' for Maintainer Email
      * Pick your login email for Dataset Owner
      * Pick '09/14/2020' for the Release Date
      * Type 'https://www.cota.com/' for Homepage URL
      * Type 'irregular' for Update Frequency
      * Type 'https://creativecommons.org/licenses/by/4.0/' for License
      * Type 'COTA, ridership, transit, onboarding, stop, 2019' for Keywords
      * Pick 'Medium' for Benefit
      * Pick 'Low' for Risk
      * Click 'Save Draft'
      * Click 'Next'
      * You should be on the Data Dictionary.
      * Click the plus icon and type 'Year' for name, pick 'Integer' for type and pick 'TopLevel' for Child Of.
      * Click 'ADD FIELD'
      * Type a Description, for inspiration look at the Data Dictionary [for the dataset we're trying to replicate locally](https://discovery.smartcolumbusos.com/dataset/central_ohio_transit_authority/2019_cota_stop_ridership_ranking)
      * Pick 'None' for P.I.I
      * Pick 'N/A' for De-Identified
      * Pick 'None' for Demographic Traits
      * Pick 'No' for Potentially Biased
      * Repeat the last 7 steps 12 more times, but with
        * 'Month' for name and 'String' for type
        * 'DAY_OF_WEEK' for name and 'String' for type
        * 'UNIQUE_STOP_NUMBER' for name and 'Integer' for type
        * 'STOP_NAME' for name and 'String' for type
        * 'ON' for name and 'Integer' for type
        * 'OFF' for name and 'Integer' for type
        * 'TOTAL' for name and 'Integer' for type
        * 'LAT' for name and 'Float' for type
        * 'LONG' for name and 'Float' for type
        * 'TRIP_AVG_DWELL' for name and 'Float' for type
        * 'DAY_AVG_DWELL' for name and 'Float' for type
        * 'RANK' for name and 'Integer' for type
      * Click 'Save Draft'
      * Click 'Next'
      * You should be on Configure Ingest Steps.
      * Pick 'HTTP'
      * Click 'Add Step'
      * Pick 'GET' for Method
      * Pick 'https://scos-source-datasets.s3-us-west-2.amazonaws.com/2019_Jan_Dec+SRR.csv' for URL. This is where the data will come from! The Data Dictionary created earlier was based on the format of this csv file!
      * Click 'Test'. You should receive a success.
      * Click 'Save Draft'
      * Click 'Next'
      * You should be on Finalize.
      * Click 'Immediately'
      * Click 'Save Draft'
      * Click 'Publish'
    * This will publish our Dataset. Reaper will gather it, Valkyrie will normalize it and Forklift will persist it.
    * Now if you go to http://localhost:9001/ you should see one dataset called 2019 COTA Stop Ridership Ranking with File Type: CSV.
    * If you click on the link you should see the details of the dataset that should look like the dataset we [copied](https://discovery.smartcolumbusos.com/dataset/central_ohio_transit_authority/2019_cota_stop_ridership_ranking).
    * However, if you click on 'Write SQL' you will not see any records.
    * Forklift created two tables in the presto database(docker micro service): city_of_columbus__2019_cota_stop_ridership_ranking and city_of_columbus__2019_cota_stop_ridership_ranking__json. city_of_columbus__2019_cota_stop_ridership_ranking__json has the uncompacted data from the csv file and city_of_columbus__2019_cota_stop_ridership_ranking doesn't have any records. Before we can query the dataset in discovery_ui we will need to run compaction. We will do it manually, but it is also possible to create a job to do it.
    * ON the tab running Forklift hit enter to see the interactive shell then run
      ```Forklift.DataWriter.compact_dataset(Forklift.Datasets.get!(<dataset_id>))```
      You can find the <dataset_id> in the url of the address bar of andi when you click on the dataset. The url should be https://127.0.0.1.xip.io:4443/datasets/<dataset_id>.
    * This will run compaction and now city_of_columbus__2019_cota_stop_ridership_ranking will have the ingested data and city_of_columbus__2019_cota_stop_ridership_ranking__json will be empty.
    * Now if you click on 'Write SQL' you will see the records.
    * If you want you can click 'Visualize' and create a visualization.
    * If you want to save your work you can click the save button.
    * If you want to see your saved visualizations you can click on the folder.
  * To trouble shoot:
    * The first thing I would do look at output of all my terminals to see if there are any error messages.
    * Then I would check the health of my pods
      ```bash
      docker ps
      ```
      If any of them say unhealthy it's a sign to look to look in there.
      You can do that with:
      ```bash
      docker logs <id_of_container>
      ```
      You can also inspect the container with:
       ```
       docker inspect <id_of_container>
       ```
    * If you can't see the dataset in discovery_ui after publishing the dataset I would wait a minute (longer depending on the size of the dataset) to make sure forklift was done. After that if I still can't see it I would check to see if the dataset is in presto:
      * ```bash
        docker exec -it <id_of_presto_container> presto
        ```
        This will get you inside the presto command line interface. Then run:
        ```bash
        use hive.default;
        ```
        This the catalog and schema forklift uses to create tables. Then I would run:
        ```bash
        show tables;
        ```
        If you can't find the tables then forklift did not create a table for the data in the dataset and I would move on to the step below where we exec into the Kafka container.

        If you are doing this for the first time then it should be easy to spot the tables. However, if you've ingested a lot of datasets and are coming back for reference you can also use the vim search command shortcut to find your table: '/<my_expected_table_name>'.
      
        The name of the tables forklift creates for a dataset is

        <org_name_the_dataset_belongs_to>__<dataname_of_the_dataset> and

        <org_name_the_dataset_belongs_to>__<dataname_of_the_dataset>__json.

        So in our example above it would be 'city_of_columbus__2019_cota_stop_ridership_ranking' and 'city_of_columbus__2019_cota_stop_ridership_ranking__json'.

        If you do see the tables then you should be able to see the the dataset in discovery_ui. If you can't I would suggest debugging discovery_ui.
    * If you can see the dataset in discovery_ui after publishing the dataset, but can't query the table in the write sql page then I would
      check to see if forklift compacted the table.

      If the city_of_columbus__2019_cota_stop_ridership_ranking table doesn't have any records and city_of_columbus__2019_cota_stop_ridership_ranking__json does then I would manually run compaction inside the forklift terminal by hitting the enter key and run: 
      ```bash
      Forklift.DataWriter.compact_dataset(Forklift.Datasets.get!(<dataset_id>))
      ```
      as described earlier.

      If compaction passes then city_of_columbus__2019_cota_stop_ridership_ranking__json should be empty and city_of_columbus__2019_cota_stop_ridership_ranking should now have the data.
        
        Now, if you go to discovery_ui you should be able to query the table.
      
    * Check the kafka topics:
      * If you can't find the table for your data in presto then I would check the Kafka topics:
        ```bash
        docker exec -it <id_of_kafka_container> bash
        ```
        Inside there I would `cd /opt/kafka`. Then I would check the dead letter queue with:
        ```bash
        bin/kafka-console-consumer.sh --bootstrap-server localhost:9092 --from-beginning --topic streaming-dead-letters
        ```
        If you see messages that have your data in it then you know that some application 'yeeted'(that's what we call it, because we are not really deleting the message) the message by sending it to the dead-letter-queue this should have created some logs in that app.
        
        Hence, Forklift never got it and that's why your tables don't exist in presto.
        
        If you don't see your data in the dead letter queue then it should be in the streaming-persisted topic. You can check that by running the same command as before, but with streaming-persisted instead of streaming-dead-letters.
        
        ```bash
        bin/kafka-console-consumer.sh --bootstrap-server localhost:9092 --from-beginning --topic streaming-persisted
        ```

        If you don't see it in either topic I would make sure you published the dataset in Andi.
  * When you're done you can kill the docker micro services inside the e2e directory by running:
    ```bash
    MIX_ENV=integration mix docker.kill
    ```

# To Run The Application from the root locally in minikube (high level):
  * Start minikube:
    ```bash
    minikube start --kubernetes-version v1.15.0 --memory 6144 --cpus 4
    ```
  * Initialize helm:
    ```bash
    helm init
    ```
  * Add datastillery charts repository:
    ```bash
    helm repo add scdp https://datastillery.github.io/charts
    ```
  * Add strimzi charts repository:
    ```bash
    helm repo add strimzi https://strimzi.io/charts
    ```
  * Add bitnami charts repository:
    ```bash
    helm repo add bitnami https://charts.bitnami.com/bitnami
    ```
  * Add hashicorp charts repository:
    ```bash
    helm repo add hashicorp https://helm.releases.hashicorp.com
    ```
  * To search charts from all the repositories added locally:
    ```bash
    helm search
    ```
  * To search charts from specific repository:
    ```bash
    helm search <REPOSITORY_NAME>
    ```
  * To install chart and deploy application:
    ```bash
    helm upgrade --install <RELEASE> <REPOSITORY_NAME>/<CHART_NAME>
    ```
    or
    ```bash
    helm upgrade --install <RELEASE> <REPOSITORY_NAME>/<CHART_NAME> --namespace=<NAMESPACE>
    ```
    if you want to install it in a specific namespace. Uses default namespace if not specified.
  * To verify pods has been created succesfully:
    ```bash
    kubectl get pods -n <NAME_SPACE>
    ```
  * To describe the pod:
    ```bash
    kubectl describe pod <POD_NAME> -n <NAME_SPACE>
    ```
  * To execute the pod:
    ```bash
    kubectl exec -itn pod <NAME_SPACE> <POD_NAME> --bash
    ```
  * To execute app inside the pod:
    ```bash
    /app/bin/<APP_NAME> remote_console
    ```
  * To execute module inside the app:
    ```bash
    <MODULE_NAME>.<FUNCTION_NAME>
    ```
  * To exit the app:
    ```bash
    CTRL+C
    ```
  * To exit the pod:
    ```bash
    exit
    ```
  * To delete minikube:
    ```bash
    minikube delete
    ```