# Docker Exit Codes

[For further reading and reference: https://betterprogramming.pub/understanding-docker-container-exit-codes-5ee79a1d58f6](https://betterprogramming.pub/understanding-docker-container-exit-codes-5ee79a1d58f6)

## How to Find Exit Codes

```bash
docker ps --filter "status=exited" # list all
docker ps -a | grep <container-name> # grep for it
docker inspect <container-id> --format='{{.State.ExitCode}}' # inspect by container id
```

## Exit Codes

| Exit Code | Explanation                                                                                                |
| --------- | ---------------------------------------------------------------------------------------------------------- |
| 0         | Absence of an attached foreground process                                                                  |
| 1         | Indicates failure due to application error                                                                 |
| 130       | Indicates failure as container received termination (++ctrl+c++)                                           |
| 137       | Indicates failure as container received SIGKILL (Manual intervention of "oom-killer") (e.g. OUT-OF-MEMORY) |
| 139       | Indicates failure as container received SIGSEGV                                                            |
| 143       | Indicates failure as container received SIGTERM                                                            |


### Exit Code 0

- Exit code 0 indicates that the specific container does not have a foreground process attached.
- This exit code is the exception to all the other exit codes to follow. It does not necessarily mean something bad happened.
- Developers use this exit code if they want to automatically stop their container once it has completed its job.

### Exit Code 1

- Indicates that the container stopped due to either an application error or an  incorrect reference in Dockerfile to a file that is not present in the  container.
- An application error can be as simple as “divide by 0” or as complex as  “Reference to a bean name that conflicts with existing, non-compatible bean definition of same name and class.”
- An incorrect reference in Dockerfile to a file not present in the container can be as simple as a typo (the example below has `sample.ja` instead of `sample.jar`)

### Exit Code 137

- This indicates that container received SIGKILL
- A common event that initiates a SIGKILL is a docker kill. This can be initiated either manually by user or by the docker daemon:

```bash
docker kill <container-id>
```

- `docker kill` can be initiated manually by the user or by the host machine. If initiated by host machine, then it is generally due to being out of memory. To confirm if the container exited due to being out of memory, verify `docker inspect` against the container id for the section below and check if `OOMKilled` is true (which would indicate it is out of memory):

```json
"State": {
 "Status": "exited",
 "Running": false,
 "Paused": false,
 "Restarting": false,
 "OOMKilled": true,
 "Dead": false,
 "Pid": 0,
 "ExitCode": 137,
 "Error": "",
 "StartedAt": "2019-10-21T01:13:51.7340288Z",
 "FinishedAt": "2019-10-21T01:13:51.7961614Z"
}
```



### Exit Code 139

- This indicates that container received SIGSEGV
- SIGSEGV indicates a segmentation fault. This occurs when a program attempts to access a [memory](https://en.wikipedia.org/wiki/Computer_memory) location that it’s not allowed to access, or attempts to access a memory location in a way that’s not allowed.
- From the Docker container standpoint, this either indicates an issue with  the application code or sometimes an issue with the base images used by the container.

### Exit Code 143

- This indicates that container received SIGTERM.
- Common events that initiate a SIGTERM are `docker stop` or `docker-compose stop`. In this case there was a manual termination that forced the container to exit:

```bash
docker stop <container-id>
# -- or --
docker-compose down <container-id>
```

- **Note:** sometimes `docker stop` can also result in exit code 137. This typically happens if the  application tied to the container doesn’t handle SIGTERM — the docker  daemon waits ten seconds then issues SIGKILL

### Uncommon Exit Codes

| Exit Code | Explanation                                                   |
| --------- | ------------------------------------------------------------- |
| 126       | Permission problem or command is not executable               |
| 127       | Possible typos in shell script with unrecognizable characters |
