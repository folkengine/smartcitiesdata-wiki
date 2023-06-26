# No Column In Preview Table

Description:
When viewing Discovery UI, if a preview table of a dataset does not have column headers, that indicates the table was not successfully created within the persisted data layer of the platform. Upon publishing a dataset from ANDI, a dataset_update event is put onto the event stream. This is consumed by forklift, which creates a table through Trino/Hive/Minio.

![NoColumnInPreviewTable](https://github.com/UrbanOS-Public/smartcitiesdata/assets/79863335/611320fd-f3a0-44ad-a47f-45366c35fcb7)
