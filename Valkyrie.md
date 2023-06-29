# Valkyrie Troubleshooting

## Validation Failure

### Error Indicators
##### User Flags
- What would the user experience when this occurs? 

When a user creates a new Ingestion, some of the data in Discovery UI will either not exist entirely or partially populate. 


##### Example Log Messages
- What does a log message look like in the pod for this error?

`18:46:02.972 [error] ingestion_id: ec3a487c-bcad-438b-99af-f03cf5f54a0c; payload: %{"MyNumber1" => "Invalid String", "MyNumber2" => 2, "MyString1" => "Hello", "MyString2" => "World"}; Failed Schema Validation: %{"MyNumber1" => :invalid_integer}`


### Resolution
- What can be done to resolve this? 

1.) The user either needs to change the data type in the Data Dictionary/Schema of the target dataset
2.) Verify the incoming data is consistent
3.) Use Transformations to convert the incoming data into the type required