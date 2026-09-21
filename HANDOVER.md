# Nabolag London — overlevering

Statusdokument for å ta prosjektet videre i en Cowork-økt.
Sist oppdatert: 20. september 2026 (appen er bygget og publisert).

---

## 1. Målet

En enkel nettbasert app som viser Påls London-nabolag på kart, med flere kategorier
(caféer, parker, butikker, markeder, gallerier) og markerte gater og gåturer.
Skal kunne deles med andre via lenke, uten at mottaker må logge inn noe sted.

Todelt strategi:

- **Webappen** — for planlegging, lesing og deling. Hostes på GitHub Pages.
- **Google My Maps** — parallelt spor for bruk i felt, siden My Maps-kart dukker
  opp i Google Maps-appen på mobil med navigasjon. Mates fra samme data via KML.

---

## 2. Hvor prosjektet står

Ferdig:

- Datamodellen er definert og `data/london.geojson` er fylt med 44 nabolag,
  pluss tre eksempelpunkter (park, café, galleri) og to eksempel-linjer
  (én gate, én gåtur). Eksemplene er merket `EKSEMPEL` i `notat`-feltet.
- `README.md` dokumenterer datamodellen.
- Tre visuelle retninger er tegnet som artboards. **Retning A er valgt.**

- `index.html`, `style.css` og `app.js` er bygget i retning A og testet:
  49 features tegnes, filtre på sone/kategori/favoritt virker, søk virker,
  gåtur-kortet virker, ingen konsollfeil, ingen horisontal scroll på mobil.
- Gammel prototype `london-map_1.html` er flyttet til `arkiv/`.
  Duplikatet `london.geojson` i rotmappen er slettet — `data/london.geojson` er fasit.

- Repoet `paalstampe/London` er opprettet og publisert via GitHub Pages:
  https://paalstampe.github.io/London/ (Deploy from a branch, `main` / `(root)`).

Gjenstår:

- Finjustering av koordinatene i de to eksempel-linjene (grove skisser).
- Eventuelt KML-eksport for My Maps-sporet.

---

## 3. Filer

Arbeidskatalogen er den lokale klonen av repoet:
`/Users/palstampe/Documents/GitHub/London/`.
Dette er eneste gjeldende kopi. Tidligere kopier i `~/Downloads/london-nabolag/`
og `Documents/Privat/Prosjekter/Nabolag London/` er utgått.

```
London/
├── index.html                      ← ferdig, retning A
├── style.css                       ← ferdig
├── app.js                          ← ferdig
├── data/
│   └── london.geojson              ← ferdig
├── arkiv/
│   └── london-map_1.html           ← gammel prototype, ikke i bruk
├── Tips til nabolag i London.md    ← råmateriale, kilden til dataene
├── .gitignore
├── README.md
└── HANDOVER.md                     ← dette dokumentet
```

Designretningene ligger som artboards her:
https://claude.ai/artifact/StKKz6su1kLPBVyy4bZ9NN
(A = `Main.dc.html`, den valgte.)

---

## 4. Datamodellen

Hele datasettet er én GeoJSON `FeatureCollection`. Punkter er steder,
`LineString` er gater og gåturer.

Koordinatrekkefølge er `[lengdegrad, breddegrad]` — motsatt av Google Maps.
Dette er den vanligste feilkilden ved manuell redigering.

`properties` per feature:

| felt | verdi |
|---|---|
| `navn` | visningsnavn |
| `kategori` | nabolag, cafe, bakeri, restaurant, bar, butikk, marked, park, galleri, gate, gaatur |
| `sone` | Central, North, South, East, West |
| `favoritt` | true / false |
| `besokt` | true / false |
| `notat` | fritekst |
| `place_id` | Google Place ID, valgfritt |
| `lengde_km`, `varighet_min` | kun gåturer |

Prinsippet: data og presentasjon er adskilt. Nye steder legges inn i GeoJSON-filen
uten at koden røres; nytt design endres i CSS uten at dataene røres. Ny kategori
krever ett oppslag i fargetabellen i `app.js` pluss en filterknapp.

