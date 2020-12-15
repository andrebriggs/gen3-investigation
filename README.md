# Learning Gen3


## Local Setup Quickstart

### Prerequisites
* [Increase](https://docs.docker.com/docker-for-mac/#resources) the Docker LinuxVM memory to atleast 3 to 4 GB (or else loading the portal will take a very long time)
* [Create](https://github.com/uc-cdis/compose-services#setting-up-google-oauth-client-id-for-fence) a Google OAUTH client id and password.
* Install [Kitematic](https://kitematic.com) to view Docker logs
* AWS Account (Use a free tier)

### Setup
1. Run `git clone https://github.com/uc-cdis/compose-services.git`
2. Navigate to `$ cd compose-services`
3. Run `$ bash ./creds_setup.sh`
4. Open in VS Code `$ code .`

5. Comment out the following in `./nginx.conf`
```
    location /guppy/ {
            proxy_pass http://guppy-service/;
    }
```
6. Comment out the YAML node at `services.kibana-service` in `./docker-compose.yml`
7. Replace `yourlogin1@gmail.com` with the email address you used to create the Google OAUTH client in `./Secrets/user.yaml`

### Run Docker Compose
1. Run `docker-compose up -d`
2. Open Kitematic and watch the logs. Wait until `portal-service` container is past the `./node_modules/.bin/webpack --bail` command in the logs. This means webpack is done loading. (Can take several minutes)
3. Run the smoke tests using `bash smoke_test.sh localhost` and confirm no errors
4. Visit `http://localhost` and login with your Google account associated with the OAUTH clientid

### Create Program
1. While local instance is running, visit `https://localhost/_root`
2. Enable _User Form Submission_ button and select _program_ from drop-down ![image](images/user_form.png)
3. Enter _123_ and _Program1_ ![image](images/user_form_1.png)
4. Click _Upload submission json from form_ and see json result ![image](images/user_form_2.png)

### Visit Your Program
1. Visit `https://localhost/Program1`
2. Verify it exists

### Generate Test Metadata
1. Generate test metadata
```console
$ export TEST_DATA_PATH="$(pwd)/testData"
$ mkdir -p "$TEST_DATA_PATH"

$ docker run -it -v "${TEST_DATA_PATH}:/mnt/data" --rm --name=dsim --entrypoint=data-simulator quay.io/cdis/data-simulator:master simulate --url https://s3.amazonaws.com/dictionary-artifacts/datadictionary/develop/schema.json --path /mnt/data --program jnkns --project jenkins --max_samples 10
```

### Download and configure up Gen3 Client
```console
$ ./gen3-client configure --profile=cse_profile --cred=~/Downloads/credentials.json --apiendpoint=http://localhost/
2020/12/12 14:47:41 Profile 'cse_profile' has been configured successfully.
```

A `.gen3` directory should existing in your current user directory (e.g. `/Users/<current-user>/.gen3/`. 
View your configuration using `cat /Users/<current-user>/.gen3/config `

### Verify Gen3 Client Access
Verify you have access:
```console
$ ./gen3-client auth --profile=cse_profile
2020/12/12 15:01:23 
You don't currently have access to data from any projects at http://localhost
```

If you get the above warning add the following to the end of your `Secrets/gitops.json` file. According to this [Slack post](https://cdis.slack.com/archives/CDDPLU1NU/p1607962367255400?thread_ts=1607822151.254000&cid=CDDPLU1NU) the message _You don't currently have access to data from any projects_ is misleading. 

```json
  "showArboristAuthzOnProfile": true, 
  "showFenceAuthzOnProfile": false
```

* You will need to restart the `portal-service` using `docker-compose restart portal-service` or shutdown the entire docker compose environment using `docker-compose down` and then `docker-compose up -d`

* After you log back in navigate to https://localhost/identity to verify you have access to resources
![image](images/profile.png)

### Upload test data using Gen3 client

1. Use the test metadata you previously created at `/testData`
2. Run the command `gen3-client upload --profile=cse_profile --upload-path=testData/`
3. Wait for possible retries 

An example of the command and results are below. Notice that the uploads are not reliable and may take several retries. You can see a log of success and failure in `~/.gen3/logs/`

```console
$ ./gen3-client upload --profile=cse_profile --upload-path=testData/

<...snip...>

Submission Results
Finished with 0 retries | 11
Finished with 1 retry   | 0
Finished with 2 retries | 5
Finished with 3 retries | 6
Finished with 4 retries | 1
Finished with 5 retries | 2
Failed                  | 3
TOTAL                   | 28
```

### Mapping uploaded test metadata

From https://gen3.org/resources/user/gen3-client/#3-upload-data-files:
> Files that have been successfully uploaded now have a GUID associated with them, and there is also an associated record in the indexd database. However, in order for the files to show up in the data portal, the files have to be registered in the PostgreSQL database. In other words, indexd records exist for the files, but sheepdog records (that is, structured metadata in the graph model) don’t exist yet. Thus, the files aren’t yet associated with any particular program, project, or node. To create the structured data records for the files via the sheepdog service, Windmill offers a “Map My Files” UI

In the screenshot below you can see the 25 files that got uploaded. Click the _Map My Files button_
![image](images/submission.png)

**Debug Postgres**

`docker exec -it compose-services_postgres_1 bash` 

`psql -Atx postgresql://fence_user:fence_pass@localhost/fence_db`


