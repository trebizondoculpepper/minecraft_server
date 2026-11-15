# Initial server configuration

The owner requested a vanilla Java server on Fly.io for at most six players,
with inactivity shutdown and deployment through GitHub Actions. The owner
explicitly accepted the Minecraft EULA.

Added a single-machine Fly configuration, a manually triggered deployment
workflow, whitelist enforcement, persistent world storage, and image-provided
inactivity shutdown. London is provisional. Initial resources are deliberately
provisioned separately from repeat deployments. Deployment awaits confirmed Fly
authentication and the whitelist usernames. Actual gameplay, clean shutdown,
wake-up, and recovery still need verification on the deployed machine.

The owner subsequently requested an initially open server, with a whitelist to
be enabled after friends have joined. Disabled whitelist enforcement and removed
the workflow's mandatory username check. Minecraft account authentication stays
enabled. Documented explicit username configuration and redeployment to lock down
access later.

After the owner authenticated Fly, provisioned the app in the personal
organization, a 10 GB encrypted London volume with scheduled snapshots, dedicated
IPv4 and IPv6 addresses, and a 30-day app deployment token stored directly in
GitHub's FLY_API_TOKEN secret. No token was printed or committed.

GitHub Actions run 43728003734 succeeded. The new machine started, generated a
fresh survival world, and answered an external Minecraft status handshake with
version 26.3, protocol 777, and zero of six players online. Fly's strict config
validation passed. Container logs confirmed autostop enabled and the whitelist
disabled. The full ten-minute idle shutdown/wake cycle and a backup restore have
not yet been observed. Public endpoint:
trebizondoculpepper-minecraft.fly.dev:25565 (188.93.144.102).
