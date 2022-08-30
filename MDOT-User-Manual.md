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

### Managing Access Groups
To manage Access Groups, add or remove Datasets or Users as needed. Selecting to Manage Datasets or Users will open a search.


Users can be searched for by username or their associated organization.


Datasets can be searched for by Dataset name, associated organization, or keywords.


### Managing Users
Users are managed through Auth0 and within ANDI. In ANDI, you can manage the Roles and Organizations associated with a user. In Auth0, you can manage a user’s access to the UrbanOS system.

| Field  | Usage |
| ------------- | ------------- |
| Email  | This is the email address associated with the user. This field should be populated by whichever authentication program is being used and should only need updated if the user requests it.  |
| Role  | Role controls what the user has permission to do in the ANDI Data Management Tool.  |
| Org  | The organization that the user is associated with. This affects what the user is allowed to view in UrbanOS.  |


## Datasets and Ingestions
### Managing Data in UrbanOS
In UrbanOS, data is stored in **Datasets**. Datasets describe the data contained in them including the purpose of the data, the maintainer, and a data dictionary.
Data is loaded into Datasets via Ingestions. A Dataset can be fed by one or many Ingestions. An Ingestion must be associated with one Dataset. Ingestions define how and when the data is being brought into the system and (optionally) modified before being stored in a Dataset. 

### Creating / Modifying Datasets
**Metadata Setup**
Proper metadata helps users and maintainers to understand what the Dataset contains, and the context for the data. 

| Field  | Usage |
| ------------- | ------------- |
| Dataset Title  | The human-readable name that will be associated The best email address to contact the Dataset maintainer. This will appear in DiscoveryUI to users viewing the Dataset.with the Dataset. This will appear in DiscoveryUI to users viewing the Dataset.  |
| Data Name  | The system-assigned unique name of the Dataset.  |
| Description  | Information about the Dataset that will be viewable by users. This will appear in DiscoveryUI to users viewing the Dataset.  |
| Maintainer Name  | The name of the person responsible for maintaining the Dataset. This will appear in DiscoveryUI to users viewing the Dataset.  |
| Maintainer Email  | The best email address to contact the Dataset maintainer. This will appear in DiscoveryUI to users viewing the Dataset.  |
| Dataset Owner  | A specific user within the UrbanOS system who owns the Dataset. By default, this is the Curator who created the Dataset.   |
| Release Date  | The initial date that the Dataset was released. This will appear in DiscoveryUI to users viewing the Dataset.  |
| Update Frequency  | Free-text description of how often the data will be refreshed. This will appear in DiscoveryUI to users viewing the Dataset.  |
| Keywords (optional)  | Keywords help users to find Datasets in search. Multiple keywords can be entered (comma separated).  |
| Risk  | An approximate level of risk that the organization feels that this Dataset represents. This field is only visible to Data Curators on this page.  |
| Benefit  | An approximate benefit to having this Dataset in UrbanOS. This field is only visible to Data Curators on this page.  |
| Last Updated  | The date that this data was last updated. This will appear in DiscoveryUI to users viewing the Dataset.  |
| Spatial Boundaries (optional)  | The geographical boundaries of a Dataset, e.g., a county or list of counties. This will appear in DiscoveryUI to users viewing the Dataset.  |
| Temporal Boundaries (optional)  | The timeframe in which a Dataset was gathered, or that the Dataset is relevant for. This will appear in DiscoveryUI to users viewing the Dataset.  |
| Source Type  | Describes how a Dataset is stored. Ingest – Dataset will be brought in and stored for querying. Streaming – Same as Ingest, additionally will be made available in real-time via the Websocket interface. Remote – A Dataset that is not stored in UrbanOS. Effectively a link to an external Dataset. This field can only be configured at Dataset creation and then becomes read-only. This will appear in DiscoveryUI to users viewing the Dataset.  |
| Organization Title  | The name of the Organization that owns this Dataset. This field can only be configured at Dataset creation and then becomes read-only. This will appear in DiscoveryUI to users viewing the Dataset.  |
| Level of Access  | **Public** – Anyone can view the Dataset. UrbanOS can be configured to restrict this to only authenticated users if desired. **Private** - Access is determined by the Organization and/or Access Groups associated with the Dataset.  |
| Language  | The language that the Dataset is in. Currently the only options are English or Spanish. This will appear in DiscoveryUI to users viewing the Dataset.  |
| Homepage URL (optional)  | A link to the homepage for the Dataset, if applicable. This will appear in DiscoveryUI to users viewing the Dataset.  |
| License  | A URL linking to the license that the Dataset falls under. This will appear in DiscoveryUI to users viewing the Dataset.  |



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