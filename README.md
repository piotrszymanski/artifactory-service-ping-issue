# Replication steps

> Note: If you're running on a Linux machine, then it might be necessary to grant write permissions to the `artifactory` directory before running the replication steps:
>
> ```
> chmod -R a+w artifactory
> ```

1. Start Artifactory using Docker compose:

```
> docker-compose up -d && docker-compose logs -f
...
artifactory-server | ###############################################################
artifactory-server | ###   All services started successfully in 38.254 seconds   ###
artifactory-server | ###############################################################
...
```

2. Once Artifactory starts, press Ctrl-C to exit the log printout.
3. Inspect the `event-service.log` to see the error message:

```
> grep "Service registry ping failed" artifactory/log/event-service.log
2022-06-20T09:46:38.526Z [jfevt] [INFO ] [3def914f657ff9f0] [access_thin_client.go:104     ] [main                ] - Cluster join: Retry 5: Service registry ping failed, will retry. Error: Failed to ping access, response status: 404 Not Found (returned 404) [startup]
```

4. Restart the containers:

```
> docker-compose restart && docker-compose logs -f
```

5. Wait for Artifactory to start again. Then, press Ctrl-C to exit the log printout.
6. Inspect the `event-service.log` again to see more error messages:

```
> grep "Service registry ping failed" artifactory/log/event-service.log
2022-06-20T09:46:38.526Z [jfevt] [INFO ] [3def914f657ff9f0] [access_thin_client.go:104     ] [main                ] - Cluster join: Retry 5: Service registry ping failed, will retry. Error: Failed to ping access, response status: 404 Not Found (returned 404) [startup]
2022-06-20T09:50:25.460Z [jfevt] [INFO ] [7dd1b598c9a03c89] [access_thin_client.go:104     ] [main                ] - Cluster join: Retry 5: Service registry ping failed, will retry. Error: Error while trying to connect to local router at address 'http://localhost:8046/access/api/v1/system/ping': Get "http://localhost:8046/access/api/v1/system/ping": dial tcp 127.0.0.1:8046: connect: connection refused [startup]
2022-06-20T09:50:30.444Z [jfevt] [INFO ] [7dd1b598c9a03c89] [access_thin_client.go:104     ] [main                ] - Cluster join: Retry 10: Service registry ping failed, will retry. Error: Error while trying to connect to local router at address 'http://localhost:8046/access/api/v1/system/ping': Get "http://localhost:8046/access/api/v1/system/ping": dial tcp 127.0.0.1:8046: connect: connection refused [startup]
2022-06-20T09:50:35.451Z [jfevt] [INFO ] [7dd1b598c9a03c89] [access_thin_client.go:104     ] [main                ] - Cluster join: Retry 15: Service registry ping failed, will retry. Error: Error while trying to connect to local router at address 'http://localhost:8046/access/api/v1/system/ping': Get "http://localhost:8046/access/api/v1/system/ping": dial tcp 127.0.0.1:8046: connect: connection refused [startup]
```
