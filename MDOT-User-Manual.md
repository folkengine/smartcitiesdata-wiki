**Note** This manual includes planned functionality changes that are currently under development.

* [Data Curator Interface (ANDI)](https://github.com/UrbanOS-Public/smartcitiesdata/wiki/MDOT-User-Manual#andi)
  - [User Data Access](https://github.com/UrbanOS-Public/smartcitiesdata/wiki/MDOT-User-Manual#user-data-access)
    - [Data access controls in UrbanOS](https://github.com/UrbanOS-Public/smartcitiesdata/wiki/MDOT-User-Manual#data-access-controls-in-urbanos)
    - [Managing Organizations](https://github.com/UrbanOS-Public/smartcitiesdata/wiki/MDOT-User-Manual#managing-organizations)
    - [Managing Access Groups](https://github.com/UrbanOS-Public/smartcitiesdata/wiki/MDOT-User-Manual#managing-access-groups)
    - [Managing Users](https://github.com/UrbanOS-Public/smartcitiesdata/wiki/MDOT-User-Manual#managing-users)
  - [Datasets and Ingestions](https://github.com/UrbanOS-Public/smartcitiesdata/wiki/MDOT-User-Manual#datasets-and-ingestions)
    - [Managing Data in UrbanOS](https://github.com/UrbanOS-Public/smartcitiesdata/wiki/MDOT-User-Manual#managing-data-in-urbanos)
    - [Creating / Modifying Datasets](https://github.com/UrbanOS-Public/smartcitiesdata/wiki/MDOT-User-Manual#creating--modifying-datasets)
    - [Creating / Modifying Ingestions](https://github.com/UrbanOS-Public/smartcitiesdata/wiki/MDOT-User-Manual#creating--modifying-ingestions)
    - [Ingestion Schema](https://github.com/UrbanOS-Public/smartcitiesdata/wiki/MDOT-User-Manual#ingestion-schema)
    - [Transformations](https://github.com/UrbanOS-Public/smartcitiesdata/wiki/MDOT-User-Manual#transformations)
    - [Finalizing Ingestion](https://github.com/UrbanOS-Public/smartcitiesdata/wiki/MDOT-User-Manual#finalizing-ingestion)
  - [Datasets and Ingestions](https://github.com/UrbanOS-Public/smartcitiesdata/wiki/MDOT-User-Manual#datasets-and-ingestions)
* [Data Discovery (DiscoveryUI)](https://github.com/UrbanOS-Public/smartcitiesdata/wiki/MDOT-User-Manual#discoveryui)
  - [Exploring Datasets](https://github.com/UrbanOS-Public/smartcitiesdata/wiki/MDOT-User-Manual#exploring-datasets)
    - [Search](https://github.com/UrbanOS-Public/smartcitiesdata/wiki/MDOT-User-Manual#search)
    - [Dataset Details](https://github.com/UrbanOS-Public/smartcitiesdata/wiki/MDOT-User-Manual#dataset-details)
    - [Write SQL](https://github.com/UrbanOS-Public/smartcitiesdata/wiki/MDOT-User-Manual#write-sql)
    - [Visualize](https://github.com/UrbanOS-Public/smartcitiesdata/wiki/MDOT-User-Manual#visualize)
    - [Workspaces](https://github.com/UrbanOS-Public/smartcitiesdata/wiki/MDOT-User-Manual#workspaces)
    - [API Key](https://github.com/UrbanOS-Public/smartcitiesdata/wiki/MDOT-User-Manual#api-key)

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

**Data Dictionary**

The Data Dictionary details each field in the Dataset. This enables UrbanOS to properly store the Dataset. The Dictionary describes the table that the data will be stored in.

A user can upload a data sample and UrbanOS will attempt to pull in all relevant field names and types. This is not always a perfect process, so the Data Curator should double check that everything came in properly. 

Alternatively, the user can manually add and configure each field.

| Field  | Usage |
| ------------- | ------------- |
| Name  | The name of the selected field. This will appear in DiscoveryUI to users viewing the Dataset.  |
| Type  | The data type associated with the field, such as string, integer, or Boolean. This will appear in DiscoveryUI to users viewing the Dataset.  |
| Description  | A human-readable description of what the field is. This metadata will help users to correctly interpret the data. This will appear in DiscoveryUI to users viewing the Dataset.  |
| PII  | A metadata field to represent that a field can or does contain Personally Identifiable Information.   |
| De-Identified  | A metadata field to represent that a field has been deidentified of PII.  |
| Potentially Biased  | Represents that data in this field is potentially biased based on sex, gender, class, race, etc.  |
| Rationale  | Free text field to describe why the data could potentially be biased.  |

### Creating / Modifying Ingestions
**Ingestion Setup**

| Field  | Usage |
| ------------- | ------------- |
| Ingestion Name  | A human-readable name of the Ingestion.   |
| Dataset Name  | The Dataset that the Ingestion will contribute to.  |
| Source Format  | The format of the data that will be brought in with this Ingestion.  |

**Configure Ingest Steps**
These steps specify how data will be brought into UrbanOS. After filling in the necessary fields, use the “Test” button to ensure the data can be accessed successfully.

There are five types of Steps that can be configured for Ingestions. Steps are executed in the order they are shown in the UI. The last step of an Ingestion must be either HTTP or S3.
* **HTTP** – Must be the last step. Represents the information needed to get data and put it into the system.
* **Auth** – Represents making an authentication call. If you need to call to an API to get a token, this step enables it.
* **Date** – Allows you to fill an arbitrary field with a date in the format of your choice. Optionally includes an offset +/- time (for example, to add 5 minutes to the field). 
* **S3** – If this step exists, it needs to be in place of HTTP. It allows you to retrieve data from a secure S3 bucket using the S3 API.
* **Secret** – Allows you to store and retrieve a secret value from the secret store of UrbanOS (Vault). At Ingestion time, the secret is retrieved to perform the Ingestion.

**HTTP Ingestion Step Type**
| Field  | Usage |
| ------------- | ------------- |
| Method  | GET or POST (GET represents a standard web request, while POST allows for sending a payload to the data provider if needed)  |
| URL  | The source location of the data being ingested.  |
| Query Parameters  | Request parameters to be sent to the data provider. For example in the URL https://example.com/over/there?name=ferret there is a query parameter with a Key of “name” and a Value of “ferret”  |
| Headers  | Request headers to be sent to the data provider. Often used to send secret values such as API tokens, they can also be used to define other things about the request such as: “Accept-Encoding: gzip, deflate, br”. In this case, the key is “Accept-Encoding”, and the value is “gzip, deflate, br”  |

**Auth Ingestion Step Type**
| Field  | Usage |
| ------------- | ------------- |
| Destination  | This freeform field represents where the value will be stored for later use in other extract steps. Stored values can be referenced using this format: {{thing}} where “thing” is the value of the Destination field.  |
| URL  | The URL for the authentication server. This is where authentication HTTP requests will be sent.  |
| Headers  | Request headers to be sent to the authentication provider. Often used to send secret values such as API tokens, they can also be used to define other things about the request such as: “Accept-Encoding: gzip, deflate, br”. In this case, the Key is “Accept-Encoding” and the Value is “gzip, deflate, br”  |
| Body  | An HTTP request body. This will usually be in JSON format if needed. Often used to send data to an authentication server such as a username/password set.  |
| Response Location  | This identifies where within the returned data from the authentication provider the value to be stored in Destination will be found. This will often be a token of some kind.  |
| Cache Duration  | The time in minutes that credentials will be stored in UrbanOS.  |

**Date Ingestion Step Type**
| Field  | Usage |
| ------------- | ------------- |
| Destination  | This freeform field represents where the date value will be stored for later use in other extract steps. Stored values can be referenced using this format: {{thing}} where “thing” is the value of the Destination field.  |
| Time Offset Units  | The units for any time offset, with options from seconds to years.  |
| Time Offset Value  | A numeric input aligned to the units selected of offset.  |
| Format  | The format of the date being specified in Timex format.  |
| Output  | This field shows a preview of the value produced by this extract step to confirm the format and offset is working as expected. NOTE: The date value is created each time the Ingestion runs.  |

**S3 Ingestion Step Type**
| Field  | Usage |
| ------------- | ------------- |
| URL  | The location of the S3 bucket for the data being ingested.  |
| Headers  | Request headers to be sent to the S3 server. Often used to send secret values such as API tokens, they can also be used to define other things about the request such as: “Accept-Encoding: gzip, deflate, br”. In this case, the Key is “Accept-Encoding” and the Value is “gzip, deflate, br”  |

**Secret Ingestion Step Type**
| Field  | Usage |
| ------------- | ------------- |
| Destination  | This freeform field represents where the secret value will be stored for later use in other extract steps. Stored values can be referenced using this format: {{thing}} where “thing” is the value of the Destination field.  |
| Value  | Values entered here will be stored securely in the platform’s secret store and used in ingestions. They cannot be retrieved from this interface, only overwritten.  |

### Ingestion Schema
Like the Data Dictionary in Datasets, this section describes the fields that will come in as part of this Ingestion. The schema represents the data from the data source as it comes into the system. The names and data types in the Ingestion Schema as modified by any transformation should align with where the data is going in the target Dataset’s schema. If there are no transformations, the schema of the Ingestion and the Dataset it is feeding should be identical.

A user can upload a data sample and UrbanOS will attempt to pull in all relevant field names and types. This is not always a perfect process, so the Data Curator should double check that everything came in properly. 

Alternatively, the user can manually add and configure each field.

### Transformations
Transformations take a field that is coming into the Ingestion and modify it in some way. This allows UrbanOS to dynamically alter data to a desired state as it arrives. 

Transformations are processed in the order they are specified in the UI. 

| Transformation Type  | Description |
| ------------- | ------------- |
| Drop Column  | Removes a specified field from the data.  |
| Change Field Type  | Attempts to convert a field from one type to another. E.g. an int to a string. The result can be stored in the same field (effectively replacing the original value) or in a different field.  |
| Extract Based on Regex  | Takes a field from the source data and applies a Regex to it. The result can be stored in the same field (effectively replacing the original value) or in a different field.  |
| Set Value  | Assigns a specified value to the field. This is useful when doing a conditional transformation, e.g. setting a “speeding” field to True when a vehicle is moving over a certain speed.  |
| Convert DateTime Format  | Changes a DateTime field to a new format. Requires the user to specify the current format of the DateTime and the desired format using TimeEx markup.  |
| Basic Arithmetic  | Add, subtract, multiple, or divide values in a field. The result can be stored in the same field (effectively replacing the original value) or in a different field.  |
| Concatenate Fields  | Combine the values from two fields with an optional connection text. The result can be stored in the same field (effectively replacing the original value) or in a different field.  |

### Finalizing Ingestion
The last step is to determine when the data Ingestion will happen. 
| Transformation Type  | Description |
| ------------- | ------------- |
| Immediately | This will kick off the Ingestion immediately. [Note] Currently this means the Ingestion will happen exactly once and never again. If there is an error on the Ingestion and you chose “Immediately”, you will have to recreate the Ingestion. The workaround until that is fixed is to select “Repeat” and specify an exact time and date for it to run.   |
| Repeat  | Allows you to specify a cron job for when the Ingestion will run. Times are in UTC.  |


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