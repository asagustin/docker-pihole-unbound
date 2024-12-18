# Pi-Hole + Unbound

## Description

This Docker deployment runs both Pi-Hole and Unbound in a single container.

The base image for the container is the [official Pi-Hole container](https://hub.docker.com/r/pihole/pihole), with an extra build step added to install the Unbound resolver directly into to the container based on [instructions provided directly by the Pi-Hole team](https://docs.pi-hole.net/guides/unbound/).

## Usage

First create a `.env` file to substitute variables for your deployment.

### Pi-hole environment variables

> Vars and descriptions replicated from the [official pihole container](https://github.com/pi-hole/docker-pi-hole/#environment-variables):

| Variable | Default | Value | Description |
| -------- | ------- | ----- | ---------- |
| `TZ` | UTC | `<Timezone>` | Set your [timezone](https://en.wikipedia.org/wiki/List_of_tz_database_time_zones) to make sure logs rotate at local midnight instead of at UTC midnight.|
| `WEBPASSWORD` | random | `<Admin password>` | http://pi.hole/admin password. Run `docker logs pihole \| grep random` to find your random pass.|
| `FTLCONF_LOCAL_IPV4` | unset | `<Host's IP>` | Set to your server's LAN IP, used by web block modes and lighttpd bind address.|
| `REV_SERVER` | `false` | `<"true"\|"false">` | Enable DNS conditional forwarding for device name resolution |
| `REV_SERVER_DOMAIN` | unset | Network Domain | If conditional forwarding is enabled, set the domain of the local network router |
| `REV_SERVER_TARGET` | unset | Router's IP | If conditional forwarding is enabled, set the IP of the local network router |
| `REV_SERVER_CIDR` | unset | Reverse DNS | If conditional forwarding is enabled, set the reverse DNS zone (e.g. `192.168.0.0/24`) |
| `WEBTHEME` | `default-light` | `<"default-dark"\|"default-darker"\|"default-light"\|"default-auto"\|"lcars">`| User interface theme to use.|

Example `.env` file in the same directory as your `docker-compose.yaml` file:

```
FTLCONF_LOCAL_IPV4=192.168.1.10
TZ=America/Los_Angeles
WEBPASSWORD=QWERTY123456asdfASDF
REV_SERVER=true
REV_SERVER_DOMAIN=local
REV_SERVER_TARGET=192.168.1.1
REV_SERVER_CIDR=192.168.0.0/16
HOSTNAME=pihole
DOMAIN_NAME=pihole.local
PIHOLE_WEBPORT=80
WEBTHEME=default-light
```

### Using Portainer
#### Port mapping
```
- 53:53 TCP
- 53:53 UDP
- 1010:80 TCP
- 4443:443 TCP
```

#### Volumes
```
- container: /etc/dnsmasq.d
  host: /portainer/pihole-unbound/dns

- container: /etc/pihole
  host: /portainer/pihole-unbound
```

#### Environment variables

| Variable                          | Value                                                                          | Description                                                                                                                                              |
|-----------------------------------|--------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------|
| `TZ`                              | `<Timezone>`                                                                   | Set your [timezone](https://en.wikipedia.org/wiki/List_of_tz_database_time_zones) to make sure logs rotate at local midnight instead of at UTC midnight. |
| `WEBPASSWORD`                     | `<Admin password>`                                                             | http://pi.hole/admin password. Run `docker logs pihole \| grep random` to find your random pass.                                                         |
| `FTLCONF_LOCAL_IPV4`              | `<Host's IP>`                                                                  | Set to your server's LAN IP, used by web block modes and lighttpd bind address.                                                                          |
| `REV_SERVER`                      | `<"true"\|"false">`                                                            | Enable DNS conditional forwarding for device name resolution                                                                                             |
| `REV_SERVER_DOMAIN`               | Network Domain                                                                 | If conditional forwarding is enabled, set the domain of the local network router                                                                         |
| `REV_SERVER_TARGET`               | Router's IP                                                                    | If conditional forwarding is enabled, set the IP of the local network router                                                                             |
| `REV_SERVER_CIDR`                 | Reverse DNS                                                                    | If conditional forwarding is enabled, set the reverse DNS zone (e.g. `192.168.0.0/24`)                                                                   |
| `WEBTHEME`                        | `<"default-dark"\|"default-darker"\|"default-light"\|"default-auto"\|"lcars">` | User interface theme to use.                                                                                                                             |
| `DNSSEC`                          | `true`                                                                         | Flag to enable or disable DNS SEC                                                                                                                        |
| `DNS1`                            | `127.0.0.1#5335`                                                               | Recursive DNS 1                                                                                                                                          |
| `DNS2`                            | `127.0.0.1#5335`                                                               | Recursive DNS 2                                                                                                                                          |
| `PATH`                            | `/opt/pihole:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin`     | Path                                                                                                                                                     |
| `phpver`                          | `php`                                                                          |                                                                                                                                                      |
| `PHP_ERROR_LOG`                   | `/var/log/lighttpd/error-pihole.log`                                           |                                                                                                                                                      |
| `IPv6`                            | `<"true"\|"false">`                                                            |                                                                                                                                                      |
| `S6_KEEP_ENV`                     | `1`                                                                            |                                                                                                                                                      |
| `S6_BEHAVIOUR_IF_STAGE2_FAILS`    | `2`                                                                            |                                                                                                                                                      |
| `S6_CMD_WAIT_FOR_SERVICES_MAXTIME`| `0`                                                                            |                                                                                                                                                      |
| `FTL_CMD`                         | `no-daemon`                                                                    |                                                                                                                                                      |
| `DNSMASQ_USER`                    | `pihole`                                                                       |                                                                                                                                                      |
| `PUID`                            | `1000`                                                                         |                                                                                                                                                      |
| `GUID`                            | `1000`                                                                         |                                                                                                                                                      |