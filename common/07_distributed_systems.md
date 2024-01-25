# Distributed Systems

> Basic concepts, principles. Difference between centralized and distributed system architecture, disadvantages of both and how to overcome them. Replication, data sharing. Service-oriented architecture (SOA), web services. Examples of existing technologies and their use. Practical examples for all of the above. (PA053)

[PA053 prednasky](https://is.muni.cz/auth/el/fi/jaro2023/PA053/um/)

## Basic concepts, principles

- **Middleware**

  - vrstva SW poskytujici rozhrani pro interakci s ruznymi sluzbami/systemy
  - abstrakce k casto pouzivane funkcionalite
  - vrstva propojujici systemy

- **Distribuovany system**
  - sklada se z komponentu (pocitacu/uzlu) propojenych v komunikacni siti
  - resi problemy/vypocty/zpracovavani requestu spolupraci jednotlivych komponent
  - snadnejsi skalovani
- **Monoliticka architektura**
  - obsahuje vse, co system potrebuje
  - mozne vertikalni skalovani
  - spatna spolehlivost (pad casti = pad systemu)
- **Tiered architektura**
  - jednotlive urovne lze distribuovat/paralelizovat/nahradit
  - komunikace urovni skrze API
  - klient muze byt tenky/tlusty dle poskytnute funkcionality
  - klasicky byvaji urovne:
    - klient
    - server
    - databaze
- **Hexagonal/Microkernel/Component-based architektura**
  - zakladni aplikace poskytuje minimum funkcionality
  - zbytek plug-in skrze komponenty komunikujici pred interfaces (preddefinovane API)
  - komponenty lze pripojovat za behu (VSCode a jeho extensions)
  - legacy system lze dale vyuzit diky komponentam (neni treba prepisovat - levnejsi)
    - komponentou ziskame dostupnost k legacy systemu skrz definovane rozhrani
  - vyvoj komponent je narocnejsi (potreba spravne zvolit rozhrani/interface)
  - ale umoznuje vetzi prizpusobitelnost, znovupouzitelnost
- **Pipeline architektura**
  - sekvencni zpracovani
  - kazda komponenta se stara o svou cast transformace vstupu na vystup
  - delej malou vec, ale delej ji dobre
- **[Service-oriented architektura](#service-oriented-architecture-soa)**
- **Microservice architektura**
  - vysoka koheze (soudrznost)
  - nizka provazanost jednotlivych sluzeb
  - velke mnozstvi malych sluzeb
  - dulezita je rychla komunikace mezi sluzbami (gRPC)
  - **nesdili se databaze**
- **Remote Procedure Call**
  - umoznuje spousteni predem vystavene metody/procedury mezi procesy (i na remote procesu)
    - jako kdybychom metodu volali primo v kodu
  - implementace procedur muze byt v rozlisnem jazyce, nez v jazyce, ve kterem je volana
  - soucasti je definice rozhrani
    - z neho generujeme odpovidajici funkce/struktury v nasem jazyce
  - de-facto standardem je gRPC
  - pro fullstack lze vyuzit tRPC
  - COBRA na podobny zpusob, ale kvuli slozitosti je obsolete
  - radsi se vyuziva SOAP a REST nebo RPC
- **Message queues & event brokers**
  - umoznuji komunikaci typu publisher-subscriber nebo zpracovani jednim z mnoziny prijemcu
  - schopnost perzistentne uchovavat zpravy (transakcni zpracovani, vyhoda pri vypadku systemu)
  - zprava do queue = klasicka fronta, zpracovani jednim konzumentem
  - topic = zprava jde vsem subscriberum
- **REST**
  - low-level kontrola
  - vysoky vykon
  - prima komunikace mezi sockety
- **Cloud**
  - vyuzivame platformu/infrastrukturu jako sluzbu
  - neni treba se o skoro nic starat
  - mitigujeme nakladnou investici
  - vypocetni vykon lze upravovat/skalovat podle aktualniho vytizeni
  - fyzicke zdroje mohou byt sdilene (nizsi cena/naklady)
    - distribuce vypocetni techniky podle usage
  - datova centra lze volit dle latence
- **GRID computing**
  - vypocet velmi narocnych uloh
  - velke mnozstvi zdroju (Folding@home)
- **Batch vs Stream processing**
  - Batch = zpracovani nekolika requestu/pozadavku najednou
    - distribuce pomoci jobs
  - Stream = zpracovani per item
- **MapReduce**
  - transformace dat pomoci map (1:1) a reduce (N:1)
  - map lze paralelizovat

## Difference between centralized and distributed system architecture

- **centralizovana**:
  - PROS:
    - shromazduje data a logiku na jednom miste
  - CONS:
    - horizontalni skalovani je no-go
    - selhani casti = selhani celku
    - nizka flexibilita, vysoka provazanost
- **distribuovana**:

  - PROS:
    - rozptyluje logiku do vice samostatnych komponent (muzou byt samostatne stroje, komunikace mezi sebou)
  - CONS:

    - komplexita celkoveho systemu
    - narocnejsi sprava
    - slozitejsi komunikace
    - slozitejsi synchronizace
    - nachylnost na latenci

  - NAVIC:
    - distribuovane systemy nebyvaji pozadavky/transakce ACID, ale BASE (Basically available)
      - **BAsically available** - nefunkčnost části nezpůsobí nefunkčnost celku, zbytek funguje i v případě nefunkční části systému. e.g. na netflixu nemusí fungovat služba hledání, ale vše ostatní běží v cajku. Na každý dotaz dostaneme nějakou odpověď.
      - **Soft state** - změny v systému mohou nastávat i když nepřichází žádné dotazy - systém takto propaguje data, aby dosáhl konzistence
      - **Eventually consistent** - data nemusí být konzistentní okamžitě po získání odpovědi na dotaz, ale až po nějaké chvíli
    - selhani distribuovaneho systemu neznamena pad celku
    - flexibilnejsi na modifikaci diky nizke provazanosti

## Replication, data sharing

- replikace u distribuovanych systemu
  - bezpecnost
  - dostupnost
  - prevence vypadku
  - obecne se snazime zajistit **rychlejsi odezvu**
- centralni databaze se muze stat bottleneckem
  - budto replikace databaze interne nebo vicero databazi
  - u replikace je potreba resit invalidaci dat po zmene
  - mozne je i vyuziti cache (redis)
- replikace kyzena u CDN (content delivery network)
  - staticke zdroje se snazime mit co nejblize prijemci pro snizeni latence
- noSQL databaze maji mechanismy pro automatickou replikaci/distribuci dat
  - **sharding** = rozbijeme data, kazdy uzel se stara o svou domenu
- **master-slave** replikace pro skalovani (master write permissions, slave read-only)

## Service-oriented architecture (SOA)

- hybrid mezi microservices a monolitem
- vzajemne nezavisle, samostatne nasaditelne a vzajemne komunikujici sluzby
- kazda sluzba ma ucelenou jednotku funkcionality
- kazda sluzba muze byt samostatne skalovatelna
- jednotne API pro vicero sluzeb = facade
- sluzby obvykle sdili DB
- narocnejsi na vykon
- ALE udrzitelnejsi a skalovatelnejsi diky modularizaci

## Web Services

- komponenty umoznujici komunikaci prostrednictvim standardizovanych protokolu a formatu
- zalozeny na service-oriented architekture
- abstrakce funkcionality skrze webove API
- definicni jazyk = formalne popsano schema sluzby
- pote muzeme vygenerovat klientsky kod pro ruzne programovaci jazyky
- schema muze byt generovano primo ze zdrojoveho kodu pomoci anotaci
  - @Controller
  - @Route
- Drive se pouzivaly web services zalozene na **SOAP** (simple object access protocol) a **XML**, definovane za pomoci **WSDL** (web service definition language)
- aktualne prevlada **REST** (representational state transfer), **JSON** (ale muzeme pouzit jakykoliv format), definovane za pomoci **OpenAPI** specification
  - NENI TO PROTOKOL, ALE ARCHITEKTONICKY STYL!
- nebo **GraphQL**
- **SOAP**
  - je nezavisly na transportu
  - jedna zprava umoznena cilit vice prijemcum
  - prechod pres prostredniky
- **REST**
  - vyuziva HTTP
  - jednodussi, rychlejsi a efektivnejsi
  - diva se na web jako na zdroje adresovatelne pomoci URL
    - vraci reprezentaci dat (HTML, XML, JSON, PNG, ...)
  - je bezstavovy
  - dotazy jsou cachovatelne

## Glossary

### gRPC example

**service definition:**

<!-- not really java ale lepsi highlighter jsem nenasel -->

```java
// The greeting service definition.
service Greeter {
  // Sends a greeting
  rpc SayHello (HelloRequest) returns (HelloReply) {}
}

// The request message containing the user's name.
message HelloRequest {
  string name = 1;
}

// The response message containing the greetings
message HelloReply {
  string message = 1;
}
```

**Running the application in code:**

```js
// SERVER
function sayHello(call, callback) {
  callback(null, { message: "Hello " + call.request.name });
}

function main() {
  var server = new grpc.Server();
  server.addService(hello_proto.Greeter.service, { sayHello: sayHello });
  server.bindAsync(
    "0.0.0.0:50051",
    grpc.ServerCredentials.createInsecure(),
    () => {
      server.start();
    }
  );
}

// CLIENT
function main() {
  var client = new hello_proto.Greeter(
    "localhost:50051",
    grpc.credentials.createInsecure()
  );
  client.sayHello({ name: "you" }, function (err, response) {
    console.log("Greeting:", response.message);
  });
}
```
