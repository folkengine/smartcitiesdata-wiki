* Data Curator Interface (ANDI)
* Data Discovery (DiscoveryUI)

# ANDI
ANDI is the visual interface for curating Datasets in UrbanOS. Users who are configured to use ANDI in Auth0 will be able to use this interface.

## User Data Access
### Data access controls in UrbanOS
In UrbanOS, data access is controlled via **Organizations** and **Access Groups**. 

Datasets can be made public, meaning anyone can view them.

For Datasets made private, they will be available only to users within the same Organization as what is assigned to the Dataset, or to users in their Access Groups.

To restrict Datasets more granularly, associate the Dataset with an Access Group. Datasets can be associated with any number of Access Groups. Access Groups can be assigned to any number of Datasets. Every user in an Access Group can view private Datasets associated with that Access Group.

**Example**: To restrict a Private Dataset called “Private” to a single user “John Doe”, you could create an Access Group called “SingleUser”. Associate the Access Group to the “Private” Dataset and to the user “John Doe”. At that point, the Dataset would only be accessible to that single user.

### Managing Organizations
| Field | Usage |
| ------------- | ------------- |
| Organization Title  | The name of the organization that will be shown to users. This will appear in DiscoveryUI to users viewing Datasets that belong to the Organization.  |
| Organization Name  | Behind-the-scenes name of the Organization.  |
| Description  | A short description of the organization. This will appear in DiscoveryUI to users viewing Datasets that belong to the Organization.  |
| Logo URL (optional)  | A direct link to the logo for the organization. This will appear in DiscoveryUI to users viewing Datasets that belong to the Organization.  |
| Homepage (optional)  | A URL for the organization’s homepage. This will appear in DiscoveryUI to users viewing Datasets that belong to the Organization.  |
| Data JSON URL (optional)  | If provided, ANDI will use this URL to harvest remote Datasets from another system. It will not ingest data from them but will make them available via DiscoveryUI Search. More information about the data.json standard can be found here: [https://resources.data.gov/resources/dcat-us/](https://resources.data.gov/resources/dcat-us/)  |
| Remote Datasets Attached to This Organization  | If a Data JSON URL is provided and the harvest process is successful, this area will show a list of Datasets from the specified location.  |


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