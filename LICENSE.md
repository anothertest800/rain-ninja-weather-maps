# Licence and attribution

The radar maps in this repository are derived from:

* Met Office UK radar composite (AWS Open Data, `met-office-radar-obs-data`):
  British Crown copyright, the Met Office, licensed under CC BY-SA 4.0
  https://creativecommons.org/licenses/by-sa/4.0/
* EUMETNET OPERA CIRRUS composite (Open Radar Data): licensed under CC BY 4.0
  https://creativecommons.org/licenses/by/4.0/

Changes: reprojected to a 1 km UK grid, merged (rain-caution rules), OPERA intensities matched to
the Met Office distribution, older observations moved forward with an estimated motion field,
encoded as uint8 DBZH-equivalent (Z = 200 R^1.6).

The maps (adapted material) are licensed under CC BY-SA 4.0. Attribution:

Radar: © Crown copyright Met Office, licensed CC BY-SA 4.0; EUMETNET OPERA, licensed CC BY 4.0. Merged, reprojected and extrapolated by Rain Ninja. These maps are licensed CC BY-SA 4.0.

Radar shows where rain was observed; it is not a guarantee of where rain is or is not falling.
Missing data is marked unknown, never dry.
