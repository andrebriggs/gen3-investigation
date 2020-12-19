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

* PostgreSQL DB
* [Poetry]()

## Notes
* Does not have Azure support. Dockerfile [explicitly](https://github.com/uc-cdis/fence/blob/b02da4be971060c042f534a379830c4beabf2cae/Dockerfile#L43) using AWS CLI for "for storing files in s3 during usersync k8s job"
* There is a [thread](https://cdis.slack.com/archives/CDDPLU1NU/p1607360555231500?thread_ts=1607288724.227500&cid=CDDPLU1NU) on the Slack where Randy Olinger has instructions on using his forked versions of `fence` and `cdis-data-client` that support Azure Storage Blob

Understanding how Azure and OpenID work
* https://docs.microsoft.com/en-us/azure/active-directory/develop/v2-protocols-oidc