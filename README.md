# Shelly RGBW2
This repository is to accompany my Shelly RGBW2 YouTube video

[![IMAGE ALT TEXT HERE](https://img.youtube.com/vi/B8DQntdtD4E/0.jpg)](https://www.youtube.com/watch?v=B8DQntdtD4E)

The RGBW2 is a full featured LED controller that supports smooth dimming of 4 channels and 16 million colors of RGB.

## !BREAKING CHANGE!
With version 2022.9 the `white_value` is no longer an available property when you define the MQTT light entity based on the template schema. 

## Home Assistant YAML for setting up RGBW2 to control RGBW LEDS (a single strip) - For version 2022.9 and higher
```yaml
mqtt:
  light:
    - name: "ledstrip"
      qos: 2
      rgbw_command_topic: "shellies/shellyrgbw2-<deviceId>/color/0/set"
      rgbw_state_topic: "shellies/shellyrgbw2-<deviceId>/color/0/status"
      rgbw_command_template: >
        {"red": {{ red }}, "green": {{ green }}, "blue": {{ blue }}, "white": {{ white }} }
      rgbw_value_template: > 
        {{value_json.red|int}},{{value_json.green|int}},{{value_json.blue|int}},{{value_json.white|int}}
      brightness_scale: 100
      brightness_command_topic: "shellies/shellyrgbw2-<deviceId>/color/0/set"
      brightness_state_topic: "shellies/shellyrgbw2-<deviceId>/color/0/status"
      brightness_command_template: >
        {%- if value == 1 -%}
          { "gain": 0 }
        {%- else -%}
          { "gain": {{ value }} }
        {%- endif -%}
      brightness_value_template: "{{ value_json.gain }}"
      command_topic: "shellies/shellyrgbw2-<deviceId>/color/0/command"
      state_topic: "shellies/shellyrgbw2-<deviceId>/color/0/status"
      payload_on: "on"
      payload_off: "off"
      state_value_template: >
        {%- if value_json.ison -%}
          on
        {%- else -%}
          off
        {%- endif -%}
      effect_list:
        - 0
        - 1
        - 2
        - 3
        - 4
        - 5
        - 6
      effect_command_topic: "shellies/shellyrgbw2-<deviceId>/color/0/set"
      effect_state_topic: "shellies/shellyrgbw2-<deviceId>/color/0/status"
      effect_command_template: >
        {"effect": {{ value }} }
      effect_value_template: "{{ value_json.effect }}"
```

Notes:
- You need to change the `<deviceId>` accordingly
- The `brightness_command_template` is defined in a way that if `brightness' equals to 1 that is actually 0. It is this way to be able to change gain to 0 without turning the light off (In case you only want white and no RGB). 

## Home Assistant YAML for setting up RGBW2 to control RGBW LEDS (a single strip) - For version 2022.8 and lower

```yaml
light:
  - platform: mqtt
    schema: template
    name: "LEDstrip MQTT"
    command_topic: "shellies/shellyrgbw2-2B9022/color/0/set"
    state_topic: "shellies/shellyrgbw2-2B9022/color/0/status"
    effect_list:
      - 0
      - 1
      - 2
      - 3
      - 4
      - 5
      - 6
    command_on_template: >
      {"turn": "on"
      {%- if brightness is defined -%}
      , "gain": {{brightness | float | multiply(0.3922) | round(0)}}
      {%- endif -%}
      {%- if red is defined and green is defined and blue is defined -%}
      , "red": {{ red }}, "green": {{ green }}, "blue": {{ blue }}
      {%- endif -%}
      {%- if white_value is defined -%}
      , "white": {{ white_value }}
      {%- endif -%}
      {%- if effect is defined -%}
      , "effect": {{ effect }}
      {%- endif -%}
      }
    command_off_template: '{"turn":"off"}'
    state_template: "{% if value_json.ison %}on{% else %}off{% endif %}"
    brightness_template: "{{ value_json.gain | float | multiply(2.55) | round(0) }}"
    red_template: '{{ value_json.red }}'
    green_template: '{{ value_json.green }}'
    blue_template: '{{ value_json.blue }}'
    white_value_template: '{{ value_json.white }}'
    effect_template: '{{ value_json.effect }}'
    qos: 2
  ```
## Home Assistant to control 4 different channels of white LEDs (strips or pot lights) - For version 2022.9 and higher
The suggestion is to use one of the following integrations:
- [Official Shelly for HA](https://www.home-assistant.io/integrations/shelly/)
- [ShellyForHASS](https://github.com/StyraHem/ShellyForHASS)

## Home Assistant YAML for setting up RGBW2 to control 4 different channels of white LEDs (strips or pot lights) - For version 2022.8 and lower

```yaml
light:
  - platform: mqtt
    schema: template
    name: "RGBW2 Channel 1"
    command_topic: "shellies/shellyrgbw2-2B9022/white/0/set"
    state_topic: "shellies/shellyrgbw2-2B9022/white/0/status"
    command_on_template: >
      {"turn": "on"
      {%- if brightness is defined -%}
      , "brightness": {{brightness | float | multiply(0.3922) | round(0)}}
      {%- endif -%}
      {%- if red is defined and green is defined and blue is defined -%}
      , "red": {{ red }}, "green": {{ green }}, "blue": {{ blue }}
      {%- endif -%}
      {%- if white_value is defined -%}
      , "white": {{ white_value }}
      {%- endif -%}
      {%- if effect is defined -%}
      , "effect": {{ effect }}
      {%- endif -%}
      }
    command_off_template: '{"turn":"off"}'
    state_template: "{% if value_json.ison %}on{% else %}off{% endif %}"
    brightness_template: "{{ value_json.brightness | float | multiply(2.55) | round(0) }}"
    qos: 2
  - platform: mqtt
    schema: template
    name: "RGBW2 Channel 2"
    command_topic: "shellies/shellyrgbw2-2B9022/white/1/set"
    state_topic: "shellies/shellyrgbw2-2B9022/white/1/status"
    command_on_template: >
      {"turn": "on"
      {%- if brightness is defined -%}
      , "brightness": {{brightness | float | multiply(0.3922) | round(0)}}
      {%- endif -%}
      {%- if red is defined and green is defined and blue is defined -%}
      , "red": {{ red }}, "green": {{ green }}, "blue": {{ blue }}
      {%- endif -%}
      {%- if white_value is defined -%}
      , "white": {{ white_value }}
      {%- endif -%}
      {%- if effect is defined -%}
      , "effect": {{ effect }}
      {%- endif -%}
      }
    command_off_template: '{"turn":"off"}'
    state_template: "{% if value_json.ison %}on{% else %}off{% endif %}"
    brightness_template: "{{ value_json.brightness | float | multiply(2.55) | round(0) }}"
    qos: 2
  - platform: mqtt
    schema: template
    name: "RGBW2 Channel 3"
    command_topic: "shellies/shellyrgbw2-2B9022/white/2/set"
    state_topic: "shellies/shellyrgbw2-2B9022/white/2/status"
    command_on_template: >
      {"turn": "on"
      {%- if brightness is defined -%}
      , "brightness": {{brightness | float | multiply(0.3922) | round(0)}}
      {%- endif -%}
      {%- if red is defined and green is defined and blue is defined -%}
      , "red": {{ red }}, "green": {{ green }}, "blue": {{ blue }}
      {%- endif -%}
      {%- if white_value is defined -%}
      , "white": {{ white_value }}
      {%- endif -%}
      {%- if effect is defined -%}
      , "effect": {{ effect }}
      {%- endif -%}
      }
    command_off_template: '{"turn":"off"}'
    state_template: "{% if value_json.ison %}on{% else %}off{% endif %}"
    brightness_template: "{{ value_json.brightness | float | multiply(2.55) | round(0) }}"
    qos: 2
  - platform: mqtt
    schema: template
    name: "RGBW2 Channel 4"
    command_topic: "shellies/shellyrgbw2-2B9022/white/3/set"
    state_topic: "shellies/shellyrgbw2-2B9022/white/3/status"
    command_on_template: >
      {"turn": "on"
      {%- if brightness is defined -%}
      , "brightness": {{brightness | float | multiply(0.3922) | round(0)}}
      {%- endif -%}
      {%- if red is defined and green is defined and blue is defined -%}
      , "red": {{ red }}, "green": {{ green }}, "blue": {{ blue }}
      {%- endif -%}
      {%- if white_value is defined -%}
      , "white": {{ white_value }}
      {%- endif -%}
      {%- if effect is defined -%}
      , "effect": {{ effect }}
      {%- endif -%}
      }
    command_off_template: '{"turn":"off"}'
    state_template: "{% if value_json.ison %}on{% else %}off{% endif %}"
    brightness_template: "{{ value_json.brightness | float | multiply(2.55) | round(0) }}"
    qos: 2
  - platform: group
    name: "All RGBW2 Lights"
    entities:
      - light.RGBW2_Channel_4
      - light.RGBW2_Channel_3
      - light.RGBW2_Channel_2
      - light.RGBW2_Channel_1
  ```
