# colour_map.jinja - Home Assistant Colour Management Library

A  Jinja2 macro library for Home Assistant that provides colour manipulation, conversion, lookup, and complementary colour generation for lighting automations.

## Features

- 🎨 **1,741 Named Colours** across 3 colour maps (Lamp, Formula 1, Home Assistant)
- 🔄 **Format Conversion** between Hex, RGB, HSV, HSL, HS, H, and XYB
- 🎯 **Colour Lookup** with case-insensitive search
- 🎲 **Random Colour Selection** with constraints
- 🌈 **Complementary Colour Generation** using 6 harmony algorithms
- 📊 **Colour Averaging** for multiple colours
- 🔍 **Nearest Colour Matching** from hex values 

## Installation

1. Copy `colour_map.jinja` to your Home Assistant `custom_templates` directory:
   ```
   /config/custom_templates/colour_map.jinja
   ```

2. If the directory doesn't exist, create it first.

3. Restart Home Assistant or reload Jinja2 templates using `homeassistant.reload_custom_templates` in `Developer Tools > Actions`.

## Quick Start

```jinja2
{%- from 'colour_map.jinja' import
  lookup_colour,
  hex_complementary,
  nearest_colour_name
-%}

{#- Look up a colour by name -#}
{{ lookup_colour("Ferrari Red", "hex", "f1") | from_json }}
{# Returns: "#ff2800" #}

{#- Generate complementary colours -#}
{{ hex_complementary("#ff0000", "triadic") | from_json }}
{# Returns: ["#ff0000", "#00ff00", "#0000ff"] #}

{#- Find nearest named colour -#}
{{ nearest_colour_name("#ff2800", "f1") | from_json }}
{# Returns: "Ferrari Red" #}
```

## Available Macros

### Random Colour Selection

#### `random_colour(map)`
Select a single random colour from a colour map.

**Parameters:**
- `map` - Colour map: `"lamp"` (default), `"f1"`, `"ha"`

**Returns:** JSON string with colour name

**Example:**
```jinja2
{{ random_colour("lamp") | from_json }}
```

#### `random_colours(n, map)`
Select multiple random colours without repeats.

**Parameters:**
- `n` - Number of colours (default: 1)
- `map` - Colour map: `"lamp"` (default), `"f1"`, `"ha"`

**Returns:** JSON array of colour names

**Example:**
```jinja2
{{ random_colours(5, "f1") | from_json }}
```

#### `sunset_colour(map)`
Select a random colour excluding red (hue > 350 or < 10) and white (saturation < 5).

**Parameters:**
- `map` - Colour map: `"lamp"` (default), `"f1"`, `"ha"`

**Returns:** JSON string with colour name

**Example:**
```jinja2
{{ sunset_colour("lamp") | from_json }}
```

### Colour Lookup

#### `lookup_colour(id, attr, map)`
Look up a colour and return it in the requested format.

**Parameters:**
- `id` - Colour name, `"random"`, or `"sunset_safe"`
- `attr` - Format: `"name"`, `"hex"`, `"rgb"`, `"hsv"`, `"hsl"`, `"hs"`, `"h"`, `"xyb"` (default: `"name"`)
- `map` - Colour map: `"lamp"` (default), `"f1"`, `"ha"`

**Returns:** JSON value in requested format

**Example:**
```jinja2
{{ lookup_colour("Red", "hex", "ha") | from_json }}
{# Returns: "#ff0000" #}
```

#### `lookup_colours(cs, attr, map)`
Look up multiple colours at once.

**Parameters:**
- `cs` - Comma-separated string, list of names, or None for random
- `attr` - Format: `"name"`, `"hex"`, `"rgb"`, etc. (default: `"name"`)
- `map` - Colour map: `"lamp"` (default), `"f1"`, `"ha"`

**Returns:** JSON array of values

**Example:**
```jinja2
{{ lookup_colours("Red, Green and Blue", "hex", "ha") | from_json }}
{# Returns: ["#ff0000", "#008000", "#0000ff"] #}
```

