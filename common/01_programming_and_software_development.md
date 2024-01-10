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

## Tools and enviromnents for software development of large-scale systems

## Basic concepts of software architectures from an implementation perspective

## Multilayer architecture of modern information systems, model-view-controller architecture

## Persistence, ORM
