# Deployment using Docker

The lorawan-server image is available on
[Docker Cloud](https://cloud.docker.com/app/gotthardp/repository/docker/gotthardp/lorawan-server/general).
Two tags are available: `stable` and `latest`.

You can run a tag (e.g. `latest`) by:

```bash
docker pull gotthardp/lorawan-server:latest

docker run --detach \
  --name lorawan \
  --hostname lorawan \
  --restart on-failure \
  --volume /path/to/local:/storage \
  --publish 8080:8080/tcp \
  --publish 1680:1680/udp \
  gotthardp/lorawan-server:latest
```

The `/path/to/local` shall point to a local directory, where the Mnesia database
and server logs will be stored.

To export different port numbers change the *first* number of the `publish`
parameter to the desired port number. The syntax is `--publish hostPort:containerPort`
so to receive packet_forwarder data at port 1700 simply use `--publish 1700:1680/udp`.

## Automatic Updates

To automatically update and restart your server whenever an update is available
to the respective branch (stable or master) you can use the
[Watchtower](https://github.com/v2tec/watchtower):

```bash
docker run -d \
  --name watchtower \
  -v /var/run/docker.sock:/var/run/docker.sock \
  v2tec/watchtower \
  --cleanup \
  lorawan
```

In development environment you may want to start the lorawan connector with the
`--rm` option instead of `--restart on-failure` to auto-delete old containers.
(You cannot use both options at the same time.)
