# Triage Flows
Triage flows are intended to describe the resulting behavior of an issue and trace back to which technical component should be inspected. Most triage flows will result in either directing you to a specific service's troubleshooting section, which can be found on this wiki. Any issue that is unable to be resolved via these triage flows should be reported as github issues so we can diagnose and document the resolution.


## No Column In Preview Table

When viewing Discovery UI, if a preview table of a dataset does not have column headers, that indicates the table was not successfully created within the persisted data layer of the platform. Upon publishing a dataset from ANDI, a dataset_update event is put onto the event stream. This is consumed by forklift, which creates a table through Trino/Hive/Minio.

![NoColumnInPreviewTable](https://github.com/UrbanOS-Public/smartcitiesdata/assets/79863335/611320fd-f3a0-44ad-a47f-45366c35fcb7)
