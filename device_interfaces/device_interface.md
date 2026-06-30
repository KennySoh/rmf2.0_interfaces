# RMF2.0 Device Interface
**Version 1.0.0 — April 2026**

---

This is a **payload contract only** — deliberately transport-agnostic.  
The same JSON payloads are reusable over AMQP, MQTT, or ROS 2. No transport logic lives here.

- [`schemas/`](./schemas/) — JSON Schema for every message type
- [`example_payloads/`](./example_payloads/) — one working JSON example per schema

---

## Connection State

Wire values: `ONLINE` · `OFFLINE`  
`CONNECTION_BROKEN` is never published — derived by the FCS from message staleness.

| | Schema | Example |
|-|--------|---------|
| DeviceController | [`device_controller_connection.schema.json`](./schemas/device_controller_connection.schema.json) | [`device_controller_connection.json`](./example_payloads/device_controller_connection.json) |
| Device | [`device_connection.schema.json`](./schemas/device_connection.schema.json) | [`device_connection.json`](./example_payloads/device_connection.json) |

---

## VDA5050 Mobile Robot

| | Schema | Example |
|-|--------|---------|
| State | [`vda5050_mobile_robot_state.schema.json`](./schemas/vda5050_mobile_robot_state.schema.json) | [`vda5050_mobile_robot_state.json`](./example_payloads/vda5050_mobile_robot_state.json) |
| Order | [`vda5050_mobile_robot_order.schema.json`](./schemas/vda5050_mobile_robot_order.schema.json) | [`vda5050_mobile_robot_order.json`](./example_payloads/vda5050_mobile_robot_order.json) |
| Instant Actions | [`vda5050_mobile_robot_instant_actions.schema.json`](./schemas/vda5050_mobile_robot_instant_actions.schema.json) | [`vda5050_mobile_robot_instant_actions.json`](./example_payloads/vda5050_mobile_robot_instant_actions.json) |
| Factsheet | [`vda5050_mobile_robot_factsheet.schema.json`](./schemas/vda5050_mobile_robot_factsheet.schema.json) | [`vda5050_mobile_robot_factsheet.json`](./example_payloads/vda5050_mobile_robot_factsheet.json) |
| Visualization | [`vda5050_mobile_robot_visualization.schema.json`](./schemas/vda5050_mobile_robot_visualization.schema.json) | [`vda5050_mobile_robot_visualization.json`](./example_payloads/vda5050_mobile_robot_visualization.json) |

`order.phase`: `PHASE_NO_ORDER` · `PHASE_ACCEPTED` · `PHASE_RUNNING` · `PHASE_COMPLETED` · `PHASE_FAILED`  
`blockingType`: `NONE` · `SOFT` · `HARD`  
Nodes use even `sequenceId` (0, 2, 4…); edges use odd (1, 3, 5…). Omit `edges` to auto-generate.  
Factsheet published once on startup or on `stateRequest`. Instant actions execute immediately.

---

## Workcell

| | Schema | Example |
|-|--------|---------|
| State | [`workcell_state.schema.json`](./schemas/workcell_state.schema.json) | [`workcell_state.json`](./example_payloads/workcell_state.json) |
| Request | [`workcell_request.schema.json`](./schemas/workcell_request.schema.json) | [`workcell_request.json`](./example_payloads/workcell_request.json) |

Sub-device `data` blocks are open — any telemetry is allowed.  
`command` examples: `start_job` · `pause` · `resume` · `cancel` · `home_all`

---

## Charger

| | Schema | Example |
|-|--------|---------|
| State | [`charger_state.schema.json`](./schemas/charger_state.schema.json) | [`charger_state.json`](./example_payloads/charger_state.json) |
| Request | [`charger_request.schema.json`](./schemas/charger_request.schema.json) | [`charger_request.json`](./example_payloads/charger_request.json) |

`state`: `idle` · `reserved` · `charging` · `error`  
`command`: `reserve` · `start_charge` · `stop_charge` · `release`

---

## Door

| | Schema | Example |
|-|--------|---------|
| State | [`door_state.schema.json`](./schemas/door_state.schema.json) | [`door_state.json`](./example_payloads/door_state.json) |
| Request | [`door_request.schema.json`](./schemas/door_request.schema.json) | [`door_request.json`](./example_payloads/door_request.json) |

`current_mode` / `requested_mode`: `MODE_CLOSED` · `MODE_MOVING` · `MODE_OPEN` · `MODE_OFFLINE` · `MODE_UNKNOWN`

---

## Lift

| | Schema | Example |
|-|--------|---------|
| State | [`lift_state.schema.json`](./schemas/lift_state.schema.json) | [`lift_state.json`](./example_payloads/lift_state.json) |
| Request | [`lift_request.schema.json`](./schemas/lift_request.schema.json) | [`lift_request.json`](./example_payloads/lift_request.json) |

`door_state`: `0`=CLOSED `1`=MOVING `2`=OPEN  
`motion_state`: `0`=STOPPED `1`=UP `2`=DOWN `3`=UNKNOWN  
`current_mode`: `1`=HUMAN `2`=AGV `4`=OFFLINE  
`request_type`: `0`=END_SESSION `1`=AGV_MODE `2`=HUMAN_MODE
