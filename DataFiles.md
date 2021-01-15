# How to get data to appear on Files tab in Gen3 UI (docker compose setup)

### Prerequisites
1. Make sure that Docker disk image size is adequate. I had to increase my disk image from 80 GB to 128 GB. The reason for this is that the Elasticsearch Docker image was giving warning and crashing routinely with a log message `rerouting shards: [high disk watermark exceeded on one or more nodes]`. If you see the above in your Docker logs for Elasticsearch you might want to increase the disk image. You should shut down the Docker compose before attempting.

2. Uncomment the following in `./nginx.conf` if commented out from [previous](./README.md) steps.
```
    location /guppy/ {
            proxy_pass http://guppy-service/;
    }
```
3. Assumes you have set up the [1st pass](./README.md)  of the docker compose setup

## Setup
In instructions on [compose-services]() repo tell you to run the `basd./guppy_setup.sh` file but I found more success in manually running the instructions inside of the file. 

**Note**: The files `guppy_config.json`, `etlMapping.yaml` and `gitops.json` in your `./Secrets` directly can remain unchanged for these purposes. 

1. Verify that `guppy-service` is not running via `docker container stop guppy-service`
2. Verify that your Elasticsearch instance is running locally. Hit `http://localhost:9200`. Execute `docker-compose up -d esproxy-service` if Elastic isn't running. 
2. See what indices your Elasticsearch instance has by running `http://localhost:9200/_cat/indices?v`. The query string parameter `v` means verbose
3. If you don't see `etl_0`, `file_0`, `file-array-config_0`, or `etl-array-config_0` as indicies you can skip to step 5.
4. OPTIONAL: Delete the indicies in step 3 by manually running the commands in `guppy_setup.sh` in your console (see below). Alternatively, you can visit Kibana(`kibana-service`) [here](http://localhost:5601/app/kibana) and run commands like `DELETE /etl_0` in the console window.
```
$ docker exec esproxy-service curl -X DELETE http://localhost:9200/etl_0
$ docker exec esproxy-service curl -X DELETE http://localhost:9200/file_0
$ docker exec esproxy-service curl -X DELETE http://localhost:9200/file-array-config_0
$ docker exec esproxy-service curl -X DELETE http://localhost:9200/etl-array-config_0
```
5. Now we have verified that the none of the indicies we are building exist in Elasticsearch (`esproxy-service`). Run the commands:
   1. `docker exec tube-service bash -c "python run_config.py"`
   2. `docker exec tube-service bash -c "python run_etl.py"`. This might take a long time and cause high CPU usage. Utilizes Spark(`spark-service`) to do transformations from `sheepdog-service` to `esproxy-service`
   3. If you have errors from the previous step (Elasticsearch docker image crashes or errors from run), check Docker image logs for `esproxy-service` and `spark-service` to debug. Also check Elasticsearch to see if indicies where created. 
6. If successful, run `docker container start guppy-service`
7. Finally, visit https://localhost/files and you should see data loaded on the page ![image](images/files.png)
8. For extra credit you can visit your Elasticsearch instance after step 5 to make sure indicies got created. If if the health is yellow it should be things worked. Notice the `docs.count` corresponds to the number of files that display on the page:
```
health status index               uuid                   pri rep docs.count docs.deleted store.size pri.store.size
green  open   .kibana_1           eFEg2DMCTNmJUfYS31LraA   1   0          0            0       230b           230b
yellow open   file-array-config_0 BKDMRHyyS_Gw4y3hRN8smg   5   1          1            0        4kb            4kb
yellow open   etl-array-config_0  gghK-sI2SsOyFiikSaD7jw   5   1          1            0      4.1kb          4.1kb
yellow open   etl_0               0C3rhjcPRfOmO07Mv_eGPw   5   1         10            0     96.8kb         96.8kb
yellow open   file_0              l1CVwDY3R5iaOCqT5gf4Aw   5   1         13            0      123kb          123kb
```

## Additional Notes
* The most frustrating thing here is that you may have to retry somethings. A docker image but kill itself but when you retry it works.
* You might see errors about `ModuleNotFoundError: No module named 'gdcdictionary'` when runnng `docker exec tube-service bash -c "python run_etl.py"`. Apparently this is a [well known issue](https://cdis.slack.com/archives/CDDPLU1NU/p1600095801039800) on the Slack channel but apparently it is harmless. You can run `docker exec tube-service bash -c "pip3 install gdcdictionary"` first to eliminate the errors.

<!-- 
You may get errors. The point here is that the Elastic proxy and gupy microservices are killed. Then Elastic Proxy spins back up and then Guppy. You can do this manually as well.

Check `http://localhost:9200` to make sure the Elastic Proxy service is up

Check the Docker logs for Guppy

To see all teh indices got to `http://localhost:9200/_cat/indices?v` 
The query string parameter `v` means verbose

To get more help with Elastsearch go to `http://localhost:9200/_cat/` to see list of APIs

I found that if I access Kibana at `"http://localhost:5601/app/kibana` I can create Indices

Click "view in console" at https://www.elastic.co/guide/en/elasticsearch/reference/current/indices-create-index.html -->