# Guess what - MVP

Egyfájlos HTML játék. Nincs backend, nincs adatgyűjtés, nincs build lépés.

---

## Fájlszerkezet

```
guess-what/
  index.html          ← minden itt van
  assets/
    hero.png          ← Gemini hero illusztráció (a "B" verzió)
    leaf.png          ← egy lóherelevél, átlátszó háttér (opcionális)
    og.png            ← LinkedIn preview kép, 1200×630
```

Ha az `assets/hero.png` még nincs meg, a helyén szaggatott keret jelenik meg -
a játék addig is működik.

---

## Élesítés

1. A `guess-what` mappát bemásolod a GitHub repo gyökerébe
2. `git add . && git commit -m "guess what" && git push`
3. Netlify magától deployol → **luckybastard.hu/guess-what/**

Ha rövidebb URL kell (`/guess`), nevezd át a mappát.

---

## Levél asset becserélése

A lóherelevél most inline SVG placeholder. A Gemini-rajzra cserélni egy sor:

`index.html`-ben keresd a `const LEAF =` sort, és írd át erre:

```js
const LEAF = `<img src="assets/leaf.png" alt="">`;
```

A levél PNG-nek átlátszó hátterűnek kell lennie, és a **nyél vége a képkocka
alsó közepén** - különben a négy levél nem talál össze. A CSS
`transform-origin: 50% 100%` értékkel forgatja őket 45/135/225/315 fokra.

A 12. levél terracottára váltása jelenleg CSS-ből megy (`.leaf.final.on svg path`).
Ha PNG-re cserélsz, ehhez kell egy második fájl: `leaf-final.png`.

---

## Analytics

A `<head>`-ben van egy kommentelt blokk. Két út:

**Plausible** - nem sütizik, nem kell cookie banner. Fizetős.
```html
<script defer data-domain="luckybastard.hu" src="https://plausible.io/js/script.js"></script>
```
Custom event-ekhez a Plausible felületén engedélyezni kell a `script.tagged-events` verziót.

**GA4** - ingyenes, de sütizik, tehát cookie banner kell hozzá (GDPR).

A `track()` függvény mindkettőt kiszolgálja, plusz a `dataLayer`-t. Ha egyik
sincs bekötve, csendben elmegy - nem hibázik.

### Mért események

| Event | Mikor | Property |
|---|---|---|
| `gw_start` | elindult a játék | - |
| `gw_pass` | helyes válasz | `q` = kérdés sorszáma |
| `gw_fail` | bukás | `q` = hányadiknál esett ki |
| `gw_continue` | "Tovább innen"-t választott | `q` = melyik kérdésnél |
| `gw_win` | mind a 12 megvan | `clean` = 1 ha hibátlan futam volt |
| `gw_claim` | jelentkezett az ebédre | - |

**Amit ezekből ki tudsz olvasni:** hol esnek ki a legtöbben (melyik kérdés túl
nehéz vagy félreírt), mennyi indításból lesz nyerés, és mennyi nyerésből lesz
tényleges e-mail. Az utolsó a lead funnel egyetlen érdekes száma.

---

## Tartalom szerkesztése

Minden a `const Q = [...]` tömbben van. Egy kérdés:

```js
{ said:"Érdekes.",
  opts:["első opció","második opció","harmadik opció"],
  correct:1 }
```

A `correct` a **helyes opció indexe**, nullától. A sorrend szándékosan kevert
- a helyes válasz négyszer az első, négyszer a második, négyszer a harmadik
pozícióban van, felismerhető minta nélkül.

Jelenlegi kulcs: `2,0,1,0,2,1,1,2,0,1,2,0`

---

## A nyeremény mechanikája

Nincs verseny, nincs időmérő, nincs becsületalap és nincs tárolt állapot.

1. Elrontod → két gomb: **Újra elölről** vagy **Tovább innen**
2. A `clean` flag bármelyik hibánál és a "Tovább innen"-nél hamisra vált
3. Csak a `clean === true` futam végén jelenik meg a jelentkező gomb
4. Minden futam a saját jogán számít - nincs localStorage, nincs sütizés
5. A jelentkezők közül Bence sorsol egyet

A záróképernyőnek két állapota van: `#winClean` (jár a jelentkezés) és
`#winDirty` (visszahívó: "Nekifutok tisztán").

---

## Következő edition

Négy dolgot kell átírni, semmi mást:

1. `--accent` a `:root`-ban → az edition saját kiemelőszíne
2. `the <em>client</em> was thinking` → `the <em>creative</em> was thinking`
3. a `Q` tömb tartalma
4. `assets/hero.png` → az új edition illusztrációja

A `Guess what` cím, a lóhere-mechanika, a copy szerkezete és az egész logika
változatlan marad.

---

## Ami még hátravan

- [x] `assets/hero.png` - a jóváhagyott B verzió (kibontva a Design bundle-ből)
- [x] Claude Designban vizuális finomhangolás (visszaportolva)
- [ ] `assets/leaf.png` - a lóherelevél, nyél az alsó közepén
- [ ] `assets/og.png` - 1200×630 LinkedIn preview
- [ ] `assets/favicon.png`
- [ ] analytics kód bekötése
- [ ] a mailto cím ellenőrzése (most: `javor.h.bence@gmail.com`)

---

## Mi jött a Claude Designból

A Design egy 368 KB-os bundle-t adott vissza (saját runtime, beépített
fontok, becsomagolt React amit nem is használt, és elveszett Open Graph
tagek). A vizuális munka jó volt, ezért visszaportoltuk ebbe a fájlba:

**Öt animáció:**

| Keyframe | Hol | Időzítés |
|---|---|---|
| `leafLand` | levél landolása | .52s, túllendülő easing |
| `missShake` | rossz válasz kártyája | .22s |
| `finalGlow` | a 12. levél a nyerésnél | .45s |
| `revealIn` | bukás/nyerés szövegblokkjai | .32s, lépcsőzetesen |
| `screenIn` | képernyőváltás | .32s |

**8px-es térköz skála** - `--s1`-től `--s6`-ig, a négy képernyőn
következetesen. Ez volt a legnagyobb rendrakás.

**Nagyobb cím** - a `Guess what` mostantól `clamp(60px,21vw,128px)`.

**Amit nem vettünk át:** a Design runtime-ja, a beépített woff2-k
(helyette Google Fonts), a React, és a bundler betöltő overlay.
Így a fájl 368 KB helyett 24 KB, és kézzel szerkeszthető maradt.
