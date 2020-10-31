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
  * To verify pods has been created succesfully:
    ```bash
    `kubectl get pods -n <NAME_SPACE>`
    ```
  * To delete minikube:
    ```bash
    `minikube delete`
    ```