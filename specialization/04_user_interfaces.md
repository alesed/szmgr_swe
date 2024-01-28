# User interfaces

> Principles of user interface design and development in modern software systems, including web, mobile. User interface development process and quality principles. User experience (UX), interaction design, prototyping, wireframing, user research, usability testing. Technologies and tools. Practical examples for all of the above. (PV182 || PV247 || PV278)

## Principles of user interface design and development in modern software systems, including web, mobile

**dobry UI:**

- uzivatelska privetivost
- snadne pouziti rozhrani
- jednoduchy system (naslapany system plny funkci muze byt overwhelming a nebude intuitivni)
- bezni uzivatele s dobrym UI dokazou prominout i nektere nedostatky

**Navrh muze ovlivnovat:**

- interni vs verejny system? (musi s nim uzivatele pracovat nebo pracuji jen kdyz chteji?)
- jaka je odbornost uzivatelu?
- jake jsou moznosti prizpusobeni?
- nutnost vicejazycne aplikace (potrebujeme font, ktery podporuje vsechny mozne specialni znaky)
- nutnost pristupnost (accessibility) pro uzivatele s omezenim (zrakove, sluchove nebo jine specificke potreby)
  - muzeme se navigovat v systemu pomoci mysi nebo jen klavesnici?

Obvykle se UI vyviji jako monolit, muzeme ale delat i microfrontendy.

**Brat v potaz:**

- uzivatelskou privetivost (slozite formulare delit na vicero casti, top-down pole, scroll nejlepe jen vertikalni)
- bezpecnost - aplikace, na ktere mohou byt cileny utoky (phishing) je dobre prizpusobit per user (jeho fotka, jeho unikatni informace - id/kontakt/bezpecnostni otazka?)
  - snazit se o to, aby bylo stranku tezke napodobit
  - skryvat hesla
- pouzivani vhodnych input poli a dalsich komponent
  - omezit mnozinu hodnot, ktere lze vypsat
  - omezit hodnoty na dropdown list
  - checkboxy
  - radio buttony
  - textarea
  - buttons
- jednotne rozhrani (tvorba komponentu a reusovani)
- responzivita
  - osetrit rozlozeni na ruznych pristrojich (pc, tablet, mobil, hodinky, tv, ...)

Nejuniverzanelsi pristup je HTML+CSS+JS

Vyuzivani popularnich frontend frameworku jako je React, Vue, Solid, Svelte, Angular, ktere lze pouzit v prohlizeci

Pro desktopove aplikace zalozene na webovych technologiich kuprikladu Electron - pribali chromium, nebo treba Tauri

Pro mobilni aplikace portovane z browser aplikaci lze vyuzit PWA (progressive web app, neni nutne instalovat z app storu, ale pouze vytvorit odkaz na stranku - tvari se jako appka)

**WebAssembly** - trend - umoznuje pouzivat kompilovany jazyk v prohlizeci, zpravidla rychlejsi nez JS. WASM podporuje 2 mody:

- praci s DOM
- vykreslovani na canvas (Figma)

Dalsi alternativou jsou multiplatformni frameworky jako Flutter, pouzivajici Dart jazyk a podporuje vytvareni apps do android, ios a webu najednou

- jeden kod = 3 ruzna zarizeni

Nativni jazyky pro mobilni aplikace jsou pak Swift (IOS) a Java/Kotlin (Android)

**Specifika pro web:**

- kdy je stranka renderovana?
  - **SSR** (Server-side rendering) - plne renderovana na serveru (vhodne pro staticke aplikace)
  - **CSR** (Client-side rendering) - plne renderovana na strane klienta, data jsou do aplikace dotazena skrze API requests ze serveru
- v minulosti jsme meli **multi-page** apps (nova stranka = novy page load)
- momentalne je trend **single-page** app (mutace DOMu)
- muzeme pristupy michat (initial load je SSR a potom CSR only) - vhodne pro SEO roboty, Lighthouse analyzu
  - tento pristup vyuziva treba Next.js

## User interface development process and quality principles

- uzivatelsky vyzkum
  - pochopit skutecne pozadavky na UI
- navrh wireframu a prototypu
  - vyuziti pro zpetnou vazbu a predloha pro vyvojovou praci
- na konec udelat uzivatelske testovani s ruznymi typy uzivatelu (ruzne role)
  - mitigace problemy pri nasazeni

### User research and analysis

- kdo jsou nasi uzivatele - jak pracuji se stavajicim systemem? co jim vyhovuje? co ne?
- jake jsou pozadavky na system? jak se pouziva? (minimalizace kroku pro provedeni frekventovane operace)
- diskuze s uzivateli pomoci **focus groups**
  - skupina rozdilnych uzivatelu
  - moderovana diskuze
  - ziskani feedbacku, nazoru, moznych vylepseni
- pouzivani A/B testovani - kazde skupine uzivatelu prezentujeme rozdilnou variantu prodkutu a sledujeme vliv

**Interaction design:**

- snazime se pochopit, jak uzivatele pracuji a interaguji se systemem a uzpusobit tomu uzivatelske rozhrani

**Wireframes:**

- navrh jednotlivych komponent rozhrani (obrazovky)
- neresime barevnou paletu
- layout a hrube rozlozeni obsahu

**Prototypovani:**

- navrh pomoci nastroju jako je AdobeXD nebo Figma
- lze vytvorit i interaktivni prototyp
- lze generovat kod z prototypu
- lze pouzit pro uzivatelske testovani

## User experience (UX)

- **UI vs UX**
  - UI
    - vizualni elementy, vse s cim muze uzivatel interagovat
    - aestetika a prezentace produktu
  - UX
    - celkova zkusenost uzivatele s produktem
    - pohodlna interakce uzivatele se systemem
    - celkova pouzitelnost produktu
    - zaklada si na bezproblemovem a prijemne interakci na zaklade potreb/ocekavani

UX je kombinace aspektu UI:

- vizualni krasa
- pouzitelnost (accessibility)
- uzitnost (splnuje vsechny potreby)
- efektivita
  - uzivatel neni nucen delat veci, ktere nepotrebuje nebo nemusi zadavat znamou informaci vicekrat
  - caste akce maji svou klavesovou zkratku

UX urcuje, zda budou chtit uzivatele nas produkt pouzivat

## Usability testing

Zahrnuje:

- pozorovani uzivatelu pri provadeni ukonu v ramci systemu
  - sledovani akci, reakci, potizi
  - na zaklade sledovani navrzeni reseni
- evaluace experty z oblasti pristupnosti (accessibility)
  - vyuziti nastroje jako je Lighthouse
- sledovani oci uzivatele (nebo mysi), zjistime tak, jake casti rozhrani nejvice upoutavaji pozornost
  - potreba souhlas uzivatele
  - tzv heatmap
