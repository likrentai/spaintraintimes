# Train journey times from Madrid

How high-speed rail changed travel times across Spain over the past forty years.

**Live pages**

Interactive time map and Madrid to Barcelona diagram: https://likrentai.github.io/spaintraintimes/

Case study: https://likrentai.github.io/spaintraintimes/case-study.html

Case study as PDF: https://likrentai.github.io/spaintraintimes/case-study.pdf

## What this project shows

Using train timetables from 1985 to 2026 for 32 cities of mainland Spain, this project measures how much closer each city came to Madrid by train, which cities were left out, whether the train now beats the car, and what the 2026 speed restrictions did to the busiest line.

The short answer: closer by train, but not for everyone. Train times from Madrid to cities with high-speed track all the way fell to about 40% of their 1985 level (median of 21 cities). Cities reached only part of the way by high-speed track fell to about 69%, and Jaén, with no high-speed track, stayed at 98%. How far the fast line reaches matters: Pamplona, with a short conventional end, is at 50%, Bilbao and San Sebastián at 80%. From Madrid, the fastest train beats free-flowing traffic to 26 of 31 cities, by about two to one where high-speed track runs all the way. In 2026, speed restrictions after the Adamuz accident added 25 minutes to Madrid to Barcelona (2 h 37 to 3 h 02), for all three operators on the line: on Wednesday 25 November 2026, 89 trains of Renfe, Iryo and Ouigo.

The page has three parts:

1. **Time map:** each city keeps its true direction from the centre, and its distance is the fastest train time. The map morphs from the real map through 1985, 1993, 2001, 2010, 2018, 2025 and 2026. Tap any city to see the map from there.
2. **Compare with car:** in 2025 and 2026, a grey mark on each city's direction at its driving time on today's roads.
3. **One day on the Madrid to Barcelona line:** all 89 trains of 25 November 2026 as a Marey diagram (time across, stations down), with each train's times and stops.

## An honest note on the data

- The measure is the fastest scheduled train on a normal weekday, station to station. It says nothing about frequency, price, punctuality or getting to the station.
- **1985, 1993 and 2001** come from Renfe timetable guides (scanned by the Biblioteca Ferroviaria), typed by hand for about 70 key links. The 2001 guide lists long-distance trains only, so 11 of its 68 links are carried over from earlier years and marked.
- **2010 and 2018 are reconstructions:** the 2025 key links, changed only where a line opened, from published opening times and time savings. Every changed row says so in its note column; derived times are marked "estimate".
- **2025 and 2026** come from Renfe's open timetables (GTFS): the archived feed of 18 December 2025 (Wednesday 14 January 2026) and the feed of 16 November 2026 (Wednesday 25 November 2026).
- The map uses the same simple network of key links in every year, with 60 minutes added for each change of train. On the 2025 timetable this matches the full timetable answer with a median error of about zero (64% of city pairs within 30 minutes). The comparison with the car uses the full timetables.
- A flat map can be exact only from its centre: trips between two outer cities mostly pass through Madrid. The stretched coastline is a backdrop; only the cities are measured.
- Narrow-gauge lines on the north coast (Bilbao to Santander and to San Sebastián) are not in Renfe's data and were added by hand. In 2018 Granada had no train (AVE plus a Renfe bus), and in 2026 Córdoba to Jaén is a replacement bus; both are kept and marked.
- **Iryo publishes no open timetable.** The Madrid to Barcelona diagram uses an unofficial community feed (by Clément Desouche, built from iryo.eu), checked against ticket listings: on Wednesday 2 December 2026 all 28 Iryo trains matched to the minute, and on 25 November every train on sale matched.
- Driving times are OSRM (car profile) on OpenStreetMap, free flow (no traffic, parking or breaks). OSRM came out about 5% slower than RACE's published route times, which favours the train a little; the case study gives both.

## Files

