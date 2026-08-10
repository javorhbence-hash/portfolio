# Handoff prompt - Claude Design

Másold be az alábbi szöveget, és csatold mellé az `index.html` fájlt.

---

```
Csatolok egy kész, működő egyfájlos HTML játékot. Vizuális finomhangolást
kérek rajta - nem újratervezést.

MI EZ
"Guess what the client was thinking" - egy 12 kérdéses tippelős játék egy
freelance kreatívigazgató (You lucky bastard / YLB) saját weboldalára.
Tizenkét mondat, amit ügyfelektől lehet hallani; a játékos tippel, hogy
melyiket mire értették valójában. Egy hiba, és kezdi elölről.

NÉGY KÉPERNYŐ
1. Landing - krém háttér. YLB jel, nagy "Guess what" cím, alcím,
   ceruzarajzos hero illusztráció, rövid felvezető, indítógomb.
2. Játék - FEKETE háttér. Fent a lóhere progress, középen egy nagy idézet,
   alul három válaszkártya. Kitölti a viewportot, nincs görgetés.
3. Bukás - fekete. Mit mondott / mire gondolt, plusz egy kiértékelő mondat.
4. Nyerés - fekete. 12/12, majd egy mailto gomb.

============================================================
AMIHEZ NE NYÚLJ - ezek végleges döntések
============================================================

- A magyar szövegek. Egyetlen szót se írj át, ne fordíts, ne javíts.
- A Q tömb tartalma és a correct indexek. A helyes válaszok pozíciója
  szándékosan van elosztva; ha átrendezed, elromlik a játék.
- Színek: cream #F5F0E8, ink #1A1A18, terracotta #C4472A, plusz a sötét
  háttérre világosított #DD6440. Ötödik szín nincs. Gradiens nincs.
- Betűtípusok: Playfair Display 900 és Jost. Harmadikat ne hozz be.
- A fordított mód SZÁNDÉKOS. A landing krém, a játék fekete. Ne
  világosítsd vissza, és ne sötétítsd el a landinget.
- A játékmechanika: 12 kérdés, egy hiba és elölről kezdi.
- A track() analytics hívások és a mailto link.
- Nincs build lépés, nincs framework, nincs npm. Egyetlen HTML fájl
  marad, külső függőség nélkül - a Google Fonts az egyetlen kivétel.

============================================================
AMIN DOLGOZZ - fontossági sorrendben
============================================================

1. A ROSSZ VÁLASZ PILLANATA
   Ezt fogják a legtöbbször látni, ezért ez érdemli a legtöbb figyelmet.
   Most: a kiválasztott kártya kap egy .miss osztályt, 620 ms múlva jön a
   reveal képernyő. Ez működik, de nyers. Kellene egy rendes dramaturgia:
   a kártya reagál, a képernyő átvált, a reveal érkezik. Három ütem,
   nem egy. Ne legyen hosszabb összesen 1 másodpercnél.

2. A LÓHERELEVELEK LANDOLÁSA
   Most csak opacity-t váltanak egy pop scale-lel. Legyen valódi mozgás:
   a levél felülről érkezik, enyhén túllendül, beáll a helyére.
   Egyesével, nem egyszerre. A 12. levél terracottára vált - ez a
   győzelem pillanata, most alig látszik.

3. A GYŐZELMI KÉPERNYŐ
   Jelenleg funkcionális, de nem esemény. Ennek kell a legjobban esnie:
   a 12. levél színt vált, aztán érkezik a szöveg. Visszafogottan -
   semmi konfetti, semmi ünneplés. A brand hangja száraz.

4. VERTIKÁLIS RITMUS
   A négy képernyő térközei most nem egységesek. Rakj rendet: egy közös
   spacing skála, következetesen alkalmazva.

5. A VÁLASZKÁRTYÁK ÁLLAPOTAI
   Alap, hover, lenyomott, letiltott. Diszkréten - keret és szín, semmi
   árnyék, semmi emelkedés.

6. A LANDING TIPOGRÁFIAI ARÁNYAI
   A "Guess what" most clamp(58px, 20vw, 92px). Nézd meg 380px-en és
   1200px-en is. A cím és az alcím közti léptékkülönbség a lényeg -
   ha kell, feszítsd tovább.

============================================================
KERETEK
============================================================

MOBILE FIRST. 380px szélességtől tervezz. A játékképernyőn semmiképp ne
legyen görgetés egyetlen breakpointon sem.

A HANG: száraz, visszafogott, magabiztos. Ez egy reklámszakmai belsős
vicc, nem egy vidám kvízapp. Ha valami játékosnak vagy cukinak érződik,
az rossz irány. A referencia inkább egy jól tervezett társasjáték doboza,
mint egy mobiljáték felülete.

MOZGÁS: minden animáció 400 ms alatt. Nincs bounce, nincs rugózás,
kivéve a lóherelevél landolását. A tempó legyen inkább lassú és
magabiztos, mint pörgős.

ILLUSZTRÁCIÓK: a hero egy ceruzarajz, ami az assets/hero.png fájlból
töltődik. Ha nincs ott, egy szaggatott keret látszik helyette - ez
normális, ne javítsd ki. A lóherelevél jelenleg inline SVG placeholder,
később PNG-re cserélődik - a szerkezetet hagyd cserélhetőnek.

A VÉGEREDMÉNY egyetlen index.html fájl legyen, ugyanazzal a szerkezettel:
CSS változók a tetején egy blokkban, a tartalom egyetlen Q tömbben,
a logika alul.
```

---

## Amit érdemes utána visszaellenőrizni

- [ ] A `Q` tömb `correct` értékei változatlanok: `1,2,0,2,0,1,2,0,1,1,2,0`
- [ ] A `track()` hívások megvannak mind az öt eseményre
- [ ] A mailto link él és a jó címre megy
- [ ] A landing krém maradt, a játék fekete
- [ ] Nincs görgetés a kérdésképernyőn 380px-en
- [ ] Egyetlen fájl, nincs új külső függőség
