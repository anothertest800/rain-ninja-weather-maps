# Rain Ninja weather maps

Live UK and Ireland rain-radar maps for the Rain Ninja motorcycle app (Android). **The app downloads
these files automatically.** You don't need to download anything from here by hand.

Every 5 minutes, a small relay server fetches the newest radar **once** from the Met Office and
EUMETNET OPERA. It merges them onto one 1 km grid, estimates where the rain is moving, and replaces the
files on the [`data`](https://github.com/anothertest800/rain-ninja-weather-maps/tree/data) branch.
That branch always holds exactly one commit, with no history, so the repository stays small.

This repository contains data only: no application code, no rider data, no locations.

## Files (branch `data`)

| Path | What | Updates |
|---|---|---|
| `uk/latest.json` | manifest: frame list, source ages, relay status, attribution | every 5 min (GitHub may cache it up to 5 min) |
| `uk/frames/YYYYMMDDTHHMMZ.h5` | one rain map per 5-minute UTC slot, the last 2 hours | a new file every 5 min; files never change |

Base URL: `https://raw.githubusercontent.com/anothertest800/rain-ninja-weather-maps/data/`

## Format

- **Container:** ODIM_H5 2.4 composite (`/what/object = COMP`), readable by any HDF5 tool.
- **Grid:** 1173 × 1542 cells of 1 km in the Met Office projection
  `+proj=tmerc +lat_0=49 +lon_0=-2 +k=0.999601 +x_0=400000 +y_0=-100000 +ellps=airy +units=m`.
  - Outer upper-left corner at (−326000 m, 1197000 m).
  - Exact corner longitudes and latitudes are in `/where`.
- **`/dataset1/data1/data`:** uint8 DBZH-equivalent of the rain-caution rate.
  - Decode: rate (mm/h) = (10^((raw × 0.5 − 32) / 10) / 200)^(1 / 1.6).
  - `0` = dry, `255` = unknown (no radar sees it, or the feeds disagree on coverage).
  - Values are rounded up: never lighter than measured.
- **"Rain-caution" rules:** a cell is wet if either radar network saw rain there recently, or rain moved into it. It is dry only if every network that is publishing sees it and none saw rain. **Missing data is never shown as dry.**
- **`/motion/u`, `/motion/v`:** int16 × 0.01 = m/s, eastward and northward, on 32 km cells from the same corner.
  Use it to move the map forward up to about 30 minutes.
- **`/how/rain_ninja`:** JSON with the source times, the merge policy and the relay version.

**For clients:** frame names are predictable from the UTC clock. Ask for slot T after about T+6.5 min.
If you get a 404, wait at least 75 s before asking for the same name again (GitHub's CDN caches 404s
for about a minute). Use `latest.json` as a hint, not as the clock.

## Licence and attribution

The maps are derived from:

- **Met Office UK radar composite** (AWS Open Data): British Crown copyright, the Met Office, licensed under
  [CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/).
- **EUMETNET OPERA CIRRUS composite** (Open Radar Data): licensed under [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/).

**Changes made:**
- reprojected and merged;
- OPERA intensities matched to the Met Office distribution;
- older observations moved forward with an estimated motion field;
- re-encoded.

These maps (adapted material) are licensed under **CC BY-SA 4.0**; see [LICENSE](LICENSE).
Please attribute: *"Radar: © Crown copyright Met Office, CC BY-SA 4.0; EUMETNET OPERA, CC BY 4.0.
Merged, reprojected and extrapolated by Rain Ninja."*

Radar shows where rain was observed. It is not a guarantee that a road is wet or dry. Look out of the window.
