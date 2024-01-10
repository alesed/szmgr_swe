# Programming and software development

> Development of software solutions, specifics of implementing web-based information systems. Tools and environments for software development of large-scale systems. Basic concepts of software architectures from an implementation perspective. Multilayer architecture of modern information systems, model-view-controller architecture. Persistence, ORM. Practical examples for all of the above. (PA165 || PV179)

## Development of software solutions

- Development se sklada z nekolika kroku (pripomina waterfall model):

  - Zvoleni metodologie, ve ktere bude development probihat (**Agile, RAD, Waterfall**)
  - **(Analysis)** Sber pozadavku - Existujici system? Potreby stakeholderu, Pozadovane standardy, Regulace zakonne, Domain informace
  - **(Design)** Navrhnuti designu
  - **(Coding)** Implementace designu
  - **(Testing)** Testovani systemu - Akceptacni testovani, integracni testovani, unit testovani, performance testovani, sanity testovani (po uplatneni patche), smoke testovani (core features funguji), ...
  - **(Deployment)** Nasazeni systemu
  - Optional: Migrace dat - pokud existuje stara aplikace, migrace z existujiciho softwaru do noveho

- Dulezite je komunikace mezi klientem/zakaznikem a developerem, potreba shodnuti se na jednotnem/univerzalnim jazyku (Ubiquitous language)

  - V momente, kdy nedochazi k dodrzovani muze dochazet k neporozumneni

- U navrhu je dulezite po sberu vsech pozadavku definovat design (treba forma UML modelu), dale navrhnout public/internal APIs, definovat datovy model, uvazovat nad infrastrukturou (je treba premyslet nad rychlosti, bezpecnosti, skalovatelnosti)

  - Dokompozice na komponenty, co nejjednodussi system -> minimalizace zavislosti mezi jednotlivyma komponentama, dodrzovani **SOLID, DRY, KISS**
  - Komponenty maji dobre navrzene kontrakty (interfaces), dulezita je zavislost na kontraktech, ne na implementacich (**Dependency Inversion principle**)

- Nasazeni a kontonialni integrace systemu s vyuzitim (**CI/CD**), spousteni automatizovanych testu, obecne verzovani kodu

## Specifics of implementing web-based information systems

- Webove aplikace jsou vetsinou **SaaS** (Software-as-a-service) aplikace
- Jsou nezavisle na zarizeni (pc, notebook, tablet, phone, hodinky, tv)
- Vetsinou se skladaji z vicero casti:
  - **Frontend:** Pristupny skrze prohlizec (HTML, CSS, JavaScript)
  - **Backend:**
  - **DB:**

### Security

- Authentication - validace identity (prihlaseni)
- Authorization - specifikace prav (access rights)

### Attacks

- **SQL injection**: utocnik vlozi jako input do SQL dotazu specialni kombinaci k zpristupneni dat/smaze data/upravi data/prida data (noveho usera s adminem..)
- **Session hijacking**: ziskani cookies nekoho jineho a zneuziti
  - **obrana** je "Secure" komunikace skrze **HTTPS** protokol (pouziti **SSL/TLS** encryption)
- **Session fixation** poslani URL se session id, obet se prihlasi, od te doby je tato session ID authenticovana, utocnik se muze jednoduchou zmenou cookies prihlasit
  - **obrana** je neakceptovat URL s podezrelyma parametrama
- **XSS (Cross site scripting):** utocnik injectuje client-side skript do webu, ktery si prohlizi obet
  - **obrana** je kontrola URL, zda neobsahuje `javascript:`, sanitizace HTML inputu
- **XSRF (Cross site request forgery):** ![XSRF example](../images/xsrf.png)
  - **obrana** je two-factor authentication, upraveni cookies `SameSite=Strict` nebo mit jednorazovy token
- **Clickjacking:** reklamy na footybite, kliknes na play a otevre se ti 42x XXX stranky
  - **obrana** je uprava HTTP headeru (`X-Frame-Options`)
- **Phishing:** utocnik se vydava za "trustworthy" identitu, typicky posle mail s odkazem na fake stranky vypadajici jako ty opravdove, URL je podobne, na prvni pohled nerozeznatelne
  - **obrana** je trenink uzivatelu k rozpoznani phishing attemptu

## Tools and enviromnents for software development of large-scale systems

## Basic concepts of software architectures from an implementation perspective

## Multilayer architecture of modern information systems, model-view-controller architecture

## Persistence, ORM
