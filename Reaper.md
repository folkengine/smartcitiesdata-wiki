# Reaper Troubleshooting

## Invalid URL/Headers/Params

##### Possible User Experiences
- Ingestion is submitted, but no data appears in DiscoveryUI

##### Example Log Messages
`16:22:01.060 [warn] Invalid Status Code returned while attempting to download file`

`16:18:01.180 [error] Reaper Task Failed with error: %Jason.DecodeError{data: "404: Not Found", position: 3, token: nil}`

##### Example Deadletter Message
```
{"app":"reaper","dataset_ids":["7e2060ff-663a-47ce-967b-33b14955606d"],"error":"%Jason.DecodeError{data: \"404: Not Found\", position: 3, token: nil}","exit_code":null,"ingestion_id":"5eaece20-69f6-4ba8-8919-8fd83230a4da","original_message":"404: Not Found","reason":null,"stacktrace":"    (elixir 1.10.4) lib/process.ex:765: Process.info/2\n    (dead_letter 1.1.4) lib/dead_letter/server.ex:73: DeadLetter.Server.format_message/5\n    (dead_letter 1.1.4) lib/dead_letter/server.ex:49: DeadLetter.Server.process/5\n    (reaper 2.0.35) lib/reaper/decoder.ex:40: Reaper.Decoder.decode/2\n    (reaper 2.0.35) lib/reaper/data_extract/processor.ex:74: Reaper.DataExtract.Processor.create_producer_stage/1\n    (reaper 2.0.35) lib/reaper/data_extract/processor.ex:41: Reaper.DataExtract.Processor.process/2\n    (reaper 2.0.35) lib/reaper/run_task.ex:21: Reaper.RunTask.handle_continue/2\n    (stdlib 3.14) gen_server.erl:689: :gen_server.try_dispatch/4\n","timestamp":"2023-06-12T20:02:01.163101Z"}
```

The above examples use a 404 error code, but anything in the range of 400-500 is possible. 

### Resolution
This is likely an issue with the ingestion configuration. 

Direct the user to double check the URL, HTTP method, headers, and query parameters of their request in step one of the ingestion edit page in Andi.
![Screenshot 2023-06-12 at 4 34 48 PM](https://github.com/UrbanOS-Public/smartcitiesdata/assets/79878368/6c12d88b-1ccd-4299-aced-70adb44c3985)

Here are a few likely error codes and their causes:
- 400 Bad request - Fallback error, likely caused by invalid query parameters or headers
- 401 Unauthorized - Incorrect authorization header
- 403 Forbidden - Provided authorization header does not have the required permissions
- 404 Not found - Bad URL
- 405 Method Not Allowed - Wrong HTTP method
- 415 Unsupported Media Type - Check the Content-Type header

As the user is troubleshooting the issue, they can utilize the test button provided in Andi at the bottom of the "Configure Ingestion Steps" section to test their configuration before submitting the ingestion.

***

## Vault is down

##### Possible User Experiences
- A previously working ingestion utilizing secrets is no longer working

### Resolution
This is likely due to vault being down.

***TODO***