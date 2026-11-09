# Reference sources and provenance

Provenance of the administrative, population, and economic reference values used in this dataset, with notes on items to confirm against the primary sources.

## 1. BPS regency (kabupaten/kota) master codes — spklu_regency_codes.csv : bps_regency_code
- Permendagri No. 72/2019 "Kode dan Data Wilayah Administrasi Pemerintahan", Kemendagri.
- BPS "Master File Desa / Kode Wilayah" (https://www.bps.go.id) — 4-digit kab/kota codes.
- Codes were matched on (bps_province_code, kabupaten_kota_clean); the regency master was joined from the open emsifa/api-wilayah-indonesia dataset (based on Permendagri No. 72/2019).

## 2. BPS administrative boundary shapefiles (for authoritative point-in-polygon)
- Badan Informasi Geospasial (BIG) / "Indonesia GeoPortal" RBI admin boundaries, or
- GADM v4.1 (https://gadm.org) levels 1–2.
- These would replace the empirical coordinate plausibility flags with true point-in-polygon containment checks.

## 3. Population — spklu_province_indicators.csv : population_sp2020
- BPS Sensus Penduduk 2020 (SP2020), provincial table. Official SP2020 values are used; the 34-province sum equals the national total of 270,203,917.

## 4. Provincial GDP — spklu_province_indicators.csv : grdp_current_idr_billion (+ grdp_year)
- Primary source: BPS "PDRB Atas Dasar Harga Berlaku Menurut Provinsi (miliar rupiah)"
  https://www.bps.go.id/en/statistics-table/3/WkdVMWRYVnBkMnBvVEhKSVkyWXhNblZtTjJSbmR6MDkjMw==
  and the publication "PDRB Provinsi-Provinsi di Indonesia menurut Pengeluaran, 2020–2024" (BPS, 2025).
- Values are nominal PDRB ADHB for 2025 (grdp_year = 2025). The four post-2022 Papua provinces are aggregated into Papua (94) and the new West Papua provinces into Papua Barat (91) for the 34-province structure.
- The 34 values were cross-checked against the BPS-sourced figures. The BPS portal table is interactive, so the figures should be confirmed against the primary BPS table for the year cited.

## 5. EV registrations — spklu_province_indicators.csv : bev_registered
- Korlantas POLRI registered BEV by province (registration domicile), or
- Gaikindo / AISI wholesales (national; a provincial split is generally unavailable).
- No open per-province BEV breakdown was found; only national totals exist (~68,207 EVs, Korlantas Sep-2023). bev_registered is left blank and chargers_per_1000_bev is not computed.
