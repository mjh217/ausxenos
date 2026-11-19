# AusXenos

[![DOI](https://zenodo.org/badge/DOI/10.5281/zenodo.24381196.svg)](https://doi.org/10.5281/zenodo.24381196)

A compilation of major-element mineral chemistry for mantle (and lower-crustal) xenoliths, xenocrysts and diamond inclusions from Australia, together with locality information and eruption ages for their host rocks. This dataset compilation was undertaken as part of Geoscience Australia's [*Resourcing Australia's Prosperity*](https://www.ga.gov.au/scientific-topics/resourcing-australias-prosperity) initiative (eCat record 151570).

## Contents

```
AusXenos.xlsx                Master spreadsheet (localities, geochemistry, code tables, Fe2O3 converter)
csv_version/
  AusXenos_geochem.csv       Mineral analyses (one row per sample/grain)
  AusXenos_locs.csv          Locality coordinates, host-rock ages and references
references.bib               BibTeX entries for all references cited in the database
LICENSE                      CC-BY-4.0
```

The CSV files are exported directly from `AusXenos.xlsx` (sheets `Geochem` and `Localities`) and are the recommended files for programmatic use.

## `AusXenos_geochem.csv`

Each row is a single sample or grain. Leading columns:

| Column | Description |
|---|---|
| `SAMPLE` | Sample / grain identifier, as given in the source publication |
| `LOCALITY` | Locality name; joins to `LOCALITY` in `AusXenos_locs.csv` |
| `AL PHASE` | Aluminous phase(s) present in the xenolith: `G` garnet, `S` spinel, `P` plagioclase, or combinations (e.g. `GS`). Blank for xenocrysts / where not reported |
| `LITHOLOGY` | Lithology code (see below) |
| `SOURCE` | Publication the analysis was compiled from |

These are followed by five blocks of 15 columns, one per mineral, each prefixed by a mineral code:

| Prefix | Mineral |
|---|---|
| `OL` | Olivine |
| `OP` | Orthopyroxene |
| `CP` | Clinopyroxene |
| `GT` | Garnet |
| `SP` | Spinel |

Each block contains oxide concentrations in wt%: `SiO2, TiO2, Al2O3, Cr2O3, FeOt, MnO, NiO, MgO, CaO, Na2O, K2O, ZnO, V2O3, P2O5` and the analytical total `Tot`. All iron is reported as FeO (`FeOt`); analyses originally reported with Fe2O3 were converted using the `Fe2O3 converter` sheet in `AusXenos.xlsx`. A blank cell means the oxide was not reported.

### Lithology codes

| Code | Lithology |
|---|---|
| `XENOC` | Xenocryst |
| `INCL` | Inclusion (in diamond) |
| `XENOL` | Xenolith (unclassified) |
| `PER` | Peridotite (generic) |
| `LHZ` | Lherzolite |
| `HARZ` | Harzburgite |
| `WEHR` | Wehrlite |
| `DUN` | Dunite |
| `PYR` | Pyroxenite |
| `WEB` | Websterite |
| `CPYR` | Clinopyroxenite |
| `HBL` | Hornblendite |
| `ECL` | Eclogite |
| `GRAN` | Granulite |

## `AusXenos_locs.csv`

| Column | Description |
|---|---|
| `LOCALITY` | Locality name |
| `SHORT FORM` | Machine-friendly identifier |
| `REGION` | Geological province / state |
| `COUNTRY` | Country |
| `LON. (GDA94)`, `LAT.  (GDA94)` | Decimal degrees, GDA94 datum. Some locations are approximate (see `NOTES`) |
| `AGE (Ma)` | Preferred eruption / emplacement age of the host rock |
| `UNCERTAINTY (Ma)` | Age uncertainty, or `Inferred` where the age is estimated from geological relationships or regional correlation |
| `ANALYSES REFERENCES` | Sources of the mineral analyses at this locality (BibTeX keys in `references.bib`) |
| `AGE REFERENCES` | Sources of the age (BibTeX keys in `references.bib`) |
| `NOTES` | Details of age determinations, site groupings and caveats |

## Quick start

```python
import pandas as pd

geochem = pd.read_csv("csv_version/AusXenos_geochem.csv")
locs = pd.read_csv("csv_version/AusXenos_locs.csv")

# Garnet analyses with locality coordinates and host-rock ages
gt = geochem.dropna(subset=["GTSiO2"]).merge(locs, on="LOCALITY")
```

## Citation

If you use AusXenos, please cite both the paper and the dataset:

> Hoggard, M. J., Sudholz, Z. J., Hawkins, R., Duvernay, T., Bagchi, S., Richards, F. D., Jaques, A. L., Yaxley, G. M., Czarnota, K., & Haynes, M. (2026). Paleothermal structure of the Australian continental lithosphere. *Geochemistry, Geophysics, Geosystems* (accepted).

> Hoggard, M. J., Sudholz, Z. J., & Jaques, A. L. (2026). AusXenos: A database of major-element mineral chemistry for Australian mantle xenoliths and xenocrysts. Zenodo. https://doi.org/10.5281/zenodo.24381196

The dataset DOI above always resolves to the latest version. Each release also has its own DOI on Zenodo (v2026.11: [10.5281/zenodo.24381197](https://doi.org/10.5281/zenodo.24381197)); cite the version-specific DOI to refer to an exact snapshot of the data.

Please also cite the original sources of the analyses you use (listed in the `SOURCE` column and in `ANALYSES REFERENCES`). BibTeX entries for all references are in `references.bib`, with keys matching those in `AusXenos_locs.csv`.

## Licence

AusXenos is released under the [Creative Commons Attribution 4.0 International](https://creativecommons.org/licenses/by/4.0/) licence (CC-BY-4.0). See `LICENSE`.

## Contact

Mark Hoggard — please open an issue for corrections or additions.
