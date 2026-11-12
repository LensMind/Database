# LensMind Aspheric Lens Database

This repository contains 500 automatically designed aspheric lenses: ten designs for each of 50 specifications derived from published optical patents. The prescriptions were generated using the automated design workflow based on sag-compensated structural perturbation (SCSP) described by Yao Gao, Weijian Hu, and Kaiwei Wang in *Sag-compensated Structural Perturbation for Local-Minimum Escape in Aspheric Lens Design*. The JSON files contain the resulting designs, not copies of the patent prescriptions.

## File naming

All 500 JSON files are directly under [`lenses/`](lenses/), with no subdirectories. Each file is named:

```text
<half_field_angle_deg>_<f_number>_<rank>_<mean_rms_um>.json
```

| Component | Meaning |
| --- | --- |
| `half_field_angle_deg` | Object-space half-field angle in degrees. The full field of view is twice this angle. |
| `f_number` | F-number of the design specification (dimensionless). |
| `rank` | Rank from 1 to 10 within the specification, ordered by increasing unrounded field-averaged RMS spot radius. |
| `mean_rms_um` | Field-averaged RMS spot radius in micrometers (µm), rounded to two decimal places. |

For example, [`10.045_2.602_1_2.02.json`](lenses/10.045_2.602_1_2.02.json) is the first-ranked design for a half-field angle of 10.045° and an F-number of 2.602. Its field-averaged RMS spot radius is 2.02 µm at the precision used in the filename.

## Reading the data

The `system_data` and `surface_parameters` objects contain the system settings and optical prescription. The `spot_diagram` object contains the saved RMS spot radii, in millimeters (mm), for individual sampled fields. Depending on the export format, these radii are stored either as `spot_diagram.fields.*.rms_radius_mm` or in the `spot_diagram.rms_radius` array.

The value in the filename is the arithmetic mean of the per-field polychromatic RMS spot radii, multiplied by 1,000 to convert from mm to µm. It is not an RMS calculated by pooling rays across fields. Use the values inside the JSON file if you need more precision than the filename provides.

Run the following example from the `aspheric/` directory. It uses only the Python standard library and handles both export formats.

```python
import json
from pathlib import Path
from statistics import mean


def mean_rms_um(lens):
    spot = lens["spot_diagram"]
    fields = spot.get("fields")
    if fields:
        radii_mm = [field["rms_radius_mm"] for field in fields.values()]
    else:
        radii_mm = spot["rms_radius"]
    return 1_000 * mean(radii_mm)


path = Path("lenses/10.045_2.602_1_2.02.json")
with path.open(encoding="utf-8") as stream:
    lens = json.load(stream)

print(f"{mean_rms_um(lens):.2f} µm")  # 2.02 µm
```

**Parser compatibility:** The original export uses the unquoted literal `Infinity` for the Abbe number (`v`) in surface records with no material assigned. Python's default `json.load()` accepts this value as positive infinity. Strict JSON parsers, including JavaScript's `JSON.parse()`, reject it.