| Path | What it is |
|---|---|
| `00renfegtfs.ipynb` | Downloads Renfe's timetables (today's feed and the archived feed of 18 Dec 2025) and takes a first look |
| `01cities.ipynb` | Picks the main station of each of the 32 cities |
| `02times.ipynb` | Fastest scheduled journey between every pair of cities on one weekday, from the full 2025 and 2026 timetables |
| `03shortcut.ipynb` | Key links for every year (1985 to 2026) and shortest paths with 60 minutes per change; calibration against the full timetable |
| `04timemap.ipynb` | Radial positions of every city for every year and centre |
| `05outline.ipynb` | Stretches the coastline of the Madrid map for every year |
| `06pagedata.ipynb` | Writes `docs/data/timemap.json` for the page |
| `07road.ipynb` | Driving times from OSRM, train against car, `docs/data/railroad.json` |
| `08mareyfeeds.ipynb` | Downloads the Ouigo and Iryo timetables and keeps every Madrid to Barcelona train of 25 Nov 2026 |
| `09marey.ipynb` | Marey diagram data and numbers, `docs/data/marey.json` |
| `10casestudy.ipynb` | Case study charts; prints every number quoted in the text |
| `data/raw/manual/` | Hand-compiled key links for 1985, 1993, 2001, 2010 and 2018, extra narrow-gauge links, and the access group of each city |
| `data/raw/osrm/` | The saved OSRM answer (so `07road` runs without asking the server again) |
| `data/clean/` | Tidy tables made by the notebooks |
| `docs/index.html` | The interactive page (plain HTML and JavaScript, no libraries) |
| `docs/case-study.html`, `docs/case-study.pdf` | The case study |
| `docs/data/` | The JSON files the page reads |
| `docs/fonts/`, `docs/images/` | Fonts and case study pictures |

## How to run it

1. Create an environment with Python 3.12 and install the packages: `pip install -r requirements.txt`.
2. Run the notebooks in order, 00 to 10. They download what they need the first time: Renfe's timetables (`00renfegtfs`), the Natural Earth coastline (`05outline`), driving times from OSRM only if `data/raw/osrm/` is empty or `FETCH_AGAIN = True` (`07road`), the Ouigo and Iryo timetables from the Mobility Database archive (`08mareyfeeds`) and the chart font (`10casestudy`).
3. To view the page, run `python -m http.server 8000` in the `docs` folder and open http://localhost:8000 (opening `index.html` as a file cannot read the data).

Renfe's live timetable changes every few weeks; `00renfegtfs` keeps the version it downloads in a dated folder, and later notebooks use the folders named in their settings.

## Credits

- Renfe timetables: Renfe Viajeros open data, "Horarios de alta velocidad, larga distancia y media distancia", https://data.renfe.com. Archived copy of 18 December 2025: Mobility Database (MobilityData), feed mdb-2620.
- Historical timetables: Renfe timetable guides 1985, 1993 and 2001, scanned by the Biblioteca Ferroviaria, Fundación de los Ferrocarriles Españoles.
- Ouigo España timetable, processed: Powered by MITRAMS, Ministerio de Transportes y Movilidad Sostenible (https://www.transportes.gob.es/), via the Punto de Acceso Nacional de Transporte Multimodal (https://nap.transportes.gob.es), licence https://nap.transportes.gob.es/licencia-datos. Archived copy: Mobility Database, feed mdb-2785.
- Iryo timetable: unofficial community feed compiled by Clément Desouche (@deryclem), https://clementdesouche.fr/gtfs, CC BY 4.0; timetables from the public iryo.eu website; not affiliated with iryo. Archived copy: Mobility Database, feed mdb-3645.
- Road network: © OpenStreetMap contributors, ODbL (https://www.openstreetmap.org/copyright); routing by OSRM on the FOSSGIS server (https://routing.openstreetmap.de).
- Reference driving times: RACE route pages, https://www.race.es.
- Coastline: Natural Earth, 1:10m admin 0 countries, https://www.naturalearthdata.com, public domain.
- Kilometre points on the Madrid to Barcelona line: Wikipedia, route diagram of the Madrid to Barcelona high-speed rail line and the Lleida Pirineus station page.
- Ticket listings used to check the timetables: Kombo, https://www.kombo.co.
- Fonts: Bricolage Grotesque and Newsreader, SIL Open Font License.

## Licence

Code: MIT (see `LICENSE`). Tables made by this project in `data/raw/manual`, `data/clean` and `docs/data`: CC BY 4.0, credit "Lik Ren Tai", except tables built from OpenStreetMap driving times (`data/raw/osrm/`, `data/clean/road_times.csv`, `data/clean/rail_road_*.csv`, `docs/data/railroad.json`), which are under the ODbL. Third-party timetables, the coastline and the fonts stay under their own terms.

This is an independent project, not affiliated with or endorsed by Renfe, Adif, Iryo, Ouigo, the Ministerio de Transportes y Movilidad Sostenible or any of the sources.

Lik Ren Tai, November 2026