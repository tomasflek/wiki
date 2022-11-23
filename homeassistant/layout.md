# Layout cheat sheet

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