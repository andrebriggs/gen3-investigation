# ssjdispatcher

https://github.com/uc-cdis/ssjdispatcher


> The SQS S3 Job Dispatcher is designed for centralizing all gen3 jobs in the Gen3 stack. It monitors a SQS queue receiveing CRUD messages from S3 buckets and determine an action basing on the object url pattern.

> For example, an url with a pattern of s3://bucketname/user.yaml of the object uploaded to S3 will trigger an even in S3 to send a message to a configured SQS. The dispatcher service pulls the message from the queue and dispatches an job that pulls fence image and run usersync job with fence-create command. For the other url with the pattern of s3://data_upload_bucket/000ed0fb-d1f4-4b80-8d77-0d134bb4c0d6/TARGET-10-PAREBA-09A-01D_GAGTGG_L003.bam, the service will dispatch an job to compute hashes, and size and register to indexd with url.

## Dependencies
* AWS SQS

## Notes
* Appears to [create](https://github.com/uc-cdis/ssjdispatcher/blob/5c1893eb24034fa8674c29ef16fb7a20fa561db0/handlers/jobs.go#L248) Kubernetes jobs 
* Creates a [k8s job](https://github.com/uc-cdis/ssjdispatcher/blob/5c1893eb24034fa8674c29ef16fb7a20fa561db0/handlers/jobs.go#L179) to handle s3 object
* Needs AWS access keys for this