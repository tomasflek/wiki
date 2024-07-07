# Layout cheat sheet

## Custom buttons

### Simple custom button
![image](https://github.com/tomasflek/wiki/assets/29582051/3dc6b213-cc65-421c-8b4d-5682aa42f579)

``` Jinja
type: custom:button-card
entity: switch.bathroom_switch_left
name: Koupelna
styles:
  card:
    - width: 100px
    - height: 100px
```

## Change color based on state

``` Jinja
icon_color: |-
  {% if is_state('input_boolean.cover_schedule', 'on') %}
    blue
  {% else %}
    grey
  {% endif %}
```

## Evaluate code and use the value

### Example 1
``` Jinja
name: |- 
  {{1 + 1}}
```

### Example 2

``` jinja
content: |-
    {% set total = 0 %}
    {% if is_state('light.sonoff_element_lights', 'on') %}
        {% set total = total + 1 %}
    {% endif %}
    {% if is_state('light.living_room', 'on') %}
        {% set total = total + 1 %}
    {% endif %}
    {% if is_state('light.wall_light_1', 'on') %}
        {% set total = total + 1 %}
    {% endif %}
    {% if is_state('light.wall_light_2', 'on') %}
        {% set total = total + 1 %}
    {% endif %}
    {% if is_state('light.wall_light_3', 'on') %}
        {% set total = total + 1 %}
    {% endif %}
    ({{total}}) Lights ON
```

## Sun

![Dawn and dusk](pics/sun.png)


``` yaml
type: custom:mushroom-chips-card
chips:
  - type: entity
    entity: sun.sun
  - type: template
    content: >-
      Východ  {% if states.sun.sun %} {{
      (as_timestamp(states.sun.sun.attributes.next_rising)) |
      timestamp_custom(('%H:%M') )}} {% endif %}
    icon: mdi:weather-sunset-up
  - type: template
    content: >-
      Západ  {% if states.sun.sun %} {{
      (as_timestamp(states.sun.sun.attributes.next_setting)) |
      timestamp_custom(('%H:%M') )}} {% endif %}
    icon: mdi:weather-sunset-down
alignment: center

```
