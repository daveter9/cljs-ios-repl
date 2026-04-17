# ClojureScript REPL — Offline PWA voor iPad

Een volledig offline ClojureScript REPL die je als app op je iPad
installeert via Safari. Geen App Store, geen handmatige downloads:
de app haalt [Scittle](https://github.com/babashka/scittle) zelf op
tijdens de eerste start en bewaart het permanent in de browser-cache.

## Hoe het werkt

- **Eerste start (online)**: de app laat een init-scherm zien met
  voortgangsbalk terwijl Scittle (~2 MB) wordt gedownload van jsDelivr
  en opgeslagen in de Cache API.
- **Daarna (ook offline)**: Scittle komt uit de cache, de init is
  onzichtbaar snel, en alles werkt zonder internet.

## Features

- ClojureScript evaluatie in de browser (via Scittle)
- Installeerbaar als PWA op iOS/iPadOS/Android/desktop
- Donker thema, touch-vriendelijke UI
- Snippet-knoppen voor veelgebruikte expressies
- Input history (pijltje omhoog)
- Cmd/Ctrl + Enter om te evalueren
- Netwerkstatus-indicator (online / offline / offline-ready)

## Bestanden

```
index.html       ← de app (UI + init-logica)
sw.js            ← service worker (cachet app shell)
manifest.json    ← PWA manifest
icon-192.png     ← app icon
icon-512.png     ← app icon (hoge resolutie)
README.md        ← deze file
```

## Installatie — 2 stappen

### Stap 1: Hosten via HTTPS

iOS vereist HTTPS voor PWA-installatie en Service Workers. Kies één
van deze gratis opties:

**Netlify Drop** (snelst, geen account):
1. Ga naar <https://app.netlify.com/drop>
2. Sleep deze map erheen
3. Je krijgt direct een HTTPS URL

**GitHub Pages**:
1. Maak een nieuwe repo, upload alle bestanden
2. Settings → Pages → Deploy from branch `main`, folder `/`
3. Beschikbaar op `https://<user>.github.io/<repo>/`

**Cloudflare Pages**, **Vercel**, **Surge** — werken ook.

### Stap 2: Op iPad installeren

1. Open de URL in **Safari**
2. Wacht tot het init-scherm klaar is (download loopt één keer)
3. Tik op de deel-knop → **"Zet op beginscherm"**
4. De app staat nu als icoon op je beginscherm

Vanaf dat moment werkt alles offline — probeer maar eens vliegtuigmodus.

## Lokaal testen

```bash
python3 -m http.server 8080
```

Daarna <http://localhost:8080>. Service Workers werken op `localhost`
en HTTPS, maar niet op `file://`, dus je moet echt een server draaien.

## Scittle updaten

Wijzig `SCITTLE_VERSION` bovenaan in `index.html`. Verhoog ook
`CACHE_NAME` naar `v2`, `v3` enz. zodat de oude versie wordt
weggegooid.

## Beperkingen

Scittle is een **ClojureScript interpreter**, geen JVM:

- ✅ `clojure.core`, data structuren, recursie, higher-order functies,
  macros, atoms, destructuring, threading macros, for, reduce-kv, …
- ❌ Java-interop, `clojure.java.*`, bestandssysteem-toegang
- ❌ Externe libraries via `require` (tenzij je ze ook inlinet)

Voor leren, experimenteren en kleine scripts is het prima. Voor
serieuze projecten gebruik je alsnog een echte dev-omgeving op
je laptop.
