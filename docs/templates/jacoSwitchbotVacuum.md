# SwitchBot Vacuum (`jaco/switchbot-vacuum`)

Platform template for the [SwitchBot Vacuum](https://github.com/jaco/switchbot-vacuum) Home Assistant custom integration, supporting the **SwitchBot Robot Vacuum S10 (`WoSweeperOrigin`)**.

---

## Map source entity

The `switchbot_vacuum` integration exposes a Map camera entity (`camera.<device_name>_map`, e.g. `camera.switchbot_vacuum_map`) that provides both the floor plan PNG and the `calibration_points` / `rooms` attributes XVMC reads directly.

---

## Minimal configuration

```yaml
type: custom:xiaomi-vacuum-map-card
entity: vacuum.switchbot_vacuum
vacuum_platform: jaco/switchbot-vacuum
map_source:
  camera: camera.switchbot_vacuum_map
calibration_source:
  camera: true
map_modes:
  - template: vacuum_clean_segment
```

---

## Room selection

The camera entity automatically exposes room polygon outlines, centroids, and contextual icons in `camera.<device_name>_map` attributes:

```json
{
  "ROOM_001": {
    "name": "Living room",
    "outline": [[x1, y1], [x2, y2], ...],
    "x": 120,
    "y": 150,
    "icon": "mdi:sofa"
  }
}
```

In the card visual editor, clicking **"Generate rooms config"** will read the `rooms` attribute and populate all rooms and outlines automatically.

When rooms are selected and cleaning is initiated, the card calls:

```yaml
service: switchbot_vacuum.clean_rooms
data:
  entity_id: vacuum.switchbot_vacuum
  rooms:
    - ROOM_001
```