#### `find_colour(name, map)`
Find a colour by name with case-insensitive search.

**Parameters:**
- `name` - Colour name to search for
- `map` - Colour map: `"lamp"` (default), `"f1"`, `"ha"`

**Returns:** JSON string with matched colour name or null

**Example:**
```jinja2
{{ find_colour("ferrari red", "f1") | from_json }}
{# Returns: "Ferrari Red" #}
```

### Colour Conversion

#### `hex2rgb(hex)`
Convert hex colour to RGB.

**Parameters:**
- `hex` - Hex colour (with or without #)

**Returns:** JSON array `[r, g, b]` (0-255)

**Example:**
```jinja2
{{ hex2rgb("#ff0000") | from_json }}
{# Returns: [255, 0, 0] #}
```

#### `rgb2hex(rgb)`
Convert RGB to hex colour.

**Parameters:**
- `rgb` - Array `[r, g, b]` (0-255)

**Returns:** JSON string with hex colour (lowercase with #)

**Example:**
```jinja2
{{ rgb2hex([255, 0, 0]) | from_json }}
{# Returns: "#ff0000" #}
```

#### `hex2hsv(hex)`
Convert hex colour to HSV.

**Parameters:**
- `hex` - Hex colour (with or without #)

**Returns:** JSON array `[h, s, v]` (h: 0-360, s: 0-100, v: 0-100)

**Example:**
```jinja2
{{ hex2hsv("#ff0000") | from_json }}
{# Returns: [0, 100, 100] #}
```

#### `hsv2rgb(hsv)`
Convert HSV to RGB.

**Parameters:**
- `hsv` - Array `[h, s, v]` (h: 0-360, s: 0-100, v: 0-100)

**Returns:** JSON array `[r, g, b]` (0-255)

**Example:**
```jinja2
{{ hsv2rgb([0, 100, 100]) | from_json }}
{# Returns: [255, 0, 0] #}
```

#### `hsv2hex(hsv)`
Convert HSV to hex colour.

**Parameters:**
- `hsv` - Array `[h, s, v]` (h: 0-360, s: 0-100, v: 0-100)

**Returns:** JSON string with hex colour (lowercase with #)

**Example:**
```jinja2
{{ hsv2hex([0, 100, 100]) | from_json }}
{# Returns: "#ff0000" #}
```

#### `hex2hsl(hex)`
Convert hex colour to HSL.

**Parameters:**
- `hex` - Hex colour (with or without #)

**Returns:** JSON array `[h, s, l]` (h: 0-360, s: 0-100, l: 0-100)

#### `hex2hs(hex)`
Extract hue and saturation from hex colour.

**Returns:** JSON array `[h, s]`

#### `hex2h(hex)`
Extract hue from hex colour.

**Returns:** JSON array `[h]`

#### `hex2xyb(hex)`
Convert hex to CIE XY coordinates for Home Assistant.

**Returns:** JSON array `[x, y, brightness]`

### Complementary Colour Generation

#### `hex_complementary(hex, mode)`
Generate harmonious colour palette from a hex colour.

**Parameters:**
- `hex` - Input hex colour (with or without #)
- `mode` - Harmony algorithm (default: `"complementary"`)
  - `"complementary"` - Base + opposite (180°)
  - `"split_complementary"` - Base + flanking opposites (150°, 210°)
  - `"analogous"` - Adjacent hues (±30°)
  - `"triadic"` - Three evenly spaced (120° apart)
  - `"tetradic"` - Four rectangularly spaced (60°, 180°, 240°)
  - `"square"` - Four evenly spaced (90° apart)

**Returns:** JSON array of hex colours (including base)

**Example:**
```jinja2
{{ hex_complementary("#ff0000", "triadic") | from_json }}
{# Returns: ["#ff0000", "#00ff00", "#0000ff"] #}
```

#### `hex_complementary_named(hex, mode, map)`
Generate named complementary colours from hex input.

**Parameters:**
- `hex` - Input hex colour
- `mode` - Harmony algorithm (see above)
- `map` - Colour map for naming: `"lamp"` (default), `"f1"`, `"ha"`

**Returns:** JSON array of nearest colour names

**Example:**
```jinja2
{{ hex_complementary_named("#ff0000", "complementary", "ha") | from_json }}
{# Returns: ["Red", "Cyan"] #}
```

#### `complementary_colours(colour, mode, map)`
Generate complementary colours from a colour name.

**Parameters:**
- `colour` - Colour name to look up
- `mode` - Harmony algorithm (default: `"complementary"`)
- `map` - Colour map: `"lamp"` (default), `"f1"`, `"ha"`

**Returns:** JSON array of hex colours

**Example:**
```jinja2
{{ complementary_colours("Ferrari Red", "split_complementary", "f1") | from_json }}
```

#### `complementary_colours_named(colour, mode, map)`
Generate named complementary colours from a colour name.

**Parameters:**
- `colour` - Colour name to look up
- `mode` - Harmony algorithm (default: `"complementary"`)
- `map` - Colour map for both input and output: `"lamp"` (default), `"f1"`, `"ha"`

**Returns:** JSON array of colour names

**Example:**
```jinja2
{{ complementary_colours_named("Red", "triadic", "ha") | from_json }}
```

### Colour Analysis

#### `average_hex_colour(cs, map)`
Average multiple colours to return a single hex colour.

**Parameters:**
- `cs` - Comma-separated string or list of colour names
- `map` - Colour map: `"lamp"` (default), `"f1"`, `"ha"`

**Returns:** JSON string with averaged hex colour

**Example:**
```jinja2
{{ average_hex_colour("Red, Green, Blue", "ha") | from_json }}
{# Returns: "#aa5555" (average of RGB values) #}
```

#### `nearest_colour_name(hex, map)`
Find the nearest named colour for a given hex colour.

**Parameters:**
- `hex` - Input hex colour (with or without #, alpha channel ignored)
- `map` - Colour map: `"lamp"` (default), `"f1"`, `"ha"`

**Returns:** JSON string with closest colour name

**Example:**
```jinja2
{{ nearest_colour_name("#ff2800", "f1") | from_json }}
{# Returns: "Ferrari Red" #}
```

## Colour Maps

### `lamp` - General Lighting Colours
Comprehensive collection of named colours suitable for general lighting applications.

### `f1` - Formula 1 Team Colours
Official Formula 1 team colours and variations. Perfect for F1 fan lighting automations.

### `ha` - Home Assistant Standard Colours
Standard colour palette used throughout Home Assistant.

## Usage Examples

### Example 1: Random Sunset Lighting
```jinja2
{%- from 'colour_map.jinja' import sunset_colour, lookup_colour -%}

{%- set colour_name = sunset_colour("lamp") | from_json -%}
{%- set hs = lookup_colour(colour_name, "hs", "lamp") | from_json -%}

service: light.turn_on
target:
  entity_id: light.living_room
data:
  hs_color: {{ hs }}
  brightness: 200
```

### Example 2: Triadic Harmony Lighting Scene
```jinja2
{%- from 'colour_map.jinja' import hex_complementary, hex2hs -%}

{%- set base_colour = "#ff0000" -%}
{%- set palette = hex_complementary(base_colour, "triadic") | from_json -%}

{%- for colour_hex in palette -%}
  {%- set hs = hex2hs(colour_hex) | from_json -%}
  {#- Apply each colour to a different light -#}
{%- endfor -%}
```

### Example 3: Team Colour Automation
```jinja2
{%- from 'colour_map.jinja' import lookup_colour -%}

{%- set team = "Ferrari Red" -%}
{%- set hs = lookup_colour(team, "hs", "f1") | from_json -%}

service: light.turn_on
target:
  entity_id: light.garage
data:
  hs_color: {{ hs }}
  brightness: 255
```

### Example 4: Colour Cycling with Complementary Colours
```jinja2
{%- from 'colour_map.jinja' import random_colour, complementary_colours, lookup_colour -%}

{%- set base = random_colour("lamp") | from_json -%}
{%- set palette = complementary_colours(base, "analogous", "lamp") | from_json -%}

{#- Cycle through analogous colours every 30 seconds -#}
```

## Design Principles

### Hex Case Consistency
- **Storage:** All colour map values are lowercase (e.g., `"ff2800"`)
- **Output:** All hex outputs are lowercase with `#` prefix (e.g., `"#ff2800"`)
- **Input:** Accepts both uppercase and lowercase hex values

### Return Values
- **All macros return JSON-encoded strings**
- Always use `| from_json` to parse results
- This ensures consistent serialisation and prevents Jinja2 type issues

## Testing

A comprehensive test suite is included: `colours_test_suite.jinja`

Paste into Home Assistant `Developer Tools > Template` to run:
- **57 basic functionality tests**
- **47 integration workflow tests**
- Tests conversion accuracy, round-trip fidelity, harmony algorithms, and more

### Running Tests
```jinja2
{#- Copy contents of colours_test_suite.jinja -#}
{#- Paste into Developer Tools > Template -#}
{#- View results immediately -#}
```

Expected output: **104 tests passed** ✅

## Technical Details

### Colour Space Conversions
- **RGB → Hex:** Standard 8-bit per channel encoding
- **Hex → HSV:** Conversion preserves hue wrapping (0° = 360°)
- **HSV → RGB:** Uses standard HSV to RGB algorithm with sector calculation
- **Hex → XYB:** Converts to CIE 1931 colour space for Home Assistant lights

### Harmony Algorithms
Complementary colour generation uses HSV colour space:
- **Hue rotation** for complementary colours (preserves saturation and value)
- **Angle-based spacing** ensures mathematically precise harmony
- **Modulo wrapping** keeps hue values within 0-360° range

### Nearest Colour Matching
Uses RGB Euclidean distance:
```
distance = √[(r₁-r₂)² + (g₁-g₂)² + (b₁-b₂)²]
```
Returns the colour with minimum distance from input.

Thanks to @dabalroman!

## Performance Considerations

### Best Practices
- Cache colour lookups in variables when used multiple times
- Use `hex_*` macros directly when working with hex values
- Prefer `lookup_colours()` for batch operations over multiple `lookup_colour()` calls

## Version History

### v3.0 (Current)
- ✨ Added 6 complementary colour generation macros
- ✨ Added `hsv2rgb` and `hsv2hex` conversion helpers
- 🚀 Converted all hex values to lowercase for performance
- 🚀 Removed runtime `| lower` filters
- 📝 Updated all examples to show `| from_json`
- ✅ Comprehensive test suite with 104 tests

### v2.0
- Initial release with 15 core macros
- 1,741 colours across 3 maps
- Basic conversion and lookup functionality

## Contributing

Contributions welcome! Please:
1. Test changes with the included test suite
2. Maintain hex case consistency (lowercase storage, lowercase output)
3. Ensure all macros return JSON-encoded strings
4. Update README with new functionality
5. Add tests for new features

## License

MIT License - Feel free to use in your Home Assistant setup!

## Credits

Created for Home Assistant home automation lighting control.

**Tech Stack:** Home Assistant, Jinja2

## Support

- 🐛 **Issues:** Report bugs via GitHub Issues
- 💡 **Feature Requests:** Open a GitHub Issue with `[Feature]` tag
- 📖 **Documentation:** See inline macro documentation in `colour_map.jinja`

## Changelog

See [CHANGELOG.md](CHANGELOG.md) for detailed version history.

---

**Note:** This library requires Home Assistant with custom template support. Tested on Home Assistant 2026.1.2.
