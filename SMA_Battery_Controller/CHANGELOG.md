# Changelog
**Warning:** This is not an official add-on and is not affiliated with SMA. Use at your own risk. This software is experimental.

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