Gater og gåturer tegnes enklest i [geojson.io](https://geojson.io): dra inn filen,
tegn med linjeverktøyet langs gatenettet, fyll ut `properties` i tabellen, last ned.

---

## 5. Valgt designretning: A — redaksjonell

Typografi (Google Fonts):

- Display: **Playfair Display**, 500 og 700
- Brødtekst: **Work Sans**, 400/500/600

Farger:

| rolle | hex |
|---|---|
| bakgrunn (papir) | `#F7F4EE` |
| kartflate | `#EFEADF` |
| kort / input-flate | `#FFFDF8` |
| tekst | `#241F19` |
| dempet tekst | `#6B5D4A` |
| kantlinje | `#DCD2C0` / `#E0D8C9` |
| aksent (kobber) | `#8A5A2B` |
| aksent hover | `#63401D` |
| park | `#D6DFCB` |
| vann | `#B9C9CE` |

Layout (desktop 1440×900):

- Sidebar 440 px til venstre, kart fyller resten.
- Sidebar ovenfra: kicker i små caps → tittel i Playfair 40 px → søkefelt →
  filterknapper som rektangulære chips (44 px høye, 3 px radius) → tellerlinje
  med tynn skillelinje → liste med steder.
- Listeelementer er korte oppslag, ikke tabellrader: navn i Playfair 21 px,
  metalinje «Sone · Kategori · Status» i 13 px dempet, eventuelt notat i 14 px.
  Favoritter merkes med små caps i aksentfarge til høyre, ikke ikon.
- Kort for valgt gåtur nede til venstre over kartet: 300 px bredt, hvit flate,
  1 px kantlinje, ingen skygge.

Prinsipper som skiller A fra de andre: papirflate framfor hvitt, serif kun til
navn og titler, ingen skygger, ingen avrundede kort — kantlinjer og luft gjør
jobben. Nærmer seg en guidebok.

---

## 6. Teknisk retning

- **Leaflet** for kartet. Gratis, ingen API-nøkkel.
- Kartfliser: **CARTO Positron** eller **Voyager** — vesentlig penere enn standard
  OSM og lar papirpaletten dominere. Husk attribusjonskravet.
- Kategorier styrer markørfarge og -ikon. Gåturer som polyline med popup.
- Clustering vurderes først når punktantallet vokser.
- Ingen byggesteg, ingen rammeverk. Fire statiske filer.

Praktisk: `fetch()` blokkeres når `index.html` åpnes fra `file://`.
Kjør `python3 -m http.server` i mappen og åpne `localhost:8000`.

---

## 7. Publisering — GitHub Pages

Netlify har fortsatt gratisplan, men den er lagt om til kreditter (300/mnd,
nettstedet pauses når de er brukt opp). GitHub Pages har ingen slik måler og
er valgt. Cloudflare Pages er et godt alternativ hvis CDN-hastighet blir viktig.

1. Opprett konto på github.com. Brukernavnet inngår i URL-en.
2. New repository → `london-nabolag` → **Public** (Pages på privat repo krever betalt plan).
3. Add file → Upload files. GeoJSON legges på `data/london.geojson` — skriv stien
   i filnavnfeltet, så lages mappen.
4. Settings → Pages → Deploy from a branch → `main` / `(root)` → Save.
5. Live etter ett–to minutter på `https://brukernavn.github.io/london-nabolag/`.
6. Egen adresse: CNAME-record i DNS + domenet under Settings → Pages.

Senere endringer gjøres i nettleseren: klikk filen, blyantikon, rediger, commit.
Live etter cirka 30 sekunder.

---

## 8. Neste steg i Cowork

1. ~~Flytt mappen inn i prosjektmappen.~~ Gjort.
2. ~~Bygg `index.html`, `style.css` og `app.js`.~~ Gjort.
3. Kjør lokalt og se over: `cd "~/Documents/Privat/Prosjekter/Nabolag London" && python3 -m http.server`,
   så `http://localhost:8000`.
4. Rett opp koordinatene i de to eksempel-linjene i geojson.io.
5. Opprett repo og publiser.
6. Vurder KML-eksport for My Maps-sporet.

## 9. Notater om implementasjonen

- Kategorifargene ligger i `KATEGORIER` øverst i `app.js`. Ny kategori = én linje der;
  filterknappen lages automatisk, og bare kategorier som faktisk finnes i dataene vises.
- Sonefiltrene leses også ut av dataene, så listen holder seg selv i synk.
- Listen grupperes på sone, favoritter først, så alfabetisk.
- Popup lenker videre til Google Maps — på `place_id` når det finnes, ellers på koordinat.
- Kartfliser: CARTO Voyager. Bytt URL-en i `app.js` til `.../light_all/...` for Positron.
- Leaflet lastes fra unpkg. Ingen API-nøkkel, ingen byggesteg.
