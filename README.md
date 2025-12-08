<div align="center">

<img src="brands/icon.png" width="128" height="128" alt="sleepd icon">

# sleepd

**real-time sleep state tracking for home assistant**

[![HACS Custom](https://img.shields.io/badge/HACS-Custom-00ffff?style=for-the-badge&logo=homeassistant)](https://hacs.xyz/docs/faq/custom_repositories)
[![License](https://img.shields.io/badge/license-MIT-ff00ff?style=for-the-badge)](LICENSE)

*processes sleep mqtt messages from android apps and exposes states, events, and controls to home assistant - automate your smart home based on whether you're awake or asleep.*

---

</div>

## ✨ features

- **wake status sensor** — `awake` or `asleep` based on configurable thresholds
- **sleep stage tracking** — light, deep, rem, awake
- **alarm controls** — dismiss, snooze, skip via HA buttons
- **sound detection** — snore, talk, cough events
- **lullaby control** — playback status and stop button
- **disturbance alerts** — apnea and anti-snoring events

## 📱 compatible apps

- [sleep as android](https://sleep.urbandroid.org/) (primary)
- any app that publishes sleep events via mqtt

## 📊 sensors

| sensor | description |
|--------|-------------|
| wake status | `awake` or `asleep` based on configurable states and durations |
| sleep stage | current stage (light, deep, rem, awake) |
| sleep tracking | whether tracking is active or paused |
| alarm event | alarm events like snooze, dismiss, skip |
| sound | snore, talk, cough, and other detected sounds |
| disturbance | apnea and anti-snoring events |
| lullaby | lullaby playback status |
| state | raw event from last mqtt message |

## 🎮 buttons

*requires home assistant companion app*

- alarm dismiss / snooze
- lullaby stop
- sleep tracking start / stop / pause / resume
- sleep tracking start with smart alarm

## 📥 installation

### option 1: hacs (recommended)

1. add this repo to HACS custom repositories:
   ```
   https://github.com/sudoxnym/sleepd
   ```

2. search for **sleepd** in HACS and install

3. restart home assistant

4. add integration via UI:

   [![Add Integration](https://my.home-assistant.io/badges/config_flow_start.svg)](https://my.home-assistant.io/redirect/config_flow_start/?domain=saas)

### option 2: manual

copy `custom_components/saas` to your HA `custom_components` directory

## ⚙️ configuration

| option | description |
|--------|-------------|
| name | identifier for this user/device |
| topic | mqtt topic to subscribe to |
| awake duration | seconds of awake events before marking as awake |
| sleep duration | seconds of sleep events before marking as asleep |
| awake states | which states indicate being awake |
| sleep states | which states indicate being asleep |
| mobile app | companion app target for buttons (optional) |

## 📡 mqtt setup

sleepd expects json messages with an `event` field:

```json
{"event": "sleep_tracking_started"}
{"event": "sleep_tracking_stopped"}
{"event": "alarm_alert_start", "value1": "1733580000000"}
{"event": "light_sleep"}
{"event": "deep_sleep"}
{"event": "rem"}
```

### sleep as android config

1. open sleep as android → settings → services → automation → MQTT

2. connection string:
   ```
   tcp://user:pass@your-ha-ip:1883
   ```

3. set topic to match your sleepd config

4. enable automatic tracking

## ⌚ tested wearables

- xiaomi mi band 7 (via notify for mi band)
- garmin fenix 7x (via garmin alternative)
- amazfit gtr3 pro

<details>
<summary>mi band 7 auth key extraction</summary>

```bash
adb shell
grep -E "authKey=[a-z0-9]*," /sdcard/Android/data/com.xiaomi.wearable/files/log/XiaomiFit.device.log | \
awk -F ", " '{print $17}' | grep authKey | tail -1 | awk -F "=" '{print $2}'
```

</details>

## 📜 license

MIT — do whatever you want with it

---

<div align="center">

made by [sudoxnym](https://sudoxreboot.com) ⚡

*formerly known as saas*

</div>
