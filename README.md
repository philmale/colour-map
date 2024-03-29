# colour-map
A custom template for Home Assistant that maps colour names to RGB, HEX, HSL, HSV, HS, and XYB.
I've put 1534 colour names in there, feel free to add or tweak, the colour names were picked to give something sensible when applied to a light bulb :)
I use this for selecting light colours, and to mix things up a little so it's not always the same boring light colour there are some ways to find simialar colours and randomly select colours.

# Install
Copy the colour_map.jinja file into your config/custom_templates directory in Home Assistant and restart, then anywhere you want to use one or more of the macros in a Jinja template code use:

```
{%- from 'colour_map.jinja' import
  random_colour, name_to_tag, tag_to_name, lookup_colour, find_colour,
  hex2rgb, hex2hsv, hex2hsl, hex2hs, hex2xyb, closest_ha_colour -%}
```

# To play with
In Developers Tools->Template try pasting this:
```
{%- from 'colour_map.jinja' import random_colour, lookup_colour, close_colours -%}
Random colour name: {{ lookup_colour() }}
Random colour: {{ random_colour() }}

HEX for Red: {{ lookup_colour("Red", "hex") }}
RGB for colour name Sky Blue: {{ lookup_colour("Sky Blue", "rgb") }}
RGB for colour SkyBlue: {{ lookup_colour("SkyBlue", "rgb") }}

{%- set aColour = lookup_colour() %}
Random colour name in variable aColour: {{ aColour }}

HEX for {{ aColour}}: {{ lookup_colour(aColour, "hex") }}
HSL for {{ aColour}}: {{ lookup_colour(aColour, "hsl") }}
HS for {{ aColour}}: {{ lookup_colour(aColour, "hs") }}
RGB for {{ aColour}}: {{ lookup_colour(aColour, "rgb") }}

Sorted closest 15 colours for {{ aColour}}: {{ close_colours(aColour,15).split(',') }}

Random colour close to {{ aColour}}: {{ close_colours(aColour,15).split(',') | random }}

Random colour name close to {{ aColour}}: {{ lookup_colour(close_colours(aColour,15).split(',') | random) }}
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

