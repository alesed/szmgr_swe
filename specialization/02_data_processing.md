# Data processing

> Basic concepts and principles of data warehousing, data analytics and business intelligence. Data warehouse lifecycle. Big data analytics, languages for implementing analytic tasks, database-level analytics. Advanced data processing techniques, performance aspects of big data processing. Practical examples for all of the above. (PA220, PA036)

[PA220 prednasky](https://is.muni.cz/auth/el/fi/podzim2022/PA220/um/)

## Basic concepts and principles of data warehousing, data analytics and business intelligence

- Business inteligence
  - procesy a nastroje pro sbery, analyzu a prezentaci/vizualizaci dat
  - podpora pri rozhodovani
  - umoznuje transformaci dat do inforrmaci
  - jadrem je datovy sklad (data warehouse)
- **OLTP (Online Transaction Processing)**
  - zpusob ukladani dat v DB pro transakcni zpracovani
  - menici se data v DB, zajisteni konzistence a CRUD operaci
  - zamykani tabulek/radku
  - vhodne pro operativni provoz podniku (zajima nas, co mame na sklade, jaka je aktualni cena)
  - queries zname dopredu, mame navrhnuty system, ktery vyuziva preddefinovane queries
- **OLAP (Online Analytical Processing)**
  - ukladani dat pro analyticke zpracovani
  - nemenne data v DB
  - zamykani neni treba
  - mnohem vetsi objem dat
  - pro dlouhodobe ulozeni dat
  - reflektuje historii a vyvoj v case
  - denormalizovana forma, hodne indexu
  - snaha o eliminaci joinu
  - nevadi nam (tak moc) datova redundance
- **Granularita**
  - popisuje, z jake urovne se na data divame (kolik se prodalo za den/tyden/mesic?)
  - nejnizsi granularita je jeden fakt
- **Measure**
  - aspekt faktu, ktery nas zajima
  - nektere hodnoty mereni lze agregovat, nektere scitat, nektere vubec
- **Conformed dimension**
  - dimenze, ktera ma stejne hodnoty pro data z vicero zdroju
  - vetsinou je to casova dimenze

### Data Warehouse

- OLAP
- oddeleni od OLTP, abychom nezatezovali produkcni DB
- obvykle 1 DB
- data se nemeni, pouze pridavaji
- data z vicero zdroju
- vyzaduje ocisteni a konzistentnost dat (pomoci ETL (extract-trasform-load) procesu)
- lze vyuzit standardni (relacni) DB nebo specializovana reseni (Teradata, Google BigQuery)
- jednoducha reprezentace dat, aby s nima mohli pracovat analytici a bylo mozne pouzivat jednoduche analyticke dotazy
  - minimum joinu
- cilem je umoznit a zjednodusit analyzu dat
- **Data mart**
  - maly DW, soustredi se na jednu zajmovou jednotku (objednavky)
  - dekompozice za ucelem zvyseni efektivity/omezeni pristupu do jednotlivych asti DW
  - podoby:
    - **Nezavisly data mart**: zadny centralni zdroj pravdy (DW), data primo ze zdroju
    - **Logicky data mart**: data marty funguji jako logicke pohledy na cast datoveho skladu, jednodussi na udrzbu
- **Data Cube**
  - obsah DW, umeznuje pohled na data z ruznych dimenzi
  - cube znamena jen omezeny pocet dimenzi, ale ve skutecnost je to klidne 15 dimenzi
    - lepsi nazev je hypercube
    - cube nazev a vizualizace hlavne z duvodu lepsi lidske predstavivosti
  - sklada se z bunek (cells), kdy kazda je kombinaci hodnot dimenzi
    - bunka bez dat = prazdna bunka
  - **dense/sparse cube** - hodne/malo neprazdnych bunek v data cube
- Na datove sklady/data marty jsou obvykle napojeny **vizualizacni aplikace** (Grafana, PowerBI)

### Dimenzionalni programovani

- stredobodem je subjekt obsahujici **fakta** (orders)
- **tabulka faktu** obsahuje konkretni zaznamy mereni a reference na **tabulky dimenzi** (popisy, filtery)
- **tabuka dimenzi**
  - casto odpovedi na otazky KDO, KDY, KDE, JAK
  - detailni a denormalizovane pro snadnou analytiku
  - redundance nevadi
  - dimenze casove muzeme rozdelit na datum a cas zvlast
  - cim vice dimenzi, tim konkretneji se muzeme DW (data warehouse) dotazovat
  - dimenze obvykle predvyplnime
  - dimenze jsou popisne data k faktum, podle kterych je mozne seskupovat
- je zadane vytvaret **surrogate keys** pro jasnou identifikaci zaznamu v DW
- **typy faktu:**
  - transakcni - udalost spojen a s hodnotou (nakup)
  - snapshot - zachycuje aktualni hodnotu (naplnenost skladu)
  - bez hodnoty - fakt nema numerickou hodnotu, obvykle nejaka udalost (click na button), agregacni data (kumulativni data z obdobi) nebo data kombinovana z vicero procesu
- nevadi nam drobna neaktualnost dat
- query nezname dopredu, ale ad-hoc

#### Star schema

- jedna tabulka faktu
- tabulky dimenzi na primo propojene s tabulkou faktu
- neexistuje hierarchie tabulky dimenzi
- joiny pouze mezi tabulkou faktu a tabulkou dimenzi

#### Snowflake schema

- star schema, kde dimenze maji hloubku (obsahuje reference na dalsi tabulky - treba month id a month name)
- zpusobuji performance problemy
- snazime se tomuto schematu vyhnout

## Data warehouse lifecycle

- **urceni cile a planovani**
  - co od systemu ocekavamae, jaky rozsah dat nas zajima, odhad ceny, prioritizace subjektu (datamartu)
  - chceme implementovat DW s **top-down** (cely DW a pak separace na casti) approachem nebo **bottom-up** (jednotlive data marty a pak celek - DW)
- **navrh infrastruktury**
  - volba vhodnych nastroju a technologii
  - architektonicke reseni
- **navrh a vyvoj data martu**
  - iterativne vytvarime data marty, ty pak zapojujeme do DW systemu
  - volba procesu vcetne modelovani (UML/BPMN)
  - urceni granularity - prodej jednoho produktu jednomu zakaznikovi na jedne pobocce
  - identifikace dimenzi
  - identifikace faktu
- **ETL (Extract, Transform, Load)**
  - extrahovat - z datovych zdroju
  - transformovat
    - odstranit duplicity
    - upravit do jednotneho stylu DB (inches/cm -> cm, feets/meters -> meters)
    - vycistit od nekompletnich/chybnych dat
    - obcas je potreba rozbit data na vicero sloupcu (name -> first name, last name)
    - lze castecne automatizovat pomoci skriptu, ale vetsinou jsou stale treba manualni zasahy
    - obvykle vkladame do staging table a potom do DW
    - je fajn batchovat (delat po castech), at se v ETL procesu nezamotame, neztratime, nezablokujeme
  - naplnit
    - aktualizujeme dimenze
    - upsert (update, insert if not exists)
    - naplnovat po vetsich castech, aby se indexy a materialized views nemuseli prepocitavat po kazdem radku
    - muze pomoct vkladani predserazenych dat
    - paralelizace procesu (dimenze, fakta) lze vykonavat soubezne
- **Zmena v dimenzi**
  - **prepis** - stara data -> nove data
  - **pridani sloupce s predchozi verzi** old_dim = old_value, dim = new value
  - **verzovani** valid from a valid to
  - **pridani dimenze** zeme a region -> ne vzdy nas zajima region -> separace do hierarchicky zanorenene dimenze

## Big data analytics, languages for implementing analytic tasks, database-level analytics

- **Big Data** = data, ktera kvuli sve rychle a kontinualni tvorbe, velkem objemu, ci slozitosti, vylucuji tradicni zpracovani analytickymi zpusoby
  - rychly prichod dat = kontinualni zpracovani
  - nelze pouzit batch processing
  - potreba stream processingu (distribuovane zpracovani - Apache Kafka)
  - velikost dat lze zvladat pomoci distribuovanych DB/souborovych systemu (noSQL, Hadoop distributed file system)
  - slozitost dat vyresit pomoci specializovanych nastroju (grafove databaze)
  - **batch** = jednou za cas zpracujeme data
  - **stream** = kontinualne vkladame data tak, jak vznikaji
- **Jazyky pro realizaci analytickych dotazu**
  - SQL a jeho derivaty
  - MapReduce / Hadoop
    - moznost specifikace transformacnich uzlu
  - noSQL a pristup jako v SQL nebo uplne jiny analyticky pristup (MongoDB a jeho knihovny)
- **Druhy SQL dotazu pro analytiku**
  - **Slice** = v ramci dimenze vybereme jen jednu hodnotu a podle ni selectujeme
  - **Dice** = jako slice, ale pracuje s intervaly od-do nebo hodnot vice dimenzi
  - **Roll-up** = agregace dat - vynechavani dimenzi a agregaci nad mensim poctem dimenzi (celkove hodnoty)
  - **Drill-down** = opak roll-upu, z abstrakce do detailu
- **Pivoting**
  - preskladani a agregace dat za ucelem vizualizace
  - nejjednodussi variantou je kontingencni tabulka (cross table)
  - v SQL se drive muselo vyuzivat UNION operace
    - nyni je mozno pouzit extendovane prikazy:
      - `GROUP BY ROLLUP(...)` - agreguje nad dimenzema a potom vynechava dimenze a agreguje nad celky
      - `GROUP BY CUBE(...)` - agregace kazdy s kazdym (vcetne NULL values, respektive vynechani dimenzi)
      - `GROUP BY GROUPING SETS(...)` - vetsi kontrola nad specifikaci dle vlastniho gusta
- **Prisupy k implementaci OLAP**
  - **ROLAP (Relational OLAP)**
    - data v relacni DB
    - dimenze pomoci star chema, dotazovani pomoci SQL
    - neni potreba specializovany system
    - flexibilita
    - horsi response time
    - zabira vetsi mnozstvi mista nez MOLAP (3x-4x)
  - **MOLAP (Multidimensional OLAP)**
    - data ve specialnich multidimenzionalnich strukturach (in-memory db, multidimenzionalni pole/matice, kde vyuzivame primou adresaci na disku)
    - rychlejsi queries
    - zabiraji mene mista
    - horsi flexibilita (pridani hodnoty do domeny = pain -> musime pridat velke mnozstvi bunek, kdezto u ROLAPu jeden radek v tabulce dimenze)
    - specializovany system
    - mnohdy soucasti DB systemu (MS SQL server/Oracle)
  - **HOLAP (Hybrid OLAP)**
    - kombinace obou = ROLAP + MOLAP
    - cista data ulozena na ROLAP
    - agregace na MOLAP
    - flexibilita, rychlost, ale vetsi slozitost systemu

## Advanced data processing techniques, performance aspects of big data processing

- pro lepsi rychlost v OLAPu se vyuziva:
  - materialized view
  - index
  - denormalizovane schema

### Indexes

- rychlejsi ziskani dat, ktere nas zajimaji, pomoci predpocitanych vysledku
- omezuji prostor nutny k progledani pri cteni dat
- obvykle **B+ stromy**, ale ty jsou omezene pro 1D data, takze nejsou vhodne pro vice dimenzi
- **UB stromy**
  - multidimenzionalni data linearizovne pomoci **Z-krivky** a nasledne indexovany pomoci **B\* stromu** (jako B+ ale lepsi rebalanc)
  - linearizace poskytuje dobry vykon pro intervalove dotazy - zajistuje, ze data, ktera jsou si blizka puvodne jsou si blizka i po linearizaci
  - samotna indexace se deje pomoci konverze souradnic na binarni cislo a nasledne prokladani bitu souradnic
- **R stromy**
  - obdelniky, vetsinou 2D, spatne se skaluji do mnoha dimenzi
- **Bitmap indexy**
  - vhodne pro dimenze s malo variantami (pohlavi)
  - vytvorime bitove pole o delce tabulky faktu
  - index v poli = radek v tabulce faktu
  - pravda ? nastavime 1 a vsechny dalsi na 1 (jinak 0)
  - hodnota neznamena, ze se narodil v mesici, ale ze uz byl nazivu v danem mesici

### Others

#### Optimalizace JOINu

- komutativni = nezalezi na poradi operandu `A join B = B join A`
- asociativni = nezalezi na zavorkach, poradi JOINu lze preskladat pro efektivnejsi provedeni SQL dotazu
- obvykle optimalizaci resi DB system za nas
  - pocet kombinaci poradi joinu je `n!`
  - uzivatel muze poskytnout hinty/vlastni plan pokud vi, co dela a byt tak o dost efektivnejsi

#### Apache Hadoop

- platforma pro paralelni/distribuovane zpracovani velkych datasetu
- batch processing
- vysoka dostupnost zajistena repliakci dat
- vyuziva **Hadoop Distributed File System** (HDFS)
  - abstrahuje distribuovanost - uzivatel pracuje s daty jednotnym zpusobem
  - spolu s HDFS vyuziva **MapReduce**
    - paralelni zpracovani dat
    - postupne map, grouping, reduce faze

#### Apache Hive

- distribuovany DW postaveny nad Hadoop (a HDFS)
- SQL like rozhrani pro dotazy (HiveQL) -> prevedeno do MapReduce dotazu
- umoznuje pouziti strukturovanych dat
- serializace/deserializace dat do ruznych formatu (xml, csv, json)
- vhodny pro dlouho bezici ETL jobs
- vyhledavame low latenc? -> radsi **Apache Impala** (SQL query engine nad Hadoopem)

#### Stream processing

- nezpracovavame balik dat, ale kontinualni stream dat
- **Apache Spark** (real-time vypocty, sklada ze zdroju dat a acyklicky propojenych zpracovavajicih uzlu)
- **Apache Storm** (analyticky engine pro large-scale data processing, umi batch i stream processing)
