# SPKLU dataset - processing log (v2.0)

- Source: petaspklu.id `/api/v1/spklu/all` (operator Solopos; data PLN). Retrieved 2026-06-16.
- Records: 3029 stations, 4707 charger boxes, 9781 chargers, 5947 connectors.
- Distinct regencies: 375 (BPS codes filled: 375/375).

## Transformations
1. Recomputed station charger/connector totals from boxes (raw totals were 0).
2. Parsed `watt` text -> numeric kW + standardized power tier.
3. Normalized province names -> BPS province codes (34 provinces).
4. Normalized regency names ('Kab/Kota X' -> official style) + admin_level; emitted BPS-code crosswalk template.
5. Inferred AC/DC from charger-box name where stated.
6. Empirical coordinate plausibility screen flagged 20 station(s) (province footprint bbox).
7. Joined province indicators (population = official SP2020 census; GDP = PDRB ADHB 2025; per-province BEV unavailable).
8. Computed coverage metrics for every indicator present.
9. Derived provider_type / operator_brand / location_type NAME-FIRST from nama_lokasi (address used only for the high-precision phrases REST AREA / BANDARA / RUMAH SAKIT) via documented keyword rules (see spklu_category_summary.csv). provider_type separates 'PLN / SPKLU network', 'Independent CPO' (Voltron, Terra Charge, Starvo, DayaGreen, ...), and 'Automaker / dealership' from the explicit operator signal in the name.

## Limitations
- provider_type/location_type are heuristic keyword classifications derived from the station name: provider_type uses the explicit operator signal ('PLN / SPKLU network' is the default for the un-tagged SPKLU brand and so may include unlabelled third-party sites; 'Independent CPO' and 'Automaker / dealership' are detected from named operators/brands). ~22% of sites are 'Other / Unclassified' venues (named only by a specific business or development). Venue keywords are matched against the name; the address is consulted only for a few high-precision phrases (REST AREA / BANDARA / RUMAH SAKIT). Verify before strong claims.
- `status` uniformly active; single snapshot (no closures / time series).
- Regency BPS codes filled from Permendagri 72/2019 source (3029/3029 stations); 1 source province mislabel corrected (Kendari). Verify against the official BPS master.
- Coordinate check is a plausibility screen, not point-in-polygon; needs BPS shapefiles (SOURCES.md #2) for authoritative validation.
- Population = official SP2020 census (BPS; sum-verified to national total); GDP = nominal PDRB ADHB 2025 (BPS via Wikipedia, confirm vs primary for cited year). Per-province BEV unavailable (national only). Source is publicly accessible (petaspklu.id, no login); attribute PLN/Solopos and verify the site's terms of use permit redistribution before deposit.
