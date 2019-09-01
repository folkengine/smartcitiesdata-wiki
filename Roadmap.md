# What's Next
The platform is evolving quickly as we refine our understanding of the requirements outlined by the core stakeholders and get more feedback from potential users. Here's a list of the things we're working on in the next 6-8 months.

(This list will continue to change over time; check back here periodically to see where projected features have been implemented and new priorities have appeared to replace them.)

## Native Visualization
We're integrating Plot.ly directly into the platform to allow pulling data queries into a simple and intuitive visualization interface. Beta testing is currently underway and while the ultimate functionality will likely be very familiar to existing users of Plot.ly, how deeply the workflow is embedded and the seams of the transition between query and visualization will likely improve by the time of the official release.

Beyond that, we're working to allow export of datasets and query results to formats easily imported to other first class visualization tools, including Tableau, JupyterHub, and Keplar. Slightly further out, we're exploring the feasibility of integrating Keplar directly into the platform much as we are currently integrating Plot.ly.

## Automatic Scaling and Resource Management
The system is getting an event bus to coordinate communications between the various components whenever a new event is triggered anywhere in the system that might affect any other component. Once system events, such as the creation of a new dataset, upload of a new geospatial data file, or completion of an ingestion flow are generating event notification messages on the event bus, other systems will respond, for example, by notifying users on the home page of the new dataset, publishing a link to the new geospatial file to the dataset's record, or reclaiming system resources allocated to the completed ingestion process, respectively.

To realize the true value of Kubernetes and the cloud, we're also working to build more robust monitors and triggers around system load and add the necessary configurations to allow the platform to request additional Kubernetes workers or horizontally scale the number of service replicas providing a function like data normalization during peak periods and then scale down again once resource consumption flattens out. With this capability, cost-conscious organizations will save significant dollars running the platform and multi-tenancy will be a very real possibility.

## OAuth and User Profiles
What's better than building a great visualization around two datasets that are really hard to query? Saving that whole effort and coming back to it later, or better yet, sharing it with a fellow researcher or enthusiast! We're looking at a first pass of user profiles by leaning on the OAuth and OpenID Connect protocols to allow you to log into the platform via a Google or Github account and save a user profile for customizing your experience with the platform. If this proves useful, we'll expand the number of OAuth providers to make it easier to log in with the identity provider you prefer.

## Connected Vehicle Environment Data!
Columbus is rolling out its ambitious Connected Vehicle Environment project early next year and we're poised to collect the data from the Roadside Units and Onboard Units being installed in thousands of light poles and vehicles around the city in the coming months. Stay tuned for new connected vehicle data format parsers, performance optimizations, data feed push gateways and other features necessary to make this a reality. Not to mention the data produced by this fleet of connected vehicles!