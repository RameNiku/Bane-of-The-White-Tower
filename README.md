# BÍLÁ VĚŽ — v0.2 (HTML / GitHub Pages)

## Co je hotové
- Věž má 123 metrů, tedy trojnásobek původních 41 metrů.
- 62 nepravidelně rozmístěných hlavních plošin a 39 doplňkových plovoucích kvádrů.
- Otevřený strop s hvězdami a 12 čtvercových červených lamp se skutečným červeným nasvícením.
- Pohled z první osoby, WASD, myš, Shift, double jump a **opakovaný wall jump / nekonečné šplhání**.
- Checkpointy, přezdívka, CREATE ROOM, pět místností, cooldown 60 sekund.
- Zobrazení ostatních hráčů jako hranolů s přezdívkou.

## A) Rychlé vyzkoušení bez Firebase
1. Nahraj `index.html` do kořene GitHub repozitáře (nahraď původní).
2. Otevři adresu svého GitHub Pages webu.
3. Zadej nick, klikni `CREATE ROOM`, potom `VSTOUPIT DO HRY`.
4. Můžeš otevřít druhou záložku stejného webu, zadat jiný nick a připojit se tlačítkem `JOIN`. Oba hráči se uvidí, pokud běží ve stejném prohlížeči / stejné origin-doméně.

**Pozor:** Bez Firebase jsou tyto místnosti *lokálním testem*, sdíleným přes `localStorage` a `BroadcastChannel`. Hráči na jiných počítačích je neuvidí.

## B) Online multiplayer pro všechny s odkazem (Firebase)
1. Jdi na https://console.firebase.google.com/ a vytvoř nový Firebase projekt.
2. V *Project settings* přidej Web app (`</>`). Firebase ti nabídne objekt `firebaseConfig`.
3. V **Authentication → Sign-in method** povol **Anonymous**.
4. V **Build → Realtime Database** vytvoř databázi (zvol region podle potřeby).
5. V kartě **Rules** vlož obsah dodaného `database.rules.json` a publikuj pravidla.
6. Otevři `index.html` v editoru, najdi konstantu `FIREBASE_CONFIG` v horní části skriptu. Doplň hodnoty z `firebaseConfig`:
   - `apiKey`
   - `authDomain`
   - `databaseURL` (z přehledu Realtime Database, často **není** uvedena v původním configu)
   - `projectId`
   - `appId`
7. Ulož upravený `index.html` do GitHub repozitáře. Ve výchozím webu teď uvidíš **ONLINE · Firebase připojeno**.
8. Pošli webový odkaz kamarádovi. Uvidíte stejné místnosti a společné pohyby hráčů.

*Veřejný Firebase web config není tajný klíč, ale databázi chrání právě správně nastavená Security Rules. Nepoužívej režim "test mode" s otevřenými pravidly.*

### Důležité meze prototypu
- Omezení CREATE ROOM je 60 sekund na anonymní identitu prohlížeče a je kontrolováno i pomocí databázových pravidel. Není odolné proti obcházení přes nové identity/anonymní profily; bez serverové autoritativní funkce nejde slíbit absolutní ochranu proti úmyslnému obcházení.
- Jedna místnost zabírá jeden z pěti slotů. Hostitel svou místnost uzavře tlačítkem `OPUSTIT MÍSTNOST`; při ztrátě jeho Firebase spojení se odebere pomocí `onDisconnect`.
- Místnosti nejsou trvale ukládané jako herní světy; mapa je deterministicky vytvořena ze seedu místnosti. Checkpoint se zatím ukládá jen na dobu aktuálního vstupu.
- Synchronizuje se pozice/otočení ostatních hráčů, **nikoli** fyzika mezi hráči, chat, inventáře ani společné skóre.
- Při náhlém výpadku během zakládání online místnosti může ve vzácném případě zůstat osiřelý slot, dokud ho nespravíš v databázi.
- Zobrazení a ovládání jsou navržené pro počítač s myší a klávesnicí. Internet je potřebný pro načtení Three.js.

## Ovládání
- **WASD**: pohyb
- **Myš**: pohled
- **Shift**: běh
- **Space**: skok, dvojskok; u stěny podržet = zachytit se, pustit = odraz
- **R**: návrat na poslední checkpoint
- **Esc**: pauza, možnost opustit místnost

## Jak nahrát na GitHub Pages
V repozitáři v **Settings → Pages** nastav **Deploy from a branch → main → / (root)**.
Při výchozím nastavení je adresa `https://TVE-JMENO.github.io/NAZEV-REPOZITARE/`.
