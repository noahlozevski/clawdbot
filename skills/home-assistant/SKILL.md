---
name: home-assistant
description: Control Home Assistant devices via the hass-cli. Use for lights, switches, media players, scenes, and other smart home entities when the user asks to control their home.
metadata: {"clawdbot":{"emoji":"🏠","os":["darwin","linux"],"requires":{"bins":["hass-cli"],"envs":["HASS_TOKEN"]},"install":[{"id":"brew","kind":"brew","formula":"homeassistant-cli","bins":["hass-cli"],"label":"Install hass-cli (brew)"}]}}
---

# Home Assistant

Control smart home devices via Home Assistant using `hass-cli`.

## Setup

### 1. Install hass-cli
```bash
brew install homeassistant-cli
```

### 2. Create a Long-Lived Access Token
1. Go to your Home Assistant instance (e.g., http://homeassistant.local:8123)
2. Click your profile (bottom left)
3. Scroll to **Long-Lived Access Tokens**
4. Click **Create Token**, name it (e.g., "clawd")
5. Copy the token

### 3. Configure environment variables
Add to your shell config (e.g., `~/.zshrc` or `~/.zshrc.secret`):
```bash
export HASS_SERVER="http://192.168.1.48:8123"  # Use IP, not hostname for reliability
export HASS_TOKEN="your_token_here"
```

**Note:** If running as a daemon, you may need to source the config or pass `--server` and `--token` flags explicitly.

## Quick Reference

### List entities
```bash
# All entities
hass-cli state list

# Filter by domain
hass-cli state list | grep "^light\."
hass-cli state list | grep "^switch\."
hass-cli state list | grep "^media_player\."
```

### Get entity state
```bash
hass-cli state get light.kitchen_island_pendants
```

### Control lights
```bash
# Turn on
hass-cli service call light.turn_on --arguments entity_id=light.kitchen_island_pendants

# Turn on with brightness (0-255)
hass-cli service call light.turn_on --arguments entity_id=light.bathroom_lights,brightness=128

# Turn on with brightness percent
hass-cli service call light.turn_on --arguments entity_id=light.bathroom_lights,brightness_pct=50

# Turn off
hass-cli service call light.turn_off --arguments entity_id=light.kitchen_island_pendants

# Toggle
hass-cli service call light.toggle --arguments entity_id=light.kitchen_island_pendants
```

### Control switches
```bash
hass-cli service call switch.turn_on --arguments entity_id=switch.kitchen_under_cabinet_1
hass-cli service call switch.turn_off --arguments entity_id=switch.kitchen_under_cabinet_1
hass-cli service call switch.toggle --arguments entity_id=switch.kitchen_under_cabinet_1
```

### Control media players
```bash
# Play/pause
hass-cli service call media_player.media_play_pause --arguments entity_id=media_player.living_room_speakers

# Volume (0.0 - 1.0)
hass-cli service call media_player.volume_set --arguments entity_id=media_player.living_room_speakers,volume_level=0.5

# Mute
hass-cli service call media_player.volume_mute --arguments entity_id=media_player.living_room_speakers,is_volume_muted=true
```

### Activate scenes
```bash
hass-cli service call scene.turn_on --arguments entity_id=scene.welcome
```

### Multiple entities at once
```bash
# Multiple entities - use quotes and proper list syntax
hass-cli service call light.turn_off --arguments 'entity_id=["light.kitchen_island_pendants","light.bathroom_lights"]'

# Or just call separately (more reliable)
hass-cli service call light.turn_off --arguments entity_id=light.kitchen_island_pendants
hass-cli service call light.turn_off --arguments entity_id=light.bathroom_lights
```

## Common Patterns

### Source env and run (for daemon context)
```bash
source ~/.zshrc.secret && hass-cli --server http://192.168.1.48:8123 state list
```

### Check if entity is on/off
```bash
hass-cli state get light.bathroom_lights | grep -q "state: on" && echo "ON" || echo "OFF"
```

### Output as JSON
```bash
hass-cli -o json state get light.bathroom_lights
```

## Troubleshooting

### Connection errors with hostname
Use IP address instead of `homeassistant.local`:
```bash
hass-cli --server http://192.168.1.48:8123 state list
```

### 401 Unauthorized
Token is invalid or expired. Create a new long-lived access token.

### Entity not found
List all entities to find the correct entity_id:
```bash
hass-cli state list | grep -i "kitchen"
```

## Entity Domains

| Domain | Description | Example |
|--------|-------------|---------|
| `light.*` | Lights with brightness/color | `light.bathroom_lights` |
| `switch.*` | Simple on/off switches | `switch.kitchen_under_cabinet_1` |
| `media_player.*` | Speakers, TVs, receivers | `media_player.living_room_speakers` |
| `scene.*` | Predefined scenes | `scene.welcome` |
| `climate.*` | Thermostats, HVAC | `climate.living_room` |
| `cover.*` | Blinds, garage doors | `cover.garage_door` |
| `lock.*` | Smart locks | `lock.front_door` |
| `fan.*` | Fans | `fan.bedroom_fan` |
| `sensor.*` | Read-only sensors | `sensor.temperature` |
| `binary_sensor.*` | On/off sensors | `binary_sensor.motion` |

## References
- [Home Assistant CLI docs](https://github.com/home-assistant-ecosystem/home-assistant-cli)
- [Home Assistant services](https://www.home-assistant.io/docs/scripts/service-calls/)
