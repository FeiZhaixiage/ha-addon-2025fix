# eWeLink Smart Home

2025 fix fixed dockerfile 

FROM node:14-slim
ENV CK_API_ENV=prod

RUN sed -i 's|deb.debian.org/debian|archive.debian.org/debian|g' /etc/apt/sources.list \
 && sed -i 's|security.debian.org/debian-security|archive.debian.org/debian-security|g' /etc/apt/sources.list \
 && printf 'Acquire::Check-Valid-Until "false";\n' > /etc/apt/apt.conf.d/99no-check-valid-until \
 && apt-get update && apt-get install -y python build-essential \
 && rm -rf /var/lib/apt/lists/*

---

## Troubleshooting

-   To solve `Failed to call service xxxxx/xxxxxxx. Service not found.` issue, use`File editor` to edit `configuration.yaml`. Append the following info to end of file:

```
switch:
  - platform: template
    switches:
      ewelink_virtual_switch:
        turn_on:
          service: switch.turn_on
        turn_off:
          service: switch.turn_off

cover:
  - platform: template
    covers:
      ewelink_virtual_cover:
        open_cover:
          service: cover.open_cover
        close_cover:
          service: cover.close_cover
        stop_cover:
          service: cover.stop_cover
        set_cover_position:
          service: cover.set_cover_position

fan:
  - platform: template
    fans:
      ewelink_virtual_fan:
        value_template: "{{ states('input_boolean.state') }}"
        turn_on:
          service: fan.turn_on
        turn_off:
          service: fan.turn_off
        set_preset_mode:
          service: fan.set_preset_mode

light:
  - platform: template
    lights:
      ewelink_virtual_light:
        turn_on:
          service: light.turn_on
        turn_off:
          service: light.turn_off
```
