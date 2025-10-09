# colour-map
A custom template for Home Assistant that maps colour names to RGB, HEX, HSL, HSV, HS, and XYB.
I've put 1534 colour names in there, feel free to add or tweak, the colour names were picked to give something sensible when applied to a light bulb :)
I use this for selecting light colours, and to mix things up a little so it's not always the same boring light colours.

The macros used to return strings, I've updated everything to return JSON formatted strings for those macros that return a data structure of some description.

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
{%- from 'colour_map.jinja' import random_colours, random_colour, lookup_colour, lookup_colours -%}
Random colour name: {{ lookup_colour() | from_json }}
Random colour: {{ random_colour() | from_json }}
List of random colours: {{ random_colours(3) | from_json }}
List of random colour names: {{ lookup_colours(random_colours(3)) | from_json }}

HEX for Red: {{ lookup_colour("Red", "hex") | from_json }}

RGB for colour name Sky Blue: {{ lookup_colour("Sky Blue", "rgb") | from_json }}
HEX for colour SkyBlue: {{ lookup_colour("SkyBlue", "hex") | from_json }}
RGB for colour SkyBlue: {{ lookup_colour("SkyBlue", "rgb") | from_json }}
HSV for colour SkyBlue: {{ lookup_colour("SkyBlue", "hsv") | from_json }}
HS for colour SkyBlue: {{ lookup_colour("SkyBlue", "hs") | from_json }}

Random HS colour: {{ lookup_colour("random", "hs") | from_json }}
Random HS colour: {{ lookup_colour(random_colour() | from_json, "hs") | from_json }}
Random HEX colour: {{ lookup_colour("random", "hex") | from_json }}
Random HEX colour: {{ lookup_colour(random_colour() | from_json, "hex") | from_json }}

{%- set aColour = lookup_colour() | from_json %}

Random colour name in variable aColour: {{ aColour }}

HEX for {{ aColour }}: {{ lookup_colour(aColour, "hex") | from_json }}
HSL for {{ aColour }}: {{ lookup_colour(aColour, "hsl") | from_json }}
HS for {{ aColour }}: {{ lookup_colour(aColour, "hs") | from_json }}
RGB for {{ aColour }}: {{ lookup_colour(aColour, "rgb") | from_json }}

List of 3 random colours: {{ random_colours(3) | from_json }}
List of 3 random colour names: {{ lookup_colours(random_colours(3) | from_json) | from_json }}
List of 3 random HEX codes: {{ lookup_colours(random_colours(3) | from_json, "hex") | from_json }}

Using the Formula1 colour map...
List of 3 random F1 colours: {{ random_colours(3, "f1") | from_json }}
List of 3 random F1 colour names: {{ lookup_colours(random_colours(3, "f1") | from_json, "name", "f1") | from_json }}
List of 3 random F1 HEX codes: {{ lookup_colours(random_colours(3, "f1") | from_json , "hex", "f1") | from_json  }}
```
You should see the output looking like this:
```
Random colour name: Black
Random colour: Driftwood
List of random colours: ['Cabaret', 'Sorbus', 'Gothic']
List of random colour names: ['Valor Green', 'Castro', 'Brown Tumbleweed']

HEX for Red: #ff0000

RGB for colour name Sky Blue: [135, 206, 235]
HEX for colour SkyBlue: #87ceeb
RGB for colour SkyBlue: [135, 206, 235]
HSV for colour SkyBlue: [197.4, 42.553, 92.157]
HS for colour SkyBlue: [197.4, 42.553]

Random HS colour: [60.504, 49.791]
Random HS colour: [48.96, 82.237]
Random HEX colour: #2e3f62
Random HEX colour: #6cdae7

Random colour name in variable aColour: Rodeo Dust

HEX for Rodeo Dust: #c9b29b
HSL for Rodeo Dust: [30.0, 29.87, 69.804]
HS for Rodeo Dust: [30.0, 22.886]
RGB for Rodeo Dust: [201, 178, 155]

List of 3 random colours: ['RoyalPurple', 'ParisDaisy', 'Yuma']
List of 3 random colour names: ['Varden', 'Moccaccino', 'Chablis']
List of 3 random HEX codes: ['#ba7f03', '#860111', '#db9690']

Using the Formula1 colour map...
List of 3 random F1 colours: ['FerrariRed', 'ScuderiaRed', 'KickSauberGreen']
List of 3 random F1 colour names: ['British Racing Green', 'Williams Blue', 'Azzurro La Plata']
List of 3 random F1 HEX codes: ['#001344', '#1085fe', '#27f4d2']
```


# Examples
In an entity-row in a lovelace dashboard (using custom:template-entity-row):

```
      - type: custom:template-entity-row
        entity: some.entity
        color: |-
          {%- from 'colour_map.jinja' import lookup_colour -%}
          {{ lookup_colour('Green Yellow','hex') | from_json }}
          
```

... or in a script:

```
service: input_text.set_value
data:
  value: >-
    {%- from 'colour_map.jinja' import random_colour -%}
    {{ random_colour() | from_json }}
target:
  entity_id: input_text.some_text_helper
```

... or turning a light on flipping the HS value into a list:
```
service: light.turn_on
data:
  transition: 5
  hs_color: >-
    {%- from 'colour_map.jinja' import lookup_colour -%}
    {{ lookup_colour(attr="hs") | from_json }}
  brightness_pct: 100
target:
  entity_id: light.my_light
```
