# Reaper Troubleshooting

## Invalid URL/Headers/Params

### Error Indicators

##### User Flags
- Ingestion is submitted, but no data appears in DiscoveryUI

##### Example Log Messages
`16:22:01.060 [warn] Invalid Status Code returned while attempting to download file`

`16:18:01.180 [error] Reaper Task Failed with error: %Jason.DecodeError{data: "404: Not Found", position: 3, token: nil}`

The above examples use a 404 error code, but anything in the range of 400-500 is possible. 

### Resolution
This is likely an issue with the ingestion configuration. 

Direct the user to double check the URL, HTTP method, headers, and query parameters of their request in step one of the ingestion edit page in Andi.
![Screenshot 2023-06-12 at 4 34 48 PM](https://github.com/UrbanOS-Public/smartcitiesdata/assets/79878368/6c12d88b-1ccd-4299-aced-70adb44c3985)

Here are a few likely error codes and their error messages:

- 400 Bad request - Double check all headers and query parameters
- 401 Unauthorized - User provided invalid authorization credentials. Check the authentication headers in the request
- 403 Forbidden - User does not have permission to access the requested resource. Check the authentication headers in the request or have the user check their permissions
- 404 Not found - The requested resource does not exist. Double check the URL in the request
- 405 Method Not Allowed - The requested resource exists, but this is not a valid HTTP Method to access it. Double check the HTTP Method in the request
- 415 Unsupported Media Type - The requested Media Type is not supported. Double check the Content-Type header in the request
- 500 Internal Server Error - Unknown server error, double check the requested resource is working
As the user is troubleshooting the issue, they can utilize the test button provided in Andi at the bottom of the "Configure Ingestion Steps" section to test their configuration before submitting the ingestion.

***

## Vault is down

##### User Flags
- A previously working ingestion utilizing secrets is no longer working

##### Admin Flags
- One of the vault instances is not ready
![Screenshot 2023-06-15 at 2 33 55 PM](https://github.com/UrbanOS-Public/smartcitiesdata/assets/79878368/f556b7c4-1a90-4913-a289-2befee84257b)

##### Example Log Messages
`19:15:00.434 [error] Unable to retrieve ingestion credential; Vault is sealed`

`19:18:00.229 [error] Reaper Task Failed with error: %RuntimeError{message: "Unable to process secret step for ingestion 11ffdf77-ff8f-4936-87f7-893a085765fa."}`

### Resolution
This can be resolved by unsealing the vault.

The vault can be unsealed manually by shelling into the vault instance and using the `vault operator unseal` command with the appropriate vault keys.

It can also be unsealed using the `unseal_vault.sh` script in the openshift-deploy project.

More information can be found here: [Vault Troubleshooting](https://github.com/UrbanOS-Public/charts/blob/6e18549cafc2b92a15a311081ffd51cb2333fa70/docs/vault.md)
