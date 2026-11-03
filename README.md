# Home Assistant stack (Unraid)

Home Assistant + Mosquitto + Zigbee2MQTT via Docker Compose.
Runtime data lives in `${APPDATA}` (default `/mnt/user/appdata`), not in this repo.

## Setup on the server
1. `git clone <repo> /mnt/user/appdata/homeassistant-stack && cd` into it
2. `cp .env.example .env`, then set `ZIGBEE_DEVICE` (`ls -l /dev/serial/by-id/`) and `ZIGBEE_ADAPTER`
3. `docker compose up -d`

## Ports
- Home Assistant: 8123
- Zigbee2MQTT frontend: 8081
- MQTT: 1883

## Home Assistant
Settings > Devices & services > add **MQTT**, broker `localhost`, port `1883`.

## Update
`git pull && docker compose pull && docker compose up -d`
