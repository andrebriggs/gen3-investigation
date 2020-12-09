# gen3-client

https://github.com/uc-cdis/cdis-data-client

> gen3-client is a command-line tool for downloading, uploading, and submitting data files to and from a Gen3 data commons.

## Dependencies

* The gen3-client needs to be configured with API credentials downloaded from the user’s data commons Profile (via Windmill data portal)
* `indexd` --> *"When files are successfully uploaded by the gen3-client, the software service indexd creates a record for that file in the file index database, which can be accessed at the /index endpoint."*
* `fence`
* If not `indexd` and `fence` it will use the [Gen3 Object Management API](https://github.com/uc-cdis/gen3-qa/blob/master/docs/testplans/obj-mgmt-api/obj-mgmt-api-test-plan.md) aka `Shepherd`

## Commands
* `gen3-client upload` (also has single and multiple variants)
* `gen3-client download-single`
* `gen3-client download-multiple`
* `gen3-client generate-tsv` --> *"In order to register data files in a Gen3 data commons, the filenames, md5sums, and file_size in bytes, must be submitted as metadata"*

## Notes
* No support for uploading to Azure Storage
