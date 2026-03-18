# Gardena Smart System - Home Assistant Integration

## Project Goal
A HACS-compatible Home Assistant custom component for Gardena Smart System API v2. No external Python dependencies — only `aiohttp` (built into HA).

## Architecture
- `custom_components/gardena_smart_system/` — the integration
  - `api/auth.py` — OAuth2 authentication against Husqvarna Group API
  - `api/client.py` — REST API client for locations/devices
  - `api/websocket.py` — WebSocket client for real-time push updates
  - `coordinator.py` — DataUpdateCoordinator, manages devices, WS connections, state
  - `config_flow.py` — UI-based config flow (client_id + client_secret)
  - `lawn_mower.py`, `valve.py`, `sensor.py`, `binary_sensor.py` — HA entity platforms
  - `entities/` — base entity classes
  - `const.py` — constants and service type strings
- `tests/` — pytest tests (conftest.py, test_coordinator.py, test_auth.py, test_config_flow.py)

## Key Design Decisions
- **DEVICE objects from the real API have NO attributes.** Device name, modelType, and serial come from the COMMON service. The coordinator's `_process_included_data()` handles this.
- WebSocket per location with auto-reconnect (exponential backoff)
- Token refresh 5 minutes before expiry
- SSL context created in executor thread (non-blocking)

## What's Done
- Full integration: config flow, API layer (auth/client/websocket), coordinator, all entity platforms
- API smoke test script (`tests/smoke_test_api.py`)
- Fix: device attributes populated from COMMON service (not DEVICE)
- CI: hassfest + pytest in `.github/workflows/validate.yaml`
- README, translations (en/sv), hacs.json, manifest.json

## What Remains for HACS Publication
1. **Add MIT LICENSE file** to repo root (HACS requirement)
2. **Add `hacs/action`** to CI workflow (`.github/workflows/validate.yaml`)
3. **Merge to main** — create PR or merge directly
3. **Create GitHub release** with tag (e.g. `v1.0.0`) — HACS requires at least one release
4. **Test in HACS** — add as custom repository and verify install works

## Development
```bash
pip install -r requirements_dev.txt
pytest tests/ -v
```

## Branch
Working branch: `claude/gardena-smart-system-integration-FKXd3`
