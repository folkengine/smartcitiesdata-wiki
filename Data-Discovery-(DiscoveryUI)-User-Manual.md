**Note** This manual includes planned functionality changes that are currently under development.

  - [Exploring Datasets](#exploring-datasets)
    - [Search](https://github.com/UrbanOS-Public/smartcitiesdata/wiki/MDOT-User-Manual#search)
    - [Dataset Details](https://github.com/UrbanOS-Public/smartcitiesdata/wiki/MDOT-User-Manual#dataset-details)
    - [Write SQL](https://github.com/UrbanOS-Public/smartcitiesdata/wiki/MDOT-User-Manual#write-sql)
    - [Visualize](https://github.com/UrbanOS-Public/smartcitiesdata/wiki/MDOT-User-Manual#visualize)
    - [Workspaces](https://github.com/UrbanOS-Public/smartcitiesdata/wiki/MDOT-User-Manual#workspaces)
    - [API Key](https://github.com/UrbanOS-Public/smartcitiesdata/wiki/MDOT-User-Manual#api-key)

# DiscoveryUI
DiscoveryUI is the public-facing interface for UrbanOS. It displays Datasets that the user is permitted to view based on configurations in ANDI. This can include sharing as widely as allowing unauthenticated users to view public Datasets if desired.

## Exploring Datasets
### Search
Datasets that the user can access are displayed. The user can search, sort, and filter to find specific Datasets. Filters shown are based on Dataset Keywords with the most common keywords at the top and the Organizations that have Datasets available to the user. The thumbnails shown for each Dataset are based on the Dataset’s Organization configuration in ANDI.


### Dataset Details
When a user selects a Dataset, they are brought to the Dataset Details page. This page shows a sample of the data in the Dataset, metadata describing the Dataset, and information on how to access the data via API. If a Dataset contains mapping data, a Map Preview section will be included on the page.

### Write SQL
This page allows users to query the Dataset directly from the web. From there, users can download results as a CSV or JSON file. Queries will take more or less time depending on the number of results, so limiting results to less than 20,000 is recommended to prevent page timeouts.

### Visualize
The visualization tool in DiscoveryUI is powered by Plo.ly. Users can create data visualizations using the Dataset and save them to their Workspace. Plot.ly help can be found here: [https://plotly.com/chart-studio-help/tutorials/#basic](https://plotly.com/chart-studio-help/tutorials/#basic)

### Workspaces
Workspaces allow users to save visualizations created on the Visualize page for Datasets. By saving a workspace, users will get a unique URL that goes directly to their visualization.

### API Key
**Coming Soon**