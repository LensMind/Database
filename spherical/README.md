# LensMind Spherical Lens Dataset

This dataset contains spherical lens prescriptions generated using OptiNeuro.
The generation starts from a base grid of target specifications and extends it through specification perturbations.

## Dataset scope

The dataset contains **147,329 distinct spherical lens prescriptions** associated with **13,090 complete target specifications**.

## Catalogue and file contents

| File | Purpose |
| --- | --- |
| `specs.csv` | Target specifications, indexed by `spec_id` |
| `designs.csv` | Prescriptions, indexed by `design_id`, with relative JSON/ZMX paths |
| `spec_design_links.csv` | Specification–prescription associations |
| `designs/<prefix>/` | Paired JSON prescriptions and ZMX files |

To retrieve lens prescriptions for a target specification, join `specs.csv` to `spec_design_links.csv` using `spec_id`, then to `designs.csv` using `design_id`.
The resulting `json_path` and `zmx_path` values locate the prescription files relative to the directory containing the CSV files.
Each JSON file contains the target settings and optical prescription from a representative source, together with its saved actual performance where available.

Units: HFOV is the half field of view in degrees. Lengths use millimetres, wavelengths use nanometres, and RMS spot radii use micrometres.

## Examples

The [`examples/`](examples/) directory contains 11 example lens prescriptions, one for each target EFL value.
Each example includes a JSON file and a ZMX file.
The accompanying CSV files list their target specifications and specification–prescription links.

## Full dataset download

The full dataset is available from the [Releases page](https://github.com/LensMind/Database/releases).
The Git repository contains only 11 examples from the spherical dataset.

The release includes the catalogue CSV files, JSON and ZMX archives, and SHA256 checksums.
To use the full dataset, download the catalogue and the prescription archives you need, then extract them into the same directory.
