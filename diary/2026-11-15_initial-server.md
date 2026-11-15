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
