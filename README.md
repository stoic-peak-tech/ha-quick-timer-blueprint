# Quick Timer blueprint for Home Assistant

A SmartThings-style one-off auto-off timer for a light, fan or switch, as a Home Assistant automation blueprint.

- Turn the device on and a **default timer** starts. When it ends, the device turns off.
- Pick **10 / 30 / 60 minutes or 2 hours** from a dropdown for a one-time length. The device turns on if it was off.
- Pick **Stay on** to cancel the timer.
- Need another length? Start the timer from its own popup (hold the tile, enter hh:mm:ss, tap **Set**). The device turns on if it was off.
- Turning the device off any way (wall switch, voice, app) cancels the timer.

One blueprint, one automation per device. Works with lights, fans and switches.

## Install

Copy `blueprints/automation/stoicpeak/quick_timer.yaml` to `<config>/blueprints/automation/stoicpeak/quick_timer.yaml`, or import it from its raw URL in Settings, Automations & scenes, Blueprints, Import Blueprint.

## Set up a device

A blueprint cannot create helpers, so create these three first (Settings, Devices & services, Helpers):

| Helper | Settings |
| --- | --- |
| **Timer** | Name it for the device (for example "Laundry Light Auto-Off"). Turn **Restore** on. Duration does not matter. |
| **Number** | Name it "<Device> Default Minutes". Min 1, max 240, step 1, display mode box. Set it to your default (for example 10). |
| **Dropdown** | Name it "<Device> Auto-Off Menu". Options, in this order and spelled exactly: `Choose timer`, `10 minutes`, `30 minutes`, `60 minutes`, `2 hours`, `Stay on`. |

Then create an automation from the **Quick Timer** blueprint and pick the device and the three helpers.

## Dashboard tile

One tile, always the same size. Tap opens the dropdown, hold opens the timer popup for a custom time. Use your own entity names:

```yaml
type: tile
entity: timer.laundry_light_auto_off
name: Laundry timer
icon: mdi:timer-outline
grid_options:
  columns: 6
tap_action:
  action: more-info
  entity: input_select.laundry_light_auto_off_menu
hold_action:
  action: more-info
```

## Behavior

| You do | Result |
| --- | --- |
| Turn the device on, pick nothing | Default timer starts; device turns off when it ends |
| Pick 10 / 30 / 60 minutes or 2 hours | Timer starts at that length; device turns on if it was off |
| Pick Stay on | Timer is cancelled; device stays on until you turn it off |
| Hold the tile, enter a time, tap Set | Timer starts at that length; device turns on if it was off |
| Turn the device off | Timer is cancelled |

Picking a time does not change the default. Edit the Number helper to change it. The dropdown returns to "Choose timer" after each pick.

## Notes

- Tested on Home Assistant 2026.x with a Z-Wave dimmer. Restart survival of a running timer has not been tested; the timer helper has Restore on.
- The dropdown labels are matched by the blueprint, so they must be spelled exactly as above.

