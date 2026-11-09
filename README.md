# A Geocoded Dataset of Public EV Charging Stations in Indonesia with Provider, Venue, and Provincial Coverage Information

Author: Richard Wiputra, Bina Nusantara University (richardwiputra@gmail.com)

Version: 1.0. Data retrieved: 2026-06-16. Spatial coverage: Indonesia (34 provinces, pre-2022 structure).

## Description

This dataset documents 3,029 public electric-vehicle charging stations (*Stasiun Pengisian Kendaraan Listrik Umum*, SPKLU) across Indonesia, comprising 4,707 charger boxes, 9,781 charger units, and 5,947 connectors. Each station record includes WGS84 coordinates, parsed power ratings, official BPS administrative codes (province and regency), a derived operator class (PLN / SPKLU network, independent charge-point operator, or automaker dealership), the operator/brand name where identifiable, and a venue-type classification. Province-level reference tables add population and GDP indicators together with per-capita and per-GDP coverage metrics, supporting analysis of charging-infrastructure equity and spatial gaps.

The records were obtained from the public map at petaspklu.id (JSON endpoint `/api/v1/spklu/all`; operator Solopos; underlying data PT PLN (Persero)). They were cleaned, validated, geocoded to BPS codes, and enriched with a documented, reproducible pipeline (`build_dataset.py`), which is openly available in the project's code repository (see "Code and reproducibility" below).

## Suggested reading order

1. `README.md` (this file): overview, methods, limitations, licensing.
2. `spklu_data_dictionary.csv`: definition of every field in every data file.
3. `data/spklu_stations.csv`: the primary dataset, one row per station (27 columns).
4. `data/spklu_chargers.csv`: charger-box detail, one row per box (join on `station_id`).
5. `reference/spklu_province_coverage.csv`: province-level coverage metrics.
6. `reference/spklu_province_indicators.csv` and `reference/spklu_regency_codes.csv`: the population and GDP inputs, and the regency-code crosswalk whose `match_status` column records how each code was assigned.
7. `docs/PROCESSING_LOG.md`: methods and limitations.
8. `docs/SOURCES.md`: sources used and items to confirm against primary BPS.
9. `supplementary/`: category summaries and the provider-by-venue crosstab.

## File inventory

| Folder | File | Rows | Role |
|---|---|---|---|
| data/ | spklu_stations.csv | 3,029 | Primary table, one row per station |
| data/ | spklu_chargers.csv | 4,707 | One row per charger box |
| reference/ | spklu_province_coverage.csv | 34 | Province coverage metrics |
| reference/ | spklu_province_indicators.csv | 34 | Population, GDP, and EV inputs |
| reference/ | spklu_regency_codes.csv | 375 | Regency BPS-code crosswalk |
| supplementary/ | spklu_category_summary.csv | 25 | provider_type, location_group, location_type counts |
| supplementary/ | spklu_category_crosstab.csv | 7 | provider_type by location_group |
| docs/ | PROCESSING_LOG.md, SOURCES.md | n/a | Methods, limitations, sources |

## Methods

Full detail is in `docs/PROCESSING_LOG.md`. In summary:

1. Recomputed station charger and connector totals from the charger boxes (the raw source totals were zero).
2. Parsed free-text power values ("22 kW") into numeric kW and a standardized power tier.
3. Normalized province names to BPS province codes (34 provinces).
4. Normalized regency names and administrative level, then matched them to BPS/Permendagri No. 72/2019 four-digit codes. All 3,029 stations are coded; one source province mislabel (Kendari) was corrected.
5. Inferred AC or DC current from charger-box names where stated.
6. Applied an empirical coordinate plausibility screen (review candidates flagged, none deleted).
7. Joined provincial indicators and computed coverage metrics.
8. Derived `provider_type` and `operator_brand` from the explicit operator signal in the station name (PLN / SPKLU network, Independent CPO, or Automaker / dealership), and `location_type`/`location_group` name-first (the address is consulted only for the high-precision phrases "rest area", "bandara", and "rumah sakit") using documented keyword rules.

## Code and reproducibility

This deposit contains the data only. The processing pipeline that builds it from the raw source (`build_dataset.py`, with the regency matcher `match_regency_codes.py`) and the figure scripts are openly available in the project's code repository: https://github.com/ (URL to be added on release). Full processing detail is in `docs/PROCESSING_LOG.md`.

## Key descriptive findings

1. Access varies roughly twentyfold across provinces: DKI Jakarta has 6.9 stations per 100,000 residents against 0.35 in Papua.
2. On a GDP-normalized basis, Bali (0.36) exceeds Jakarta (0.19) stations per trillion rupiah of regional GDP.
3. The operator structure has three tiers: PLN's own SPKLU network (1,876 stations, 62 percent) supplies almost all utility and public-civic sites; independent charge-point operators (716, 24 percent) concentrate at offices, hotels, and retail; and automaker dealerships (405, 13 percent) are entirely showrooms in the retail-and-commercial group.

## Known limitations and verification status

1. The `status` field is uniformly active. The dataset is a single snapshot and contains no closed or offline stations and no time series.
2. Population is the official SP2020 census (BPS); the 34-province values sum exactly to the national total of 270,203,917. GDP is nominal PDRB ADHB for 2025 (BPS, obtained via a secondary compilation); confirm the figures against the primary BPS table for the year cited. The post-2022 Papua provinces are aggregated back to the 34-province structure.
3. Per-province EV registrations are not publicly available (only national totals exist), so `bev_registered` and `chargers_per_1000_bev` are left blank.
4. `provider_type` and `location_type` are heuristic, name-based classifications. "PLN / SPKLU network" is the default for the un-tagged SPKLU brand and may include some unlabelled third-party sites; "Independent CPO" and "Automaker / dealership" are assigned only when the operator or brand is explicit in the name. About 22 percent of venues fall in "Other / Unclassified" (stations named only by a specific business or development). Classification is keyed on the station name; the street address is consulted only for a few high-precision phrases. These labels should be verified before strong claims are drawn.
5. Coordinate validation is a plausibility screen, not point-in-polygon containment, which would require BPS boundary shapefiles.
6. Reference years differ (population 2020, GDP 2025, charging snapshot 2026); this should be stated when reporting.

## Licensing and reuse

The source data is publicly accessible at petaspklu.id without authentication, and individual station locations are factual records. Before redistribution, confirm that the site's terms of use carry no explicit restriction. The recommended license is Creative Commons Attribution 4.0 (CC BY 4.0), to be applied once that confirmation is complete. PT PLN (Persero) and Solopos are attributed as the source.

## Sources and attribution

1. Charging-station records: petaspklu.id, endpoint `/api/v1/spklu/all` (operator Solopos; data from PT PLN (Persero)), publicly accessible without authentication, retrieved 2026-06-16.
2. Administrative codes: BPS and Permendagri No. 72/2019; the regency master was joined from the open emsifa/api-wilayah-indonesia dataset.
3. Population: BPS *Sensus Penduduk 2020*. GDP: BPS *PDRB Atas Dasar Harga Berlaku*. National EV totals: Korlantas POLRI.

## Recommended citation

Wiputra, R. (2026). *A Geocoded Dataset of Public EV Charging Stations in Indonesia with Provider, Venue, and Provincial Coverage Information* (Version 1.0) [Data set]. Mendeley Data. DOI assigned by Mendeley on publication.

## License

Creative Commons Attribution 4.0 (CC BY 4.0), pending the terms-of-use confirmation noted above.

## Contact

Richard Wiputra, Bina Nusantara University. Email: richardwiputra@gmail.com
