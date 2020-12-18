# PEREGRINE

https://github.com/uc-cdis/peregrine
https://petstore.swagger.io/?url=https://raw.githubusercontent.com/uc-cdis/peregrine/master/openapis/swagger.yaml

> Query interface to get insights into data in Gen3 Commons
## Sequence
<!-- ![image](https://static.swimlanes.io/96ba3c900b4ce3960d62b04484d0964f.png) -->
<img src="https://static.swimlanes.io/96ba3c900b4ce3960d62b04484d0964f.png" width="800" />

[Source](https://swimlanes.io/#jZJBTwIxEIXv+yt6hESW+8aYYLhgjKzBxGMztMNuobTLTFfdf28XlYVgjed+783rmwkmWCxEiYQVGYdiVi4KMa0Imvpgsyx7NU7vjbVicjdQUbBcvQzYIL+kGIMkBC1BKWSWDfktqsCjcVJS/VMyo7UnwyGGhTbU0z00jXHVGXQ7mZxR1+/Xc32D7megNPo65ur5UczvC5E7r5FHJfnYwH6cg7UXbD/6BPeu5ZfpYs7JAApUjVoYJzYWeJdX+W81JOUNEGNs7tAiB7ll7/4oGT9QtQFlhKlL/vLdhDqukNl4VySYb6fR0elGvAEZWFvkZBuE3NqQawiQjNenN5tOxkLUTiKRp34Xp1M8Gg74w2r5JErorAf9CQ==)


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