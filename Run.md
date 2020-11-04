# To Run The Application from the root locally in minikube:
  * Start minikube:
    ```bash
    `minikube start --kubernetes-version v1.15.0 --memory 6144 --cpus 4`
    ```
  * Initialize helm:
    ```bash
    `helm init`
    ```
  * Add datastillery charts repository:
    ```bash
    `helm repo add scdp https://datastillery.github.io/charts`
    ```
  * Add strimzi charts repository:
    ```bash
    `helm repo add strimzi https://strimzi.io/charts`
    ```
  * Add bitnami charts repository:
    ```bash
    `helm repo add bitnami https://charts.bitnami.com/bitnami`
    ```
  * Add hashicorp charts repository:
    ```bash
    `helm repo add hashicorp https://helm.releases.hashicorp.com`
    ```
  * To search charts from all the repositories added locally:
    ```bash
    `helm search`
    ```
  * To search charts from specific repository:
    ```bash
    `helm search <REPOSITORY_NAME>`
    ```
  * To install chart and deploy application:
    ```bash
    `helm upgrade --install <POD_NAME> <REPOSITORY_NAME>/<CHART_NAME>`
    ```
    or
    ```bash
    `helm upgrade --install <POD_NAME> <REPOSITORY_NAME>/<CHART_NAME> --namespace=<NAMESPACE>`
    ```
    if you want to install it in a specific namespace. Uses default namespace if not specified.
  * To verify pods has been created succesfully:
    ```bash
    `kubectl get pods -n <NAME_SPACE>`
    ```
  * To describe the pod:
    ```bash
    `kubectl describe pod <POD_NAME> -n <NAME_SPACE>`
    ```
  * To execute the pod:
    ```bash
    `kubectl exec -itn pod <NAME_SPACE> <POD_NAME> --bash `
    ```
  * To execute app inside the pod:
    ```bash
    `/app/bin/<APP_NAME> remote_console`
    ```
  * To execute module inside the app:
    ```bash
    `<MODULE_NAME>.<FUNCTION_NAME>`
    ```
  * To exit the app:
    ```bash
    `CTRL+C`
    ```
  * To exit the pod:
    ```bash
    `exit`
    ```
  * To delete minikube:
    ```bash
    `minikube delete`
    ```