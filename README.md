# Home Assistant stack (Unraid)

Home Assistant + Mosquitto + Zigbee2MQTT via Docker Compose (run through the Unraid Compose Manager, stack folder `/mnt/user/appdata/homeassistant_stack`).
Runtime data lives in `${APPDATA}` (default `/mnt/user/appdata`), not in this repo. Secrets live only in `.env` (git-ignored).

## Setup on the server
1. `cp .env.example .env`, then set `ZIGBEE_DEVICE` (`ls -l /dev/serial/by-id/`), `ZIGBEE_ADAPTER` and the secrets (see constraints in `.env.example`).
2. Create the Mosquitto users (once, before the first start), choosing a password at each prompt:
   ```
   mkdir -p /mnt/user/appdata/mosquitto/auth
   docker run --rm -it -v /mnt/user/appdata/mosquitto/auth:/auth eclipse-mosquitto:2.0.22 mosquitto_passwd -c /auth/passwd z2m
   docker run --rm -it -v /mnt/user/appdata/mosquitto/auth:/auth eclipse-mosquitto:2.0.22 mosquitto_passwd /auth/passwd homeassistant
   chown 1883:1883 /mnt/user/appdata/mosquitto/auth/passwd && chmod 600 /mnt/user/appdata/mosquitto/auth/passwd
   ```
   The `z2m` password must equal `Z2M_MQTT_PASSWORD` in `.env`. The first command uses `-c` (creates the file), the second does not (adds a user).
3. Compose Up in the Unraid UI.

## Ports
- Home Assistant: 8123 (own login)
- Zigbee2MQTT frontend: 8081 (login with `Z2M_FRONTEND_TOKEN`)
- MQTT: 1883, **localhost only** (not reachable from the LAN)

## Home Assistant
Settings > Devices & services > **MQTT** > Configure: broker `localhost`, port `1883`, user `homeassistant` and its password.

## Update
`git pull`, then Compose Down / Up (or Update stack) in the Unraid UI.
