# Vxillum Static API

A free, read-only, static API designed to access almost every common flag with flexible dimensions, with no authentication, no API key, and limited rate-limiting. Every endpoint is a plain `GET` for a static file.

**Base URL:** `https://vxillum.com/api`

---

## Quick start

```bash
# List every registered country and its code
curl https://vxillum.com/api/en/index.json

# Get a flag as SVG
curl https://vxillum.com/api/svg/us.svg

# Get a flag as a 320px-wide PNG
curl -o us.png https://vxillum.com/api/w320/us.png
```

---

## Endpoints

| Endpoint | Description |
|---|---|
| `/en/index.json` | Master list of all registered countries and their codes |
| `/en/<category>.json` | Entries grouped by category (see below) |
| `/svg/<code>.svg` | Vector image |
| `/w<width>/<code>.png` | Raster PNG at a fixed width (see below) |

`<code>` is the country code as listed in `index.json`.

---

## Data (JSON)

All JSON files for english country codes live under `/en/` (English).
It is the only language currently supported. For more information, please view [FAQ](FAQ.md)

### Master index

```
GET /api/en/index.json
```

Returns every registered country along with its country code. Use this as the source of truth for valid codes.

### Category files

Each file contains the subset of entries belonging to that category.

| File | Contents |
|---|---|
| `/en/africa.json` | Africa |
| `/en/asia.json` | Asia |
| `/en/europe.json` | Europe |
| `/en/north_america.json` | North America |
| `/en/south_america.json` | South America |
| `/en/oceania.json` | Oceania |
| `/en/us_states.json` | US States |
| `/en/territories.json` | Territories |
| `/en/international_organisations.json` | International Organisations |
| `/en/pride.json` | Pride Flags |
| `/en/alternatives.json` | Alternative Spelling |

### Example response

> The shape below is illustrative. Check the live files for the exact field names.

```json
[
  { "code": "us", "name": "United States" },
  { "code": "sg", "name": "Singapore" }
]
```

---

## Images

### SVG

```
GET /api/svg/<code>.svg
```

Resolution-independent vector, best for web use where the image needs to scale.

```
https://vxillum.com/api/svg/sg.svg
```

### PNG

```
GET /api/w<width>/<code>.png
```

Pre-rendered PNG at a fixed width (The height follows the flag's aspect ratio).

| Folder | Width |
|---|---|
| `/w20/` | 20 px |
| `/w40/` | 40 px |
| `/w80/` | 80 px |
| `/w160/` | 160 px |
| `/w320/` | 320 px |
| `/w640/` | 640 px |
| `/w1280/` | 1280 px |
| `/w2560/` | 2560 px |

Only these widths exist, requesting any other width returns `404`. For more information, please view [FAQ](FAQ.md)

```
https://vxillum.com/api/w640/sg.png
```

---

## Usage examples

### HTML

```html
<img src="https://vxillum.com/api/svg/sg.svg" alt="Singapore flag" width="64">

<!-- Responsive PNG -->
<img
  src="https://vxillum.com/api/w160/sg.png"
  srcset="https://vxillum.com/api/w320/sg.png 2x"
  alt="Singapore flag">
```

### JavaScript

```js
const res = await fetch("https://vxillum.com/api/en/index.json");
const countries = await res.json();
console.log(countries);
```

### Python

```python
import requests

countries = requests.get("https://vxillum.com/api/en/index.json").json()

png = requests.get("https://vxillum.com/api/w320/sg.png")
with open("sg.png", "wb") as f:
    f.write(png.content)
```

---

## Errors

Because this is a static host, errors are standard HTTP responses.

| Status | Meaning |
|---|---|
| `200` | OK |
| `404` | Unknown code, unsupported width, or wrong path |

---