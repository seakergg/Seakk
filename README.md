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


## Linkes meghívó és állásszerkesztő

- **Meghívó link:** Online fül → *Szoba nyitása* után a **Meghívó link** gomb a megosztó menüt nyitja (vagy vágólapra másol): `…/index.html?invite=KÓD`. A linkre kattintva a másik fél automatikusan a szobához csatlakozik, másolgatás nélkül (a régi `#r=KÓD` link is érvényes). A kapcsolódás jelzése a nyilvános ntfy.sh-n megy (fiók nélkül, csak a kapcsolódáskor), a játszma ezután közvetlenül a két eszköz között zajlik (WebRTC).
- **Állásszerkesztő:** Puzzle fül → *Állásszerkesztő*. Bábulerakás, törlés (🗑), áthelyezés (✥), tábla ürítése, alapállás, ki következik, sáncolási jogok, FEN másolása / betöltése, majd indítás a gép ellen vagy két játékossal, tetszőleges variánssal (normál, Király a hegyen, Három sakk, Atomi, Ködös). Indítás előtt ellenőrzi az állást (egy-egy király, gyalog a szélső soron, a nem soron lévő fél nincs sakkban).


## Egyedi játékmódok (Beállítások → Variáns)

- **Időugrás:** minden 10. lépés (fél-lépés) után a tábla minden bábuja egy sort csúszik a saját előre / hátra irányába (felváltva), ha a cél üres. Ha két bábu ugyanarra a mezőre csúszna, egyik sem mozdul, a gyalog nem csúszik az 1. / 8. sorra, az ugrás elmarad, ha a lépő fél királya sakkba kerülne. A tábla alatt visszaszámláló mutatja a következő ugrást. A gép tudja a szabályt.
- **Vak-zóna:** a Ködös sakk változata: a saját térfeled (az első 4 sor), a bábuid és az általuk elérhető / támadott mezők látszanak, az ellenfél hátsó sorai nem. Egy gépen átadó képernyő van.
- **Túlélő mód (hullámok):** egyjátékos. Egy alapcsapattal (király, 3 gyalog, huszár, futó) védekezel az ellenséges hullámok ellen. Egy hullám akkor ér véget, ha minden ellenséges bábut leütöttél (vagy az ellenséges királyt mattoltad). Utána 3 véletlen bábu közül választasz egyet jutalmul, majd erősebb és nagyobb hullám jön erősebb gép-szinttel. A futam akkor ér véget, ha mattot kapsz. A legjobb eredmény mentődik. Nem értékelt, nincs visszavonás és mentés.


## További egyedi változatok (Beállítások → Variáns)

- **Lélekvándorlás:** ütés után az ütő bábu a következő lépésedre az áldozat mozgását kapja (lila aura + a kölcsönzött bábu jele), majd visszaváltozik. A kölcsönzött képesség sakkot is adhat, és a gép is használja. Gyalog kölcsönzött képességgel nem léphet az alap- / ellenfél alapsorra.
- **Gravitációs sakk:** minden lépés után az összes bábu balra csúszik (az a-vonal felé), amíg bábu vagy a tábla széle meg nem állítja. Sáncolás és en passant nincs, a csúszás animált.
- **Kráter-sakk:** minden ütés helye 3 lépésig kráterré válik (lila gödör, a hátralévő körökkel): nem léphetsz rá, és a futó / bástya / vezér nem tud átmenni rajta (a sakkot is blokkolja).
- **Királyi testőrség:** nincs sakk és matt, a király leüthető. Mindkét fél VIP-je a vezére (arany korona). Nyer, aki a VIP-jét az ellenfél alapsorára juttatja, vagy leüti az ellenfél VIP-jét.
- **Időzített bomba:** mindkét félnek egy véletlen bábuján 5 körös bomba ül (💣). Ha a hordozóval ütsz, a számláló újraindul, ha új saját bábu mellé lépsz vele, átadod neki a bombát (újraindul), különben az 5. lépés végén felrobban a 3×3-as területen (a királyok kivételével, a sajátokat is viszi). Az ellenfél hordozóját leütve hatástalanítod. Online is szinkronban indul.
- **Tükör-sakk:** minden harmadik fél-lépésnél (3., 6., 9., … felváltva világos és sötét) ugyanolyan típusú bábuval kell lépned, mint amilyennel az ellenfél az előző lépésben lépett, ha van ilyen szabályos lépésed.


## Reversi-sakk és Biliárd-sakk

- **Reversi-sakk:** ha egy lépéssel (vagy ütéssel) ellenséges tiszteket (huszár, futó, bástya, vezér) két saját bábud közé szorítasz egy vonalon (sor, oszlop, átló), azok **átállnak a te színedre** (kék szilánkos átfordulás). A gyalog és a király nem fordítható, és ha egy sorban gyalog vagy király áll, vagy rés van, vagy a sor a tábla szélén ér véget, nincs átfordítás. Az átfordítás sakkot is feloldhat, a sáncjog elvész, ha a sarokbástya átáll.
- **Biliárd-sakk:** minden ütésnél a célmező szomszédos bábui (a királyok kivételével, a sajátjaid is) **egy mezővel távolodnak** az ütés helyétől, ha oda szabad a mező. A tábla szélén túlra lökött bábu **leesik**. Gyalog nem lökődik az alap- vagy ellenfél alapsorra. Az ütő bábu marad a helyén, lökéshullám-gyűrűvel.
