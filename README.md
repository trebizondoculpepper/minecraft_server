# Vanilla Minecraft on Fly.io

One vanilla Java server for six players, provisionally hosted in London with
4 GB RAM, a 3 GB Java heap, and a persistent 10 GB world volume. Minecraft is
pinned to 26.3; the Java 25 container is pinned by digest for repeatable deployments.
Review image updates before deploying an existing world.

The owner accepted the Minecraft EULA on 2026-11-15; `EULA=TRUE` records that.
See https://www.minecraft.net/eula.

## Initial provisioning

Authenticate locally with `fly auth login`. Confirm the app name in `fly.toml`
is available, and change it if needed. Run these once, choosing your Fly
organization when prompted:

```sh
fly apps create trebizondoculpepper-minecraft
fly volumes create minecraft_data --region lhr --size 10
fly ips allocate-v4
fly ips allocate-v6
```

The dedicated IPv4, volume, snapshots, and running compute incur charges.
The workflow deploys the server; the app, IP addresses, and persistent volume
are provisioned separately to avoid accidentally duplicating stateful resources.

## GitHub deployment

Create an app-scoped deployment token and send it directly to the repository
secret, without printing it:

```sh
fly tokens create deploy --app trebizondoculpepper-minecraft --expiry 720h | gh secret set FLY_API_TOKEN
gh variable set MINECRAFT_WHITELIST --body 'PlayerOne,PlayerTwo'
```

Use real Java Edition usernames. Authentication and whitelist enforcement are
enabled. Renew the token before its 30-day expiry.

Push the configuration to GitHub, then run **Actions → Deploy Minecraft → Run
workflow**, or `gh workflow run deploy.yml`. Deployments are manual and serialized;
deploying restarts the server and interrupts play. No RCON port is public.

Join `trebizondoculpepper-minecraft.fly.dev:25565` with Java Edition 26.3.

## Inactivity and storage

The image's autostop feature gracefully stops Minecraft about ten minutes after
the last game connection leaves, or ten minutes after startup if nobody joins.
This observes connections rather than an authenticated player count. Fly Proxy
autostop is disabled so the image controls the grace period. A successful exit
stays stopped; Fly autostart wakes the machine on incoming TCP traffic.

Server-list pings and internet scanners can wake it. Startup may take long enough
that a player must retry joining. Idle timers and wake-up must be verified after
the first deployment; CPU and RAM billing stop only when the Machine is stopped.
The world does not advance while stopped.

World files live under `/data` on the volume, outside this repository. Do not
delete the volume. Fly volume snapshots provide a recovery layer, but independent
world backups and a restore test remain follow-up work before relying on the
server for a long-lived world.

Useful checks:

```sh
fly status
fly logs
fly volumes list
fly volumes snapshots list VOLUME_ID
```

Documentation: https://docs.fly.io/reference/fly-proxy-autostop-autostart and
https://docker-minecraft-server.readthedocs.io/en/latest/misc/autopause-autostop/autostop/.
