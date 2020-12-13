# Learning Gen3


## Local Setup Quickstart

**Prerequisites**
* [Increase](https://docs.docker.com/docker-for-mac/#resources) the Docker LinuxVM memory to atleast 3 to 4 GB (or else loading the portal will take a very long time)
* [Create](https://github.com/uc-cdis/compose-services#setting-up-google-oauth-client-id-for-fence) a Google OAUTH client id and password.
* Install [Kitematic](https://kitematic.com) to view Docker logs
* AWS Account (Use a free tier)

**Setup**
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

**Run Docker Compose**
1. Run `docker-compose up -d`
2. Open Kitematic and watch the logs. Wait until `portal-service` container is past the `./node_modules/.bin/webpack --bail` command in the logs. This means webpack is done loading. (Can take several minutes)
3. Run the smoke tests using `bash smoke_test.sh localhost` and confirm no errors
4. Visit `http://localhost` and login with your Google account associated with the OAUTH clientid

**Create Program**
1. While local instance is running, visit `https://localhost/_root`
2. Enable _User Form Submission_ button and select _program_ from drop-down ![image](images/user_form.png)
3. Enter _123_ and _Program1_ ![image](images/user_form_1.png)
4. Click _Upload submission json from form_ and see json result ![image](images/user_form_2.png)

**Visit Your Program**
1. Visit `https://localhost/Program1`

**Generate Test Metadata**
1.
```console
export TEST_DATA_PATH="$(pwd)/testData"
mkdir -p "$TEST_DATA_PATH"

docker run -it -v "${TEST_DATA_PATH}:/mnt/data" --rm --name=dsim --entrypoint=data-simulator quay.io/cdis/data-simulator:master simulate --url https://s3.amazonaws.com/dictionary-artifacts/datadictionary/develop/schema.json --path /mnt/data --program jnkns --project jenkins --max_samples 10
```


**Download and configure up Gen3 Client**
```console
$ ./gen3-client configure --profile=cse_profile --cred=~/Downloads/credentials.json --apiendpoint=http://localhost/
2020/12/12 14:47:41 Profile 'cse_profile' has been configured successfully.
```

A `.gen3` directory should existing in your current user directory (e.g. `/Users/<current-user>/.gen3/`. 
View your configuration using `cat /Users/<current-user>/.gen3/config `

Verify you have access:
```console
./gen3-client auth --profile=cse_profile
2020/12/12 15:01:23 
You don't currently have access to data from any projects at http://localhost
```

**Debug Postgres**

`docker exec -it compose-services_postgres_1 bash` 

`psql -Atx postgresql://fence_user:fence_pass@localhost/fence_db`