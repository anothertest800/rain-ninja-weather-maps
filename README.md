# Rain Ninja weather maps, `data` branch

Machine-made files, replaced every 5 minutes by the Rain Ninja radar relay. This branch
always holds exactly one commit (no history). For the format and the licence, read the
`main` branch: https://github.com/anothertest800/rain-ninja-weather-maps

* `uk/latest.json`: what is here, source ages, relay status (cached up to 5 min by GitHub)
* `uk/frames/YYYYMMDDTHHMMZ.h5`: one UK rain map per 5-minute slot (ODIM_H5), last 2 hours

Radar: © Crown copyright Met Office, licensed CC BY-SA 4.0; EUMETNET OPERA, licensed CC BY 4.0. Merged, reprojected and extrapolated by Rain Ninja. These maps are licensed CC BY-SA 4.0.
