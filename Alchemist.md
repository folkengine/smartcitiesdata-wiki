# Alchemist Troubleshooting

## Missing Field

### Error Indicators
##### User Flags
- A field with transformed data is not showing up in DiscoveryUI

##### Example Log Messages
`Transformation error; INGESTION_ID: 953f1306-9344-4acb-96c8-bac6aa0b416e; "PAYLOAD: %{"lat" => 39.9932218, "long" => -82.9822289, "status" => "occupied", "vendor" => "ben's parking meters inc"}; Transformation Error: "A given value myMissingField cannot be parsed to integer or float`

### Resolution
- This is likely due to a typo or a missing field in the dataset. The user will need to correct the mistake or create a new dataset/ingestion with the proper data fields.

## Conversion error

### Error Indicators
##### User Flags
- A field in DiscoveryUI either contains untransformed data or has no data when it is expected

##### Example Log Messages
`15:21:00.671 [error] Transformation error; INGESTION_ID: 953f1306-9344-4acb-96c8-bac6aa0b416e; "PAYLOAD: %{"lat" => 39.9932218, "long" => -82.9822289, "status" => "occupied", "vendor" => "ben's parking meters inc"}; "Addition Transformation Error: "A given value status cannot be parsed to integer or float"`

### Resolution
- This is likely due to selecting the wrong field, or attempting to convert a field to an invalid type. For example, a user might be trying to convert the string "MyTestField" to an integer. The string would need to be something capable of being transformed into an integer first, such as the string "12345".

## Regex error

### Error Indicators
##### User Flags
- An expected regex transformation is not occurring as it should be

##### Example Log Messages
`Transformation error; INGESTION_ID: 953f1306-9344-4acb-96c8-bac6aa0b416e; "PAYLOAD: %{"lat" => 39.9932218, "long" => -82.9822289, "status" => "occupied", "vendor" => "ben's parking meters inc"}; Regex Transformation Error: "Failure to parse regex: ~r/invalidRegex/"`

### Resolution
- This is likely due to the user-provided regex being invalid. The user will need to re-enter their regex in a valid format.