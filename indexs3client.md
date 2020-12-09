# indexs3client

https://github.com/uc-cdis/indexs3client

> Indexes s3 object The fuction does several things. It downloads the object from S3, computes size and hashes, and updates Indexd and potentially Metadata Service

## Dependencies
* AWS S3

## Commands
* `gen3-client upload` (also has single and multiple variants)
* `gen3-client download-single`
* `gen3-client download-multiple`
* `gen3-client generate-tsv` --> *"In order to register data files in a Gen3 data commons, the filenames, md5sums, and file_size in bytes, must be submitted as metadata"*

## Notes
* This is essentially an event based *job handler*. Perhaps Azure functions can be used