# colour-map
A custom template for Home Assistant that maps colour names to RGB, HEX, HSL, HSV, HS, and XYB.
I've put 1534 colour names in there, feel free to add or tweak, the colour names were picked to give something sensible when applied to a light bulb :)
I use this for selecting light colours, and to mix things up a little so it's not always the same boring light colour there are some ways to find simialar colours and randomly select colours.

# Install
Copy the colour_map.jinja file into your config/custom_templates directory in Home Assistant and restart or call the 'homeassistant.reload_custom_templates' in the Developer Tools->Services tab, then anywhere you want to use one or more of the macros in Jinja code use (just list the specific macros you want to use, no need to list everything):

```
{%- from 'colour_map.jinja' import 
  random_colour, random_colours,
  name_to_tag, tag_to_name, textify_list,
  lookup_colour, lookup_colours, find_colour,
  hex2rgb, hex2hsv, hex2hsl, hex2hs, hex2xyb -%}
```

# To play with
In Developers Tools->Template try pasting this once you have installed:
```
{%- from 'colour_map.jinja' import random_colours, random_colour, lookup_colour, lookup_colours, textify_list -%}
Random colour name: {{ lookup_colour() }}
Random colour: {{ random_colour() }}
Random colours: {{ random_colours(3) }}
Random colour names: {{ lookup_colours(random_colours(3)) }}

HEX for Red: {{ lookup_colour("Red", "hex") }}

RGB for colour name Sky Blue: {{ lookup_colour("Sky Blue", "rgb") }}
HEX for colour SkyBlue: {{ lookup_colour("SkyBlue", "hex") }}
RGB for colour SkyBlue: {{ lookup_colour("SkyBlue", "rgb") }}
HSV for colour SkyBlue: {{ lookup_colour("SkyBlue", "hsv") }}
HS for colour SkyBlue: {{ lookup_colour("SkyBlue", "hs") }}

Random HS colour: {{ lookup_colour("random", "hs") }}
Random HS colour: {{ lookup_colour(random_colour(), "hs") }}
Random HEX colour: {{ lookup_colour("random", "hex") }}
Random HEX colour: {{ lookup_colour(random_colour(), "hex") }}

{%- set aColour = lookup_colour() %}

Random colour name in variable aColour: {{ aColour }}

HEX for {{ aColour}}: {{ lookup_colour(aColour, "hex") }}
HSL for {{ aColour}}: {{ lookup_colour(aColour, "hsl") }}
HS for {{ aColour}}: {{ lookup_colour(aColour, "hs") }}
RGB for {{ aColour}}: {{ lookup_colour(aColour, "rgb") }}

String of 3 random colours: {{ random_colours(3) }}
List of 3 random colours: {{ random_colours(3).split(',') }}
String of 3 random colour names: {{ lookup_colours(random_colours(3)) }}
String of 3 random HEX codes: {{ lookup_colours(random_colours(3), "hex") }}

Using the Formula1 colour map...
String of 3 random F1 colours: {{ random_colours(3, "f1") }}
String of 3 random F1 colour names: {{ lookup_colours(random_colours(3, "f1")) }}
String of 3 random F1 colour names, formatted: {{ textify_list( lookup_colours(random_colours(3, "f1")) ) }}
List of 3 random F1 colours: {{ random_colours(3, "f1").split(',') }}
List of 3 random F1 colour names: {{ lookup_colours(random_colours(3, "f1"), "names", "f1").split(',') }}
List of 3 random F1 HEX codes: {{ lookup_colours(random_colours(3, "f1"), "hex", "f1").split(',') }}
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

... or turning a light on splitting the HS value into a list (remember all macros return strings):
```
service: light.turn_on
data:
  transition: 5
  hs_color: >-
    {%- from 'colour_map.jinja' import lookup_colour -%}
    {{ lookup_colour(random_colour(), "hs").split(',') }}
  brightness_pct: 100
target:
  entity_id: light.my_light
```
