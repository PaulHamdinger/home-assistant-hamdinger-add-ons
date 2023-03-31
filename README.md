# home-assistant-hamdinger-add-ons

## Installation

1. Add this repository to Home Assistant.
2. After some time new addons will appear in list of available addons.
3. Install needed add-on.
4. Start installed add-on.
5. Check logs and status of installed add-on to see if everything went well.

## Watchdog Dev

This add-on uses the Raspberry Pi's `/dev/watchdog` hardware watchdog timer. It will restart the server on no response.
For details about watchdog see https://www.kernel.org/doc/Documentation/watchdog/watchdog-api.txt.

Example log...

  ```
  2023-03-31 16:42:42 INFO     Opening watchdog device
  2023-03-31 16:42:42 INFO     Watchdog identity: Broadcom BCM2835 Watchdog timer
  2023-03-31 16:42:42 INFO     Watchdog firmware version: 0
  2023-03-31 16:42:42 INFO     Watchdog options: 33152
  2023-03-31 16:42:42 INFO     Watchdog timeout: 15
  2023-03-31 16:42:42 INFO     Starting main cycle with sleep time 5 sec...
  ```

Service sends keepalive to watchdog timer every 5 seconds, on hang or other software problems system will do hardware restart in 15 seconds.

To get notifications on HA start/stop you can add this automations:

```
- id: "Hass Startup Notification"
  alias: 'Hass Startup Notification'
  trigger:
    - platform: homeassistant
      event: start
  action:
    service: notify.telegram_notify
    data:
      title: "Warning"
      message: "Hass restarted"

- id: "Hass Shutdown Notification"
  alias: 'Hass Shutdown Notification'
  trigger:
    - platform: homeassistant
      event: shutdown
  action:
    service: notify.telegram_notify
    data:
      title: "Warning"
      message: "Hass shutdown"
```

If you received only "restarted" notification - looks like HA restarted uncorrectly.

