# Shelly RGBW2
This repository is to accompany my Shelly RGBW2 YouTube video

[![IMAGE ALT TEXT HERE](https://img.youtube.com/vi/B8DQntdtD4E/0.jpg)](https://www.youtube.com/watch?v=B8DQntdtD4E)

The RGBW2 is a full featured LED controller that supports smooth dimming of 4 channels and 16 million colors of RGB.

## Home Assistant YAML for setting up RGBW2 to control RGBW LEDS (a single strip) 

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
## Home Assistant YAML for setting up RGBW2 to control 4 different channels of white LEDs (strips or pot lights)

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
