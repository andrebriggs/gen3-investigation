# FENCE

https://github.com/uc-cdis/fence

```
git clone https://github.com/uc-cdis/fence.git
cd fence
docker build .
```

## Sequence
<img src="https://github.com/uc-cdis/fence/raw/master/docs/images/seq_diagrams/openid_connect_flow.png" width="800" />

## Dependencies
* AWS/GCP SDKs for pre-signed URLS
* PostgreSQL DB
* [Poetry]()

## Notes
* Does not have Azure support. Dockerfile [explicitly](https://github.com/uc-cdis/fence/blob/b02da4be971060c042f534a379830c4beabf2cae/Dockerfile#L43) using AWS CLI for "for storing files in s3 during usersync k8s job"
* There is a [thread](https://cdis.slack.com/archives/CDDPLU1NU/p1607360555231500?thread_ts=1607288724.227500&cid=CDDPLU1NU) on the Slack where Randy Olinger has instructions on using his forked versions of `fence` and `cdis-data-client` that support Azure Storage Blob
* "Does not fill in the data blob cloud URL when it generates the upload url. You'll have to populate that with your own script along with authz, md5 (or some other hash), and size or just ignore that entry, upload whatever you want to the bucket, generate a DIIRM manifest with the info you want in indexd, and use the SDK script to upload the manifest into indexd ... something like that should work" From [Slack](https://cdis.slack.com/archives/CDDPLU1NU/p1599762762017300?thread_ts=1599751053.013500&cid=CDDPLU1NU)

Understanding how Azure and OpenID work
* https://docs.microsoft.com/en-us/azure/active-directory/develop/v2-protocols-oidc

Generates presigned URLS on S3? (hence why it has references to S3 SDK)
* "Fence will see s3://mybucket/path/to/file.bam, it will check that the user has access to download, and then generate an S3 presigned URL for the file" via [Slack](https://cdis.slack.com/archives/CDDPLU1NU/p1596641152222700?thread_ts=1596308475.197400&cid=CDDPLU1NU)
* Code that chooses GCP or AWS strategy is [here](https://github.com/uc-cdis/fence/blob/d690f78fe356ccdba0e0e8e3f1e4bdce25c9b654/fence/blueprints/data/indexd.py#L44). We could add Azure support to get pre-signed urls (AKA [Shared Access Signature](https://docs.microsoft.com/en-us/rest/api/storageservices/delegate-access-with-shared-access-signature)) For Python see Azure docs [here](https://docs.microsoft.com/en-us/python/api/azure-storage-blob/azure.storage.blob.sharedaccesssignature.blobsharedaccesssignature?view=azure-python-previous)