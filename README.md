# Gausta Skisenter — Interaktivt ski-kort ⛷️

Interaktivt kort over en skiddag på [Gausta Skisenter](https://www.gaustablikk.no), 1. marts 2026.
Bygget med GPX-data fra Strava, OpenStreetMap piste-data og Open-Elevation API.

**🗺️ [Åbn kortet live](https://cbroberg.github.io/gausta-offpiste/)**

---

## Funktioner

### Kort
- **GPX-rute** farvekodet: cyan = nedkørsel, orange = lift/opstigning
- **Piste-numre 1–35** som farvede cirkler (grøn/blå/rød/sort efter sværhedsgrad)
- **Alle heiser A–M** med navn og bogstav fra OpenStreetMap
- **Hjemmemarkør** 🏠 ved præcise GPS-koordinater (Brendstaultunet 5, 927 m o.h.)
- **Off-piste ski-out** — stiplede linjer fra huset ned til nærmeste officielle pister
- **Ski-in hjem-rute** — piste 22 fra Brendstaulheisen (H) top direkte til huset

### Højdeprofil
- Interaktivt diagram under kortet (Chart.js)
- Farvekodet: cyan = nedkørsel, orange = lift, grå = stationær
- Hover for højde og distance
- Kan foldes ud/ind med ▲/▼-knappen

### Mobil
- Responsive layout med touch-venlige knapper
- Sidepanel som bottom sheet (glider op fra bunden)
- Floating action button til panel-toggle
- Zoom-kontrol placeret i bund-venstre

### Toggles
| Knap | Indhold |
|------|---------|
| Kort | OpenStreetMap standard |
| Piste | OSM + OpenSnowMap piste-overlay |
| Satellit | Esri World Imagery |
| Rute | GPX-track farvekodet (ned/op) |
| Pister | Numre 1–35 med farve efter sværhed |
| Heiser | Lift-navne med bogstav A–M |
| Off-piste | Ski-out forslag fra huset |
| Hjem-rute | Piste 22 fra Brendstaulheisen top → hjem |
| 📋 Løb | Sidepanel med løb og rute-oversigt |

---

## Teknisk løsning

### Datakilder
| Kilde | Bruges til |
|-------|-----------|
| **Strava GPX** (`Gausta_1_.gpx`) | Rute, elevation, tid, puls |
| **OpenStreetMap / Overpass API** | Piste-geometrier, lift-navne, sværhedsgrad |
| **Open-Elevation API** | Verificering af om off-piste er nedad |
| **Nominatim** | Geocoding af adresse til koordinater |
| **OpenSnowMap.org** | Piste-tile-overlay (valgfrit lag) |

### Teknologi
- **Leaflet.js** — interaktivt kort
- **Chart.js** — højdeprofil-diagram
- **Single-file HTML** — alt GPX-data indlejret som JSON, ingen build-step

### GPX-analyse (Python)
```
17.764 trackpoints → downsamples til ~4.400 (hvert 4. punkt)
Segmenteret i descent/ascent ud fra elevation-ændring (threshold: 2m)
15 nedkørsler identificeret · total nedkørsel: 2.940 m
```

### Ski-in/ski-out analyse

**Ski-out fra huset** (ned til piste, verificeret via Open-Elevation API):

| Piste | Afstand | Fald | Hældning | Sværhed |
|-------|---------|------|----------|---------|
| **22** | **277 m** | **26 m** | **5,4° / 9%** | 🟢 Begynder |
| 24/35 | 621 m | 123 m | 11,2° / 20% | 🔵 Let |
| 25 | 723 m | 76 m | 6,0° / 11% | 🔴 Middel |
| 26 | 874 m | 42 m | 2,8° / 5% | 🟢 Begynder |

**Ski-in til huset** (fra Brendstaulheisen H-top):

Piste 22's top er kun **7 meter** fra toppen af Brendstaulheisen (bekræftet via OSM-geometri). Piste 22 er begynder (grøn) og ender **277 m** fra Brendstaultunet 5 — praktisk talt ski-in direkte til huset.

---

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

---

## Brug lokalt

```bash
git clone https://github.com/cbroberg/gausta-offpiste.git
cd gausta-offpiste
python3 -m http.server 8765
# Åbn http://localhost:8765
```

> Kortet virker ikke direkte fra `file://` — tiles kræver HTTP.
