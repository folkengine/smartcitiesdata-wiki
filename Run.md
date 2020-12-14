# To Run The Application from the root locally:
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
    * Open a new tab in your terminal and go inside the apps/discovery_api directory and run
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
  * When you're done you can kill the docker micro services inside the e2e directory by running:
    ```bash
    MIX_ENV=integration mix docker.kill
    ```

# To Run The Application from the root locally in minikube:
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