# ssjdispatcher

https://github.com/uc-cdis/ssjdispatcher

> The SQS S3 Job Dispatcher is designed for centralizing all gen3 jobs in the Gen3 stack. It monitors a SQS queue receiveing CRUD messages from S3 buckets and determine an action basing on the object url pattern.

> For example, an url with a pattern of s3://bucketname/user.yaml of the object uploaded to S3 will trigger an even in S3 to send a message to a configured SQS. The dispatcher service pulls the message from the queue and dispatches an job that pulls fence image and run usersync job with fence-create command. For the other url with the pattern of s3://data_upload_bucket/000ed0fb-d1f4-4b80-8d77-0d134bb4c0d6/TARGET-10-PAREBA-09A-01D_GAGTGG_L003.bam, the service will dispatch an job to compute hashes, and size and register to indexd with url.

## Dependencies
* AWS SQS

## Notes
* Watches the SQS and spins up an indexing job (https://github.com/uc-cdis/indexs3client) to update `indexd` with the file information (size, hash).
  * Listens to the bucket (via an SNS notifier), and computes the size and md5 of objects
* Appears to [create](https://github.com/uc-cdis/ssjdispatcher/blob/5c1893eb24034fa8674c29ef16fb7a20fa561db0/handlers/jobs.go#L248) Kubernetes jobs 
* Creates a [k8s job](https://github.com/uc-cdis/ssjdispatcher/blob/5c1893eb24034fa8674c29ef16fb7a20fa561db0/handlers/jobs.go#L179) to handle s3 object
* Needs AWS access keys for this
* There are terraform scripts to setup the SNS and SQS queues
* Quote from Gen3 maintainer: _the data upload flow is depressingly complex_ 👀

Accepts a [config](https://github.com/uc-cdis/ssjdispatcher/blob/c0c2a97e0033edbbbd91c7e25fff662df0722b73/handlers/utils_test.go#L11) file that has:
* AWS account secrets and region 
* Url to an AWS SQS instance 
* A job defintion (indexing) for the `indexs3client` Docker image and S3 url pattern
* A job definition (usersync) for `fence` and a url pattern on S3 for `user.yaml` 
* **Note**: Configuration only supports public images?  

In depth:
* Pulls messages [from SQS queue](https://github.com/aws/aws-sdk-go/blob/v1.36.13/service/sqs/api.go#L1536) 1 at a time in the consuming poll loop
* Handler processes [message](https://github.com/uc-cdis/ssjdispatcher/blob/aa5ec0668666a66545bad130c7f3b386782b83cb/handlers/handler.go#L196) and determines. Maps S3 url pattern to jobs.
* Using a [k8s rest client](https://github.com/kubernetes/client-go) creates a batch Kubernetes [Job](https://kubernetes.io/docs/concepts/workloads/controllers/job/) (since `ssjdispatcher` is expected to be running in k8s!) in a Gen3 namespace with configured service account/resoure requirements.
* Handler uses a mutex with to control access to a `MonitoredJobs` object that the job objects get added to
* Loop looks at job statuses, retries failed jobs and removes completed jobs. 
* Also creates a REST [endpoints](https://github.com/uc-cdis/ssjdispatcher/blob/dce4444289ac9ed329aaa870e6f1d90c04775094/handlers/handler_api.go#L10) to:
  * Get, post, and delete job configs 
  * Post to retry indexing jobs
  * Get indexing job status
  * Get and list jobs
  * Health endpoint


<!-- https://swimlanes.io/#fZCxTsQwEER7f8WUd5FSpUERCgU1EoIfSLDnLnsYO+d1Dvh7nIQmFGy12p15O9os2bOF6sWJTkO2IxMk4P1OjTHaoK67DnrVomnAG0MGw3XmTH0wZY77eu9u8Ry9V3xKHtG/0FJufKLqcObh2MPsb238PUBO+NgMCn6JZsxKjENwnkmRI6YUbVH8ZXVL7haPiUPm0uMS3w56NEu7Xlr3r0XtZl/oVVXISb+DrapF+5v6xGDZY4ruH6cEV8KF8865DrWxXsqnNkKpHw== -->