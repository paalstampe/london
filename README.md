# Pål's London

Interaktivt kart over nabolag, gater, gåturer og steder i London.

Publisert: https://paalstampe.github.io/London/

## Struktur

```
London/
├── index.html          struktur: kartcontainer, sidebar, filtre
├── style.css           alt visuelt: farger, typografi, kort
├── app.js              logikk: henter GeoJSON, tegner på Leaflet, filtrerer
├── data/
│   └── london.geojson  dataene — dette er "databasen"
├── arkiv/              tidligere versjoner, ikke i bruk
├── HANDOVER.md         statusdokument
└── README.md
```

## Datamodell

Hvert sted er en GeoJSON-`Feature`:

- `geometry.type: "Point"` for steder — `coordinates: [lengdegrad, breddegrad]` (merk rekkefølgen)
- `geometry.type: "LineString"` for gater og gåturer — liste av koordinatpar i rekkefølge

`properties`:

| felt | verdi |
|---|---|
| `navn` | visningsnavn |
| `kategori` | nabolag, cafe, bakeri, restaurant, bar, butikk, marked, park, galleri, gate, gaatur |
| `sone` | Central, North, South, East, West |
| `favoritt` | true / false |
| `besokt` | true / false |
| `notat` | fritekst |
| `place_id` | Google Place ID (valgfritt, for dyplenking) |
| `lengde_km`, `varighet_min` | kun for gåturer |

Nye felter kan legges til fritt — koden ignorerer det den ikke kjenner.

## Redigering

- **Punkter og tekst:** rediger `data/london.geojson` direkte, eller på GitHub i nettleseren.
- **Gater og gåturer:** bruk [geojson.io](https://geojson.io) — dra inn filen, tegn med linjeverktøyet, last ned igjen.

## Lokal kjøring

`fetch()` blokkeres ved åpning fra `file://`. Kjør i stedet:

```
cd London
python3 -m http.server
```

og åpne `http://localhost:8000`.

## Publisering

GitHub Pages: Settings → Pages → Deploy from a branch → `main` / `(root)`.
