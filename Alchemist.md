# Alchemist Troubleshooting

## Missing Field

### Error Indicators
##### User Flags
- A field with transformed data is not showing up in DiscoveryUI

##### Example Log Messages
`Missing field: 14:45:01.490 [error] Ingestion: bc86ab32-9d4c-4639-a49d-f1b3670a05c4; Failed to transform payload: "A given value myMissingField cannot be parsed to integer or float, nor is it in the following payload: %{\"lat\" => 40.0613491, \"long\" => -82.9887223, \"status\" => \"occupied\", \"vendor\" => \"ben's parking meters inc\"}`

### Resolution
- This is likely due to a typo or a missing field in the dataset. The user will need to correct the mistake or create a new dataset/ingestion with the proper data fields.

## Conversion error

### Error Indicators
##### User Flags
- A field in DiscoveryUI either contains untransformed data or has no data when it is expected

##### Example Log Messages
`06:01.124 [error] Ingestion: bc86ab32-9d4c-4639-a49d-f1b3670a05c4; Failed to transform payload: "Cannot parse field status with value occupied into float"`

### Resolution
- This is likely due to selecting the wrong field, or attempting to convert a field to an invalid type. For example, a user might be trying to convert the string "MyTestField" to an integer. The string would need to be something capable of being transformed into an integer first, such as the string "12345".

## Regex error

### Error Indicators
##### User Flags
- An expected regex transformation is not occurring as it should be

##### Example Log Messages
`16:03:01.383 [error] Ingestion: bc86ab32-9d4c-4639-a49d-f1b3670a05c4; Failed to transform payload: "Failure to parse regex: ~r/invalidRegex/"`

### Resolution
- This is likely due to the user-provided regex being invalid. The user will need to re-enter their regex in a valid format.