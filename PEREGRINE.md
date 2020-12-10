# PEREGRINE

https://github.com/uc-cdis/peregrine
https://petstore.swagger.io/?url=https://raw.githubusercontent.com/uc-cdis/peregrine/master/openapis/swagger.yaml

> Query interface to get insights into data in Gen3 Commons

## Dependencies

* ~~[storage-client](https://github.com/uc-cdis/storage-client)~~ (Referenced but no usage)
* ~~`indexclient`~~ (Referenced but no usage)
* Python
* Postgres driver
* Gen3 `DataDictionary` 
* `arborist` for auth

## Note 
* Flask based webapp
* Uses a "blueprint" from `sheepdog`
Using 
* Seems to be a microservice to microservice communication pattern
* [Loads](https://github.com/uc-cdis/peregrine/blob/5eff625b4fd6d3e2c4a64de865e26de42bacdbe9/peregrine/api.py#L215) configuration that [references](https://github.com/uc-cdis/peregrine/blob/ab9a8387ef8eba1bd77b0ce1c570f02d59e14260/peregrine/dev_settings.py#L27) AWS S3 buckets and postgres [instance](https://github.com/uc-cdis/peregrine/blob/ab9a8387ef8eba1bd77b0ce1c570f02d59e14260/peregrine/dev_settings.py#L55)
* Has an OAUTH [connection](https://github.com/uc-cdis/peregrine/blob/ab9a8387ef8eba1bd77b0ce1c570f02d59e14260/peregrine/dev_settings.py#L77) in config but cannot find the usage. Dead code reference?
* Cannot find where the method [get_s3_conn](https://github.com/uc-cdis/peregrine/blob/e74f55f35ddd4e8ec978f9245f7f0d3f241d4a3a/peregrine/utils/pyutils.py#L19) is actually used though