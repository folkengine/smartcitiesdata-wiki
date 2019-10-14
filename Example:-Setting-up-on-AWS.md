# AWS Setup Example

This is an example of setting up the platform on AWS, using AWS services in place of Kubernetes services.

## User Setup
1. Open IAM
1. Create user
1. Create programmatic [access keys](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_users_create.html#id_users_create_console)
1. Store them under default profile in `~/.aws/credentials`.
    ```bash
    aws configure
    ```

## Platform setup
### EKS
1. Install [eksctl](https://eksctl.io/introduction/installation/).
1. Create cluster (This step could take a while. ~10+minutes)
    ```bash
    eksctl create cluster --name <INSERT-NAME-HERE> \
           --version 1.13 \
           --nodegroup-name standard-workers \
           --node-type t2.large \
           --nodes 5 \
           --nodes-min 1 \
           --nodes-max 8 \
           --node-ami auto
    ``` 

   If you get a `SignatureDoesNotMatch` error you need to verify that your aws credentials are correct.  You can delete and regenerate them and run `aws configure` again.

1. Setup for helm usage

    ```yaml
    #tiller.yaml
    ---
    apiVersion: v1
    kind: ServiceAccount
    metadata:
      name: tiller
      namespace: kube-system
    ---
    apiVersion: rbac.authorization.k8s.io/v1beta1
    kind: ClusterRoleBinding
    metadata:
      name: tiller
    roleRef:
      apiGroup: rbac.authorization.k8s.io
      kind: ClusterRole
      name: cluster-admin
    subjects:
    - kind: ServiceAccount
      name: tiller
      namespace: kube-system
    ```

   Run the commands below

    ```bash
    kubectl apply -f tiller.yaml
    helm init --service-account tiller
    helm repo add scdp https://smartcitiesdata.github.io/charts
    ```

#### EKS for other users
The setup above will allow you to access the EKS cluster, but some steps need to be taken to
enable others to access it too.

1. Create role (IAM > Roles > Create role)
   * Trusted entity = Another AWS account
     * Account ID: $YOUR_ACCOUNT_ID
     * Name and description
     * Capture this created Role ARN for later (`$NEW_ROLE_ARN`)
1. Create an `iamidentitymapping` in your EKS cluster.
    ```bash
    eksctl create iamidentitymapping \
           --name $CLUSER_NAME \
           --role $NEW_ROLE_ARN \
           --group system:masters \
           --username admin
    ```
1. Anyone you wants to use the role to access your cluster must edit their `~/.aws/credentials`.

    ```
    [aws-user-profile]
    aws_access_key_id = $USER_KEY
    aws_secret_access_key = $USER_SECRET

    [eks-admin]
    role_arn = $NEW_ROLE_ARN
    source_profile = aws-user-profile
    ```

1. Users will need to have the correct `KUBECONFIG` setup.

    ```bash
    eksctl utils write-kubeconfig --name $CLUSTER_NAME
    ```

1. Users must use the new AWS profile to access the system.

    ```bash
    export AWS_PROFILE=eks-admin
    kubectl get pod # works
    ```

### Redis
Redis is used to store dataset metadata and microservice view states.

1. Go to [ElastiCache](https://us-east-2.console.aws.amazon.com/elasticache/home?region=us-east-2)
1. Select Redis, then Create
     - version 5.0.4
      - default parameter group
      - port 6379
      - 0 replicas
      - Create new subnet group
        - Select EKS VPC
        - Select EKS private subnets
        - Edit security group and 
            - Select EKS standardworker and sharednetwork security groups
            - Unselect the default one
        - Set backup policy per your wishes
1. Wait until the cluster is available. (This could take ~10 min)
    Take note of the redis primary endpoint and add it to the redis.yaml in the next step. Note: Do not add the port
1. Create =redis.yaml=
    ```bash
    # redis.yaml
     kind: "Service"
    apiVersion: "v1"
    metadata:
      name: "redis"
    spec:
      type: ExternalName
      externalName: $REDIS_URI_NO_PORT
    ```
1. Run command to apply to your EKS
    ```bash
    kubectl apply -f redis.yaml
    ```
1. Test
    ```bash
    kubectl run --image=redis:5.0.4 redis-client
    pod_name=$(kubectl get pod -l run=redis-client -o name | cut -d'/' -f2)
    kubectl exec $pod_name -- redis-cli -h redis set foo bar # OK
    kubectl exec $pod_name -- redis-cli -h redis get foo     # bar
    kubectl exec $pod_name -- redis-cli -h redis del foo     # 1
    kubectl delete deployment redis-client
    ```
### LDAP
LDAP is currently used to authenticate/authorize users and orchestrate organizations. It will be replaced with OAuth
in the near future. There are also plans to support SAML.

1. Install LDAP chart
    ```bash
    helm install --name ldap stable/openldap \
        --set adminPassword=ThisIs4n4dminP4ssword \
        --set env.LDAP_BASE_DN="dc=example,dc=org"
    ```
1. Port forward to LDAP (Continuous running process)
    ```bash
    kubectl port-forward svc/ldap-openldap 38900:389
    ```
1. Using ApacheDirectoryStudio 
    1. create new connection
        - localhost:38900
        - Use the following dn: cn=admin,dc=example,dc=org/
            - with password: ThisIs4n4dminP4ssword
    1. Add new entry under dc=example,dc=org
        - RDP:ou Value: orgs 
        - cn=admin,dc=example,dc=org/ThisIs4n4dminP4ssword
1. Add LDAP secrets
    ```yaml
    # ldap.yaml
    apiVersion: v1
    kind: Secret
    metadata:
      name: ldap
    type: Opaque
    data:
      host: bGRhcC1vcGVubGRhcA== # ldap-openldap
      base_dn: ZGM9ZXhhbXBsZSxkYz1vcmc= # dc=example,dc=org
      user: Y249YWRtaW4= # cn=admin
      password: VGhpc0lzNG40ZG1pblA0c3N3b3Jk # ThisIs4n4dminP4ssword
      environment_ou: b3Jncw== # orgs
    ```
1. Run

    ```bash
    kubectl apply -f ldap.yaml
    ```
### Kafka
Kafka is a distributed log used to pass messages and events between decoupled microservices. Strimzi is an 
operator that makes deploying/running Kafka in Kubernetes easier.

1. Deploy strimzi
    ```bash
    helm repo add strimzi https://strimzi.io/charts
    helm upgrade --install strimzi-kafka-operator strimzi/strimzi-kafka-operator --version 0.08.0
    ```
1. Create Kafka cluster
    ```yaml
    # kafka.yaml
    apiVersion: kafka.strimzi.io/v1alpha1
    kind: Kafka
    metadata:
      name: streaming-service
    spec:
      kafka:
        replicas: 1
        listeners:
          plain: {}
          tls: {}
        config:
          offsets.topic.replication.factor: 3
          transaction.state.log.replication.factor: 3
          transaction.state.log.min.isr: 2
        storage:
          type: ephemeral
      zookeeper:
        replicas: 3
        storage:
          type: ephemeral
      entityOperator:
        topicOperator: {}
        userOperator: {}
    ```
1. Apply
    ```bash
    kubectl apply -f kafka.yaml
    ```
1. Create topics
    1. Example 1
    ```yaml
    # topics.yaml
    ---
    apiVersion: kafka.strimzi.io/v1alpha1
    kind: KafkaTopic
    metadata:
      name: streaming-dead-letters
      labels:
        strimzi.io/cluster: streaming-service
    spec:
      partitions: 1
      replicas: 1
      config: {}
    ```
   1. Example 2
    ```yaml
          ---
          apiVersion: kafka.strimzi.io/v1alpha1
          kind: KafkaTopic
          metadata:
            name: event-stream
            labels:
              strimzi.io/cluster: streaming-service
          spec:
            partitions: 1
            replicas: 1
            config: {}
    ```
1. Apply 
    ```bash
    kubectl apply -f topics.yaml
    ```
### Andi
Andi is the data curation/definition API. Organizations and datasets are created through it.

1. Deploy andi
    ```bash
    helm upgrade --install andi scdp/andi \
         --set image.tag=$ANDI_VERSION \
         --set redis.host=redis \
         --set strimzi.kafka.brokers=streaming-service-kafka-bootstrap:9092 \
         --set service.type=ClusterIP
    ```
1. Test
    ```bash
    kubectl port-forward svc/andi 8080:80
    curl -sfL http://localhost:8080/api/v1/datasets # []
    ```
   Verify the terminal which is running the port forwarding logs a handled connection
   
### API
The discovery-api is the programmatic entry point to data access. It is a REST API for searching datasets, viewing 
their details, and querying their historical (batch) data.

1. Deploy guardian key
    ```yaml
    # guardian.yaml
    apiVersion: v1
    kind: Secret
    metadata:
      name: guardian
    type: Opaque
    data:
      secret_key: ce1RV/hBmrFVh11geYV4wPWvZ9vOF1en/tZTD/Qwlr27UmJka+Qi+jbFAZLu7FAu
    ```
    ```bash
    kubectl apply -f guardian.yaml
    ```
1. Deploy discovery-api
    ```bash
    helm upgrade --install discovery-api scdp/discovery-api \
     --set image.tag=$DISCOVERY_API_VERSION \
     --set redis.host=redis \
     --set presto.url=http://kdp-kubernetes-data-platform-presto:8080 \
     --set ldap.host=ldap-openldap,ldap.base="dc=example,dc=org" \
     --set s3.hostedFileBucket=hosted-bucket,s3.hostedFileRegion=us-east-2 \
     --set vault.endpoint="" \
     --set environment=demo \
     --set service.type=LoadBalancer
    ```
1. Test
    ```bash
    kubectl port-forward svc/discovery-api 8080:80
    curl -sfL localhost:8080/api/v1/dataset/search?limit=10 # Returns JSON with 200 
    ```
    Verify the terminal which is running the chained port forwarding logs a handled connection
### UI
The discovery-ui is the frontend for dataset searches and details.

1. Deploy
    ```bash
    helm upgrade --install discovery-ui scdp/discovery-ui \
         --set image.environment=demo,image.tag=$DISCOVERY_UI_VERSION \
         --set env.api_host="$DISCOVERY_API_URL" \
         --set env.base_url="$DISCOVERY_UI_URL" \
         --set service.type=LoadBalancer
    ```

### KDP
KDP is the platform's PrestoDB setup.

1. Create S3
    
    ### Using policy generator
     $EKS_WORKER_ROLE_ARN -> https://console.aws.amazon.com/iam/home?#/roles -> nodegroup-standar-NodeInstanceRole
     
     $BUCKET_ARN -> https://s3.console.aws.amazon.com/s3/home?region=us-east-2# -> Check box of bucket -> copy bucket arn
    - Select s3 Bucket Policy
    - Encrypt with AES-256
    - Make private
    - Access policy statement 1
      - Principal: $EKS_WORKER_ROLE_ARN
      - Actions: DeleteObject, DeleteObjectVersion, GetObject, PutObject
      - Resource: $BUCKET_ARN/#
    - Access policy statement 2
      - Principal: $EKS_WORKER_ROLE_ARN
      - Actions: ListBucket
      - Resource: $BUCKET_ARN
    1. Generate policies and add them to the s3 buckets permissions
  
    ### RDS
    - Postgres 10.6-R1 (dev/test template)
    - database id (devs choice)
    - username:postgres
    - password:<insert-password-here>
    - Burstable db.t3.medium (100GB General Purpose SSD)
    - No autoscaling
    - EKS VPC w/new private subnet group
    - EKS shared node security group
    - Initial database name "metastore"
    - Backups/encryption/maintenance as preferred
    - Default for everything else
1. Configure
    ```yaml
    # kdp.yaml
    global:
      environment: demo
      objectStore:
        bucketName: $CREATED_BUCKET_NAME << Update this
        accessKey: null
        accessSecret: null
    presto:
      workers: 2
      ingress:
        enabled: false
      jvm:
        maxHeapSize: 1536M
      deploy:
        container:
          resources:
            limits:
              memory: 2Gi
              cpu: 2
            requests:
              memory: 2Gi
              cpu: 1
    hive:
      enabled: false
    minio:
      enabled: false
    postgres:
      enabled: false
      service:
        externalAddress: $METASTORE_ADDRESS
      db:
        name: metastore
        user: postgres
        password: ""
    metastore:
      allowDropTable: true
      timeout: 360m
    ```
1. Deploy
    ```bash
    helm upgrade --install kdp scdp/kubernetes-data-platform \
         --values kdp.yaml \
         --set postgres.db.password=$DB_PASSWORD
    ```
1. Test
    ```bash
    pod_name=$(kubectl get po -l role=coordinator -o name | cut -d/ -f2)
    kubectl exec -it $pod_name -- presto --catalog hive --schema default
    ```

### Forklift
Forklift is a service for loading data via PrestoDB into object storage. It also runs table compaction and will replace `carpenter` as the table creation/alteration/deletion service.
1. Deploy
   ```bash
   helm upgrade --install forklift scdp/forklift \
        --set image.tag=$FORKLIFT_VERSION \
        --set kafka.brokers=streaming-service-kafka-bootstrap:9092 \
        --set kdp.url="http://kdp-kubernetes-data-platform-presto:8080" \
        --set redis.host=redis \
        --set prometheus.scrape=false
   ```
2. Test
   ```bash
   pod_name=$(kubectl get po -l app.kubernetes.io/name=forklift -o name | cut -d/ -f2)
   kubectl exec -it $pod_name -- bin/forklift rpc 'Prestige.execute("show tables") |> Prestige.prefetch' # []
   ```
### Reaper
Reaper gathers (extracts) data to be stored in the platform.
1. Deploy pods

   ```bash
   helm upgrade --install reaper scdp/reaper \
        --set vault.endpoint="" \
        --set strimzi.kafka.brokers=streaming-service-kafka-bootstrap:9092 \
        --set redis.host=redis \
        --set image.tag=$REAPER_VERSION
   ```

2. Deploy service

   ```yaml
   # reaper.yaml
   apiVersion: v1
   kind: Service
   metadata:
     name: reaper
     labels:
       app: reaper
   spec:
     selector:
       app.kubernetes.io/name: reaper
     ports:
     - protocol: TCP
       port: 80
       targetPort: 4001
       name: tcp-80
     type: LoadBalancer
   ```

   ```bash
   kubectl apply -f reaper.yaml
   ```

### Valkyrie
Valkyrie is a microservice to standardize/normalize data within a dataset.

1. Deploy
   ```bash
   helm upgrade --install valkyrie scdp/valkyrie \
        --set kafka.brokers=streaming-service-kafka-bootstrap:9092 \
        --set redis.host=redis \
        --set image.tag=$VALKYRIE_VERSION
   ```

### Streams
The `discovery-streams` API is a websocket API for streaming public data to end user consumers.

1. Deploy
    ```bash
    helm upgrade --install discovery-streams scdp/discovery-streams \
     --set image.tag=$DISCOVERY_STREAMS_VERSION \
     --set kafka.brokers=streaming-service-kafka-bootstrap:9092
    ```

## Ingesting data
### Organizations

Every dataset has an owner -- or organization. The organization must exist before a dataset
can be ingested. `POST` organizations to `andi` `/api/v1/organization` endpoint.

You can see an example (test) organization [here](definitions/test/org.json). Remember to change all the values.

#### Example

```json
# org.json
{
    "description": "This is a test organization that should not be seen",
    "homepage": "https://github.com/smartcitiesdata",
    "id": "1b1cdc66-ad5e-45f4-baed-b874227838a6",
    "logoUrl": "https://placekitten.com/97/97",
    "orgName": "test_org",
    "orgTitle": "Test Organization"
}
```

```bash
kubectl port-forward svc/andi 8080:8080

curl -X POST \
     -H "Content-Type: application/json" \
     -d @org.json \
     http://localhost:8080/api/v1/organization
```

### Datasets

Datasets are created with a `PUT` to `andi` `/api/v1/dataset`.

You can see an example (test) dataset [here](definitions/test/batch.json). Remember to change values to be relevant to your dataset. Specifically, make sure the `technical.orgName` and `technical.orgId` match your organization definition. `andi` does not make this check yet.

#### Example

This is a remote dataset, which means it will not ingest any data into the system. But it will be discoverable via the API/UI.

```json
# remote.json
{
    "business": {
        "categories": null,
        "conformsToUri": null,
        "contactEmail": "me@me.com",
        "contactName": "me",
        "dataTitle": "Some test data",
        "describedByMimeType": null,
        "describedByUrl": null,
        "description": "A test dataset",
        "homepage": "https://github.com/smartcitiesdata",
        "issuedDate": "2016-08-10T20:20:58.000Z",
        "keywords": ["foo", "bar"],
        "language": null,
        "license": null,
        "modifiedDate": "2017-11-28T17:40:24.000Z",
        "orgTitle": "Test Organization",
        "parentDataset": null,
        "publishFrequency": null,
        "referenceUrls": null,
        "rights": null,
        "spatial": null,
        "temporal": null
    },
    "id": "c2ea7056-198e-4f78-b953-8176747f8463",
    "technical": {
        "cadence": "never",
        "dataName": "test_remote",
        "headers": {},
        "orgId": "1b1cdc66-ad5e-45f4-baed-b874227838a6",
        "orgName": "test_org",
        "partitioner": {
            "query": null,
            "type": null
        },
        "private": false,
        "sourceQueryParams": {},
        "schema": [],
        "sourceFormat": "csv",
        "sourceType": "remote",
        "sourceUrl": "https://smartcitiesdata.github.io/charts/index.yaml",
        "systemName": "test_org__test_remote",
        "transformations": [],
        "validations": []
    }
}
```

```bash
kubectl port-forward svc/andi 8080:8080

curl -X PUT \
     -H "Content-Type: application/json" \
     -d @remote.json \
     http://localhost:8888/api/v1/dataset
```
