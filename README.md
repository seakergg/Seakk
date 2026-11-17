# Seakk

Önálló sakkalkalmazás (HTML + JS). Webes (GitHub Pages) és Android (APK) változat ugyanabból a kódból.

## GitHub Pages (weboldal és PWA)
1. Hozz létre egy GitHub tárolót, és töltsd fel ennek a mappának a tartalmát (a `.github` mappával együtt).
2. Settings > Pages > **Source: GitHub Actions**.
3. A `main` ágra érkező push automatikusan közzéteszi az oldalt: `https://FELHASZNÁLÓ.github.io/TÁROLÓ/`.
   Minden közzététel új gyorsítótár-verziót kap, így a telepített PWA a következő megnyitáskor frissül.
4. A cím megadható a pwabuilder.com-on, és telefonon a böngészőből is telepíthető („Hozzáadás a kezdőképernyőhöz”).

## APK készítése
Actions > **Android APK** > Run workflow. A futás végén az Artifacts között találod az `app-debug.apk` fájlt.
`v1.0`-hoz hasonló címke esetén az APK a Releases oldalra is felkerül. A debug APK telepíthető, de nem Play Áruházba szánt.

## Lichess-bejelentkezés (OAuth)
Nincs regisztráció és titkos kulcs, az app PKCE-t használ. A weben a visszatérési cím az oldal saját címe (a Pages-cím),
az APK-ban a `hu.seakk.app://oauth` (a workflow beállítja az AndroidManifest.xml-ben).

## Értesítések
A böngészőben az app az ablak elrejtett állapotában rendszerértesítést küld (engedély kell hozzá).
Az APK-ban a napi emlékeztető (18:00) a `@capacitor/local-notifications` bővítménnyel ütemezett. Szerverről érkező (push) értesítés nincs.


## Variánsok

A Beállítások → *Variáns* menüben választhatók (nem értékelt játszmák):

- **Chess960**: véletlen alapállás, a sáncolás a 960-as szabályok szerint (a király a bástya mezőjére kattintva vagy a célmezőre kattintva sáncol).
- **Teleport sakk**: minden játékos egyszer a játszmában bármelyik bábuját (a királyt is) bármelyik üres mezőre teheti. A teleportáló lépés nem üthet, és nem adhat sakkot. Csak ebben a variánsban van jelen. A gép nem teleportál.
- **Hendikep**: a gép vagy te vezér / bástya (a-vonal) / huszár (b) / f-gyalog nélkül kezdesz.
- **Háromszemélyes hatszögsakk** (a zoldsakk.hu szabályai alapján): 96 mezős hatszögtábla, fehér → piros → fekete sorrend, az első matt véget vet a játéknak. 1 ember + 2 gép vagy 3 ember egy készüléken. Nincs en passant, a gyalog automatikusan vezérré változik.

## Animációk

Minden stílusnak saját effektkészlete van (nyomvonal, leérkezés, ütés, a leütött bábu eltűnése, sakk-gyűrű, matt, átváltozás, konfetti): neon = pixelek és glitch, kristály = szilánkok, erdő = levelek és hervadás, naplemente = fényszikrák, rózsa = szirmok, homok = por és omlás, jég = hópelyhek és fagyás, láva = parázs és égés. A sáncolásnál a bástya célmezején a logó stílusú (kék/fehér/ezüst, világító gyűrűs) kastély nő ki a földből.


## Minijátékok (Feladvány fül)

- **Guess the Elo:** a gép két azonos erősségű (500–2000 Elo) játékos között legenerál egy ~22 lépéses játszmát, te pedig végignézed (léptetés, lejátszás, lépéslistára kattintás), majd csúszkával tippelsz a szintre. A pontszám az eltéréstől függ (0 eltérés = 100 pont), az összesítés mentődik.
- **Nyitás-rulett:** pörgethető kerék a beépített megnyitásokból. A kisorsolt állásból választott színnel (vagy véletlenül) játszol a gép ellen az aktuális szinten, nem értékelt. A megnyitás neve a táblánál is látszik.


## További variánsok (Beállítások → Variáns)

- **Király a hegyen:** a matt mellett az nyer, akinek a királya a d4, d5, e4 vagy e5 mezőre lép (a négy mező aranyszínnel jelölt).
- **Három sakk:** a tábla alatt mindkét fél sakkszámlálója (♔ 0/3 · ♚ 0/3), aki először ad 3 sakkot, nyer.
- **Atomi sakk:** ütéskor a célmező és a 8 szomszédja megsemmisül (a szomszédos gyalogok túlélik), a király nem üthet, az ellenfél királyának felrobbantása azonnali győzelem, szomszédos királyok nincsenek sakkban. Robbanás-animációval.
- **Ködös sakk:** csak a saját bábuk, az általuk elérhető mezők és a gyalogok átlós mezői látszanak, a többi sötét, az ott álló ellenséges bábuk rejtettek. Egy gépen (2 játékos) lépés után „Következő játékos” átadó képernyő jelenik meg. A lépéslista és a tipp a játszma végéig rejtett.

Mind a négy működik a gép ellen és online szobában is, nem értékelt játszma, és mentés / folytatás / PGN-export is kezeli.
