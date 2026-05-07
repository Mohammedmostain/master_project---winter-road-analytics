# NIRA Vehicle & Weather Dataset Report

## Overview

| Property | Value |
|---|---|
| **Source folder** | `vehicle_weather_parquet_files/` |
| **Total files** | 177 daily `.parquet` files |
| **Date range** | 2025-10-01 → 2026-04-01 (6 months) |
| **Missing days** | 6 calendar days have no file (177 of ~183 expected) |
| **Total rows** | ~23,002,002 |
| **Geometry version** | 5831 (consistent across all files) |

---

## Schema

Each file contains **6 columns**:

| Column | Type | Description |
|---|---|---|
| `timestamp` | `datetime64[ns]` | Observation timestamp, 10-minute intervals, 00:00–23:50 local time |
| `segmentid` | `Int64` | Unique road segment identifier |
| `geometryversion` | `Int64` | Version of the road network geometry (always `5831`) |
| `wiperspeedaverage` | `float32` | Average wiper speed across vehicles on the segment — proxy for precipitation intensity |
| `temperatureaverage` | `float64` | Average air temperature on the segment, in **°F** |
| `geometrywkt` | `string` | Road segment geometry as a WKT `LINESTRING` in WGS-84 (lon/lat) |

---

## Geographic Coverage

All segments are located in the **Edmonton, Alberta, Canada** metropolitan area (approx. longitude −113.5°, latitude 53.4°). Each row's `geometrywkt` field encodes the physical shape of the road segment.

---

## File-by-File Structure

Every file follows the same pattern:

- **One calendar day** of data per file (`vehicle_weather_YYYY-MM-DD.parquet`)
- **Timestamps** always span `00:00:00` to `23:50:00` in 10-minute steps
- **19,905–29,489** unique road segments observed per day
- **64,737–170,693** total rows per day (varies with traffic volume and sensor coverage)

---

## Data Ranges & Statistics

### Row counts per day
| Metric | Value |
|---|---|
| Minimum | 64,737 (2026-01-01, New Year's Day) |
| Maximum | 170,693 (2025-10-30) |
| Typical weekday | ~130,000–165,000 |
| Typical weekend/holiday | ~65,000–110,000 |

### Temperature (`temperatureaverage`, °F)
| Metric | Value |
|---|---|
| Dataset minimum | −31.9 °F (−35.5 °C) — Dec 2025 cold snap |
| Dataset maximum | 86.9 °F (30.5 °C) — early October |
| October average | ~45–62 °F (7–17 °C) — autumn |
| November average | ~10–47 °F (−12 to 8 °C) — freeze onset |
| December average | −6 to 29 °F (−21 to −2 °C) — deep winter |
| January average | ~3–39 °F (−16 to 4 °C) |
| February–March average | ~5–48 °F (−15 to 9 °C) — gradual warming |

### Wiper speed (`wiperspeedaverage`, unitless 0–1.04+)
| Metric | Value |
|---|---|
| Range observed | 0.0 – 2.56 |
| Null rows (total) | 49,293 (~0.2% of all rows) |
| Rows with active wipers (> 0) | 1,773,314 (~7.7% of all rows) |
| Notable precipitation days | Oct 11, Oct 27, Nov 2, Nov 7, Dec 4, Dec 19, Mar 19 (wiper_mean > 0.10) |

A value of `0.0` means wipers were off (dry conditions). Higher values indicate heavier precipitation. The column is the only one with nulls, likely from segments with no wiper-equipped vehicles passing.

---

## Seasonal Patterns

| Period | Temperature character | Precipitation pattern |
|---|---|---|
| **Oct 2025** | Mild to cool (45–86 °F) | Occasional rain events |
| **Nov 2025** | Freezing onset (below 0 °F mins by late Nov) | Mixed rain/snow events increasing |
| **Dec 2025** | Deep winter (down to −31.9 °F) | Snow events; many days with light activity |
| **Jan 2026** | Cold winter (−17 to 39 °F) | Moderate precipitation; lower traffic volume |
| **Feb 2026** | Gradually warming (−30 to 27 °F low-end) | Mix of clear and precipitation days |
| **Mar–Apr 2026** | Spring transition (−11 to 70 °F) | Increased rain events; wiper activity picks up |

---

## Data Quality Notes

1. **Missing dates**: 6 days within the Oct 2025 – Apr 2026 range have no file (Dec 25 is confirmed absent; others appear in Mar 2026).
2. **Wiper nulls**: ~0.2% of rows have a null `wiperspeedaverage`. All other columns are complete.
3. **Geometry version**: Constant at `5831` across all files — the road network geometry did not change during this period.
4. **Low-traffic days**: New Year's Day (Jan 1), Christmas (Dec 25 missing), and some Sundays show noticeably fewer rows, reflecting reduced vehicle sensor coverage.
5. **Outlier wiper values**: Jan 3 shows a max wiper speed of 2.56 (vs. the usual cap of ~1.04) — likely a sensor anomaly on one segment.
