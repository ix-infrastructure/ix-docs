# ix docker

Manage the Ix backend Docker containers (ArangoDB + Memory Layer).

## Usage

```sh
ix docker <action>
```

## Actions

### start

```sh
ix docker start
```

Start ArangoDB and the Memory Layer. Downloads the docker-compose file if not present. Waits for health checks to pass.

### stop

```sh
ix docker stop
ix docker stop --remove-data
```

Stop containers. Data is preserved by default. Use `--remove-data` to also delete the ArangoDB volume.

### status

```sh
ix docker status
```

Show container status and health check results.

### logs

```sh
ix docker logs
ix docker logs -f
```

Tail container logs. `-f` follows output (default).

### restart

```sh
ix docker restart
```

Restart both containers.
