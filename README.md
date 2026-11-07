# EmberCast forecast ledger

Public record of every live forecast issued by **EmberCast**, an early-warning system for crop-residue burning in Punjab, India.
Made by Kamaaksha Kharul, Aakash Jawle, Viraj Jadhav and Kshitij Londhe.

Each forecast gives, for every one of Punjab's 1,907 squares of about 5 × 5 km, the chance that a satellite sees at least one crop fire
in that square during the next 7 days. Forecasts are committed here **before the week they describe begins**, so anyone can check them
afterwards against NASA FIRMS fire detections without having to trust us.

## Files

| File | Contents |
|---|---|
| `batches/forecast_<date>.json` | One forecast batch: issue time, forecast window, model fingerprint, which satellite served each look-back day, and every square's chance (`p`) and likely range (`p_lo`, `p_hi`). Canonical JSON. |
| `ledger.jsonl` | One line per batch: `batch_sha256` (fingerprint of the batch file), `model_sha256`, `prev_entry_sha256` and `entry_sha256`. Each entry's fingerprint covers the one before it, so changing any old forecast breaks every later line. |

## How to check a forecast

1. Compute the SHA-256 of a batch file and compare it with `batch_sha256` in `ledger.jsonl`:
   * Windows: `certutil -hashfile batches\forecast_2026-11-07.json SHA256`
   * Linux or macOS: `sha256sum batches/forecast_2026-11-07.json`
2. Check the commit time of that file in this repository's history. It must be earlier than the first day of its forecast window.
3. Score it against NASA FIRMS VIIRS active fires for Punjab over the window (EmberCast's dashboard does this automatically).

## Publication times, stated plainly

| # | Forecast week | Locked on the team's machine (UTC) | Published here |
|---|---|---|---|
| 1 | 6 to 12 Nov 2026 | 5 Nov 2026, 17:59 | 6 Nov 2026, 20:04 UTC (after its week had started) |
| 2 | 7 to 13 Nov 2026 | 6 Nov 2026, 17:54 | 6 Nov 2026, 20:04 UTC (01:34 on 7 Nov in India, so after its week had started there) |

Forecasts 1 and 2 were locked and chained locally before their weeks began, but this public repository was only created afterwards, so
GitHub's timestamps cannot prove that for them. **From forecast 3 onward, each batch is pushed here automatically when it is issued,
before its week begins.**

Licence: CC BY 4.0. Fire data: NASA FIRMS. Harvest and straw data: Copernicus Sentinel-2.
