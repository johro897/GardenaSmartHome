# Gardena Smart System — Home Assistant Integration

A modern Home Assistant custom component for the Gardena Smart System API v2.

[![HACS](https://img.shields.io/badge/HACS-Custom-orange.svg)](https://hacs.xyz)
[![HA Version](https://img.shields.io/badge/Home%20Assistant-2023.9%2B-blue.svg)](https://www.home-assistant.io)

## Features

- **Lawn mower** (Sileno): mowing / docked / paused / error + start, dock, pause
- **Irrigation valves** (Irrigation Control, Water Control): open/close with duration
- **Soil sensors**: temperature (°C), humidity (%), light intensity (lux)
- **Binary sensors**: gateway connectivity, WebSocket connection status
- **Real-time updates** via WebSocket with automatic reconnection
- **Multi-location support**

## Installation

### Via HACS (recommended)

1. Open HACS in Home Assistant
2. Go to **Integrations** → **⋮** → **Custom repositories**
3. Add `https://github.com/johro897/GardenaSmartHome` — category: **Integration**
4. Search for "Gardena Smart System" and install
5. Restart Home Assistant

### Manual

Copy `custom_components/gardena_smart_system/` to your `/config/custom_components/` and restart.

## Prerequisites

1. Create an account at [developer.husqvarnagroup.cloud](https://developer.husqvarnagroup.cloud)
2. Create an application and connect **Authentication API** + **GARDENA smart system API**
3. Note your **Application Key** and **Application Secret**

## Configuration

Settings → Devices & Services → Add Integration → **Gardena Smart System**

## Supported devices

| Device | HA Platform | States | Controls |
|---|---|---|---|
| Sileno Mower | `lawn_mower` | mowing, docked, paused, error | start, dock, pause |
| Irrigation Control | `valve` | open, closed | open (duration), close |
| Water Control | `valve` | open, closed | open (duration), close |
| Soil Sensor | `sensor` | temp, humidity, light | — |
| Gateway | `binary_sensor` | online / offline | — |

## Architecture

- No external dependencies — uses only `aiohttp` (built into HA)
- WebSocket push updates with exponential backoff reconnection (5s → 10s → 30s → 60s)
- Proactive token refresh — 5 minutes before expiry
- Non-blocking SSL context (executor thread)
- Proper cleanup on unload

## Troubleshooting

**WebSocket disconnects** — automatic reconnect with backoff. Check the `binary_sensor` entities for live connection status.

**Auth errors** — verify credentials at [developer.husqvarnagroup.cloud](https://developer.husqvarnagroup.cloud) and confirm the Gardena API is connected to your application.

## License

MIT — forked from [CorSeptem/GardenaSmartHome](https://github.com/CorSeptem/GardenaSmartHome)
