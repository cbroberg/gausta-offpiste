# Gausta Skisenter — Interaktivt ski-kort ⛷️

Interaktivt kort over en skiddag på [Gausta Skisenter](https://www.gaustablikk.no), 1. marts 2026.
Bygget med GPX-data fra Strava, OpenStreetMap piste-data og Open-Elevation API.

## Hvad kortet viser

**[→ Åbn kortet (index.html)](./index.html)**

- **Din GPX-rute** farvekodet: cyan = nedkørsel, orange = lift/opstigning
- **Piste-numre** (1–35) som farvede cirkler efter sværhedsgrad
- **Alle heiser** med bogstav (A–M) og navn fra OSM
- **Hjemmemarkør** 🏠 ved præcise GPS-koordinater (Brendstaultunet 5, 927 m o.h.)
- **Off-piste forslag** — stiplede linjer fra huset ned til nærmeste officielle pister

## Teknisk løsning

### Datakilder
| Kilde | Bruges til |
|-------|-----------|
| **Strava GPX** (`Gausta_1_.gpx`) | Rute, elevation, tid, puls |
| **OpenStreetMap / Overpass API** | Piste-geometrier, lift-navne, sværhedsgrad |
| **Open-Elevation API** | Verificering af om off-piste er nedad |
| **Nominatim** | Geocoding af adresse til koordinater |
| **OpenSnowMap.org** | Piste-tile-overlay (valgfrit lag) |

### Kort-teknologi
- **Leaflet.js** — interaktivt kort
- **OpenStreetMap** base tiles + valgfri satellit (Esri) og piste-overlay (OpenSnowMap)
- **Single-file HTML** — alt GPX-data er indlejret som JSON, ingen server kræves

### GPX-analyse (Python)
```
17.764 trackpoints → downsamples til ~4.400 (hvert 4. punkt)
Segmenteret i descent/ascent ud fra elevation-ændring (threshold: 2m)
15 nedkørsler identificeret · total nedkørsel: 2.940 m
```

### Off-piste beregning
Fra huset (927 m o.h.) til nærmeste officielle pister — kun ruter der er **nedad** medtages:

| Piste | Afstand | Fald | Hældning | Sværhed |
|-------|---------|------|----------|---------|
| **22** | **277 m** | **26 m** | **5,4° / 9%** | Begynder |
| 24/35 | 621 m | 123 m | 11,2° / 20% | Let |
| 25 | 723 m | 76 m | 6,0° / 11% | Middel |
| 26 | 874 m | 42 m | 2,8° / 5% | Begynder |

Elevationer verificeret via Open-Elevation API. Piste 22 er klart den nemmeste ski-out fra Brendstaultunet 5 — kun 277 m off-piste med 5,4° hældning.

## Dagens statistik

| | |
|---|---|
| **Dato** | 1. marts 2026 |
| **Sted** | Gausta Skisenter, Rjukan, Norge |
| **Tidsrum** | 09:36 – 14:33 UTC |
| **Nedkørsler** | 15 løb |
| **Total nedkørsel** | 2.940 m |
| **Højeste punkt** | 1.090 m o.h. |
| **Laveste punkt** | 704 m o.h. |
| **Overnatning** | Brendstaultunet 5, 927 m o.h. |

## Brug lokalt

```bash
git clone https://github.com/cbroberg/gausta-offpiste.git
cd gausta-offpiste
python3 -m http.server 8765
# Åbn http://localhost:8765
```

Kortet virker ikke direkte fra `file://` — tiles skal hentes via HTTP.

## Kortlag (toggles)

| Knap | Indhold |
|------|---------|
| Kort | OpenStreetMap standard |
| Piste | OSM + OpenSnowMap piste-overlay |
| Satellit | Esri World Imagery |
| Din rute | GPX-track farvekodet (ned/op) |
| Piste-nr | Numre 1–35 med farve efter sværhed |
| Heiser | Lift-navne med bogstav A–M |
| Off-piste | Forslag fra huset ned til nærmeste pister |
