# Changelog
**Warning:** This is not an official add-on and is not affiliated with SMA. Use at your own risk. This software is experimental.

## 0.0.20
- When Balanced overwrite is active, poll sensor data every second for faster reaction to grid changes. In other modes, keep the configured polling interval.

## 0.0.19
- Balanced: Do not send any Modbus write when internally Automatic (battery_control=0) — no more 803/0 writes in this case.
- Balanced: Dynamically adjust and publish battery_control. Increase by grid_draw when drawing from grid; decrease by grid_feed when exporting. Clamp within max. If it reaches 0, stop writing and switch to internal Automatic.
- Balanced: Still only acts when Overwrite is set to Balanced, and only sends discharge commands. Skips post_command_delay_ms for responsiveness.

## 0.0.18
- Balanced: if battery_control becomes 0 or remains 0, treat as internal Automatic and do not send Modbus commands (prevents discharge→automatic→discharge oscillation).
- Balanced: ignore post_command_delay_ms to react quickly to grid_draw/grid_feed changes; we still read back immediately without waiting.
- Kept guard that Balanced only sends discharge commands and only when Overwrite is set to Balanced (not in Automatic mode).

## 0.0.17
- Balanced mode now sends Modbus commands only when Overwrite is set to Balanced (not in Automatic mode), and only issues discharge commands (no writes for automatic/no-control).

## 0.0.16
- Add new "Balanced" option to Automatic and Overwrite Logic with grid-based discharge control.
- In Balanced: if grid_draw=0 and battery_discharge_power=0 → switch to Automatic (no control) and set battery_control to 0; if grid_draw>0 → discharge with battery_control+grid_draw; if grid_draw=0 and grid_feed>0 → discharge with battery_control-grid_feed if positive, otherwise switch to Automatic and set battery_control to 0.
- Remove old Current Logic Selection select entity by clearing its MQTT discovery topics; keep new sensor entity (Unknown on start until first update).

## 0.0.15
- Make Current Logic Selection read-only by publishing it as a sensor (no command entity in Home Assistant).
- Ensure no Modbus command is sent when battery_control changes while in Automatic mode (logic already prevents writes; documented behavior).
- Reduce telemetry noise: publish sensors on startup, only when values change, and force a refresh every 30 minutes.

## 0.0.14
- Make post-command stabilization delay configurable via environment variable POST_COMMAND_DELAY_MS and add-on options (config.json/config.yaml). Default is 1600 ms.

## 0.0.13
- Increase post-command stabilization delay by 300ms (now 500ms) before reading back sensor values.

## 0.0.12
- Read and publish sensor data immediately after sending Modbus settings.
- Add a short delay after successful write to ensure fresh values are read from the inverter.

## 0.0.11
- Always publish discovery for selects (automatic, overwrite, current) and battery control number so Home Assistant can send commands.
- Wait for MQTT wildcard subscription to complete to ensure commands are received immediately.

## 0.0.10
- Ensure Modbus commands are sent immediately on MQTT commands by serializing Modbus access with a mutex.
- Prevent interference between reads and writes and between concurrent commands by locking around Modbus IO and command application.

## 0.0.9
- Performance optimizations:
  - Avoid redundant MQTT publishes by caching last sensor values.
  - Reduced allocations by caching MQTT topic prefixes and using efficient number formatting.
  - Replaced per-iteration map with static register list in Modbus read loop.
  - Non-blocking MQTT publishes for high-frequency telemetry (retain=false).

## 0.0.8
- trying to optimize reconnect

## 0.0.7
- added some more sensors
  - battery_temperature
  - inverter_temperature
  - battery_health
  - battery_status
  - dc1_current
  - dc1_voltage
  - dc1_power
  - dc2_current
  - dc2_voltage
  - dc2_power

## 0.0.6
- Add currentLogicSelection to see the current active Modus
- Check for broken pipe at modbus connection (also monitor count / time)
- make deviceId configurable
- Change Hardcoded deviceId to configurable deviceId

## 0.0.5
- Removed Check and Reset, which caused to remove the OverwriteLogicSelection to reset

## 0.0.4
- Fixed the Logic for Pause (charge ok)

## 0.0.3
- Fix an overwrite of BatteryControl on Startup
- Fix that control commands are not send on ReadIntervall

## 0.0.2
- Retain Configuration in MQTT and read them on startup

