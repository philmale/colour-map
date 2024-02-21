# colour-map
A custom template for Home Assistant that maps colour names to RGB, HEX, HSL, HSV, HS, and XYB.
I've put 1534 colour names in there, feel free to add or tweak, the colour names were picked to give something sensible when applied to a light bulb :)

# Install
Copy the colour_map.jinja file into your config/custom_templates directory in Home Assistant, then anywhere you want to use one or more of the macros in some Jinja template code use:

```
{%- from 'colour_map.jinja' import
  random_colour, name_to_tag, tag_to_name, lookup_colour, find_colour,
  hex2rgb, hex2hsv, hex2hsl, hex2hs, hex2xyb, closest_ha_colour -%}
```

# To play with
In Developers Tools->Template try pasting this:
```
{%- from 'colour_map.jinja' import random_colour, lookup_colour, close_colours -%}
{{ lookup_colour() }}
{{ lookup_colour("Red", "hex") }}
{{ lookup_colour("Sky Blue", "rgb") }}
{{ lookup_colour("SkyBlue", "rgb") }}
{{ random_colour() }}
{%- set aColour = lookup_colour() %}
{{ aColour }}
{{ lookup_colour(aColour, "hex") }}
{{ lookup_colour(aColour, "hsl") }}
{{ lookup_colour(aColour, "hs") }}
{{ lookup_colour(aColour, "rgb") }}
{{ close_colours(aColour,15).split(',') }}
{{ close_colours('Energy Yellow',15).split(',') | random }}

```

# Examples
In an entity-row in a lovelace dashboard (using custom:template-entity-row):

```
      - type: custom:template-entity-row
        entity: some.entity
        color: |-
          {%- from 'colour_map.jinja' import lookup_colour -%}
          {{'#'}}{{ lookup_colour('Green Yellow','hex') }}
          
```

... or in a script:

```
service: input_text.set_value
data:
  value: >-
    {%- from 'colour_map.jinja' import random_colour -%}
    {{ random_colour()|string }}
target:
  entity_id: input_text.some_text_helper
```

