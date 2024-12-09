## Základy OOP
* polymorfizmus
    * Čo je to polymorfizmus a aké sú jeho výhody?
    * Aké sú rôzne spôsoby zabezpečenia polymorfizmu?
    * Ako pomáha dedičnosť pri zabezpečovaní polymorfizmu?
    * Ako môže byť využitý polymorfizmus pri testovaní a pri predídení chybám?
    * Čo je to parametrický polymorfizmus?
    * Ako pomáha generické programovanie pri parametrickom polymorfizmu? Ako vieme použiť typové parametre pre zabezpečenie parametrického polymorfizmu?
    * Čo je to subtype polymorfizmus a v čom sa líši od parametrického polymorfizmu?
    * Môže byť použité generické programovanie pre zabezpečenie subtype polymorfizmu?
* prepisovanie metód
    * Čo je to prepisovanie metód?
    * Čo je to dynamický (runtime) polymorfizmus a ako funguje v C#?
    * Aké sú možnosti prepísania metód v C# (kľúčové slová) a aký je medzi nimi rozdiel?
    * Je možné prepísať statické metódy z nadtriedy alebo z implementovaného rozhrania?
    * Je možné prepísať sealed metódy z nadtriedy?
    * Je možné prepísať konštruktor nadtriedy?
    * Aké sú výhody prepisovania metód?
* preťaženie metód
    * Čo je to preťaženie metód a ako súvisí so statickým polymorfizmom?
    * Aký je rozdiel medzi statickým a dynamickým polymorfizmom?
    * Môžeme preťažiť metódu tak, že všetky parametre budú rovnakého typu, rozdiel bude iba v návratovom type?
    * Čo je to preťaženie operátorov? Uveďte príklad v C#.
    * Aké sú výhody preťaženia metód?
    * Aký je rozdiel medzi preťažením a prepísovaním metód?
* abstrakcia
    * Čo je to abstrakcia?
    * Aký je rozdiel medzi abstrakciou a enkapsuláciou?
    * Aké sú výhody abstrakcie?
    * Je možná abstrakcia bez enkapsulácie?
    * Čo je to trieda a čo je objekt?
    * Čo je to stav objektu?
    * Čo je to interface a contract?
* enkapsulácia
    * Čo je to enkapsulácia?
    * Aký je rozdiel medzi enkapsuláciou a abstrakciou?
    * Ako vieme zabezpečiť enkapsuláciu v C#?
    * Aké sú dobré zvyky pri použití enkapsulácie?
* dedičnosť
    * Čo je to dedičnosť a čo je jej zámerom?
    * Na základe akej podmienky vieme definovať dedičnosť medzi dvomi triedami?
    * Akú rolu hrá dedičnosť pri polymorfizme?
    * Môžeme dediť od sealed triedy?
    * Čo sa stane pri volaní prepísanej metódy bez použitia kľúčového slova base?
    * Ako funguje dedenie statických prvkov z nadtriedy? Uveďte možné prípady použitia.
    * Koľkonásobná môže byť dedičnosť v C# a aký je dôvod za týmto návrhovým rozhodnutím?
* kompozícia
    * Čo je to kompozícia?
    * Aký je rozdiel medzi dedičnosťou a kompozíciou?
    * Na základe akých kritérií vieme určiť, či medzi dvomi triedami môže byť vzťah kompozície?
    * Aké problémy s dedičnosťou môžeme eliminovať pomocou kompozície?
    * V akých prípadoch je preferovaná kompozícia k dedičnosti a naopak?

## Základy C\#
* anatómia programu a kódu
    * Čo je to assembly?
    * Prečo potrebujeme importovať pomocné menné priestory?
    * Prečo potrebujeme zadefinovať modifikátory prístupu?
    * Aký je rozdiel medzi parametrom a argumentom?
* kompilácia a spustenie C# kódu
    * Vysvetlite proces kompilácie a spustenia kódu v C#.
    * Aký je rozdiel medzi zdrojovým kódom a Common Intermediate Language?
    * Čo je to strojový kód?
    * Čo je to .NET a aké sú jeho hlavné vlastnosti?
    * Aké sú základné časti .NET rámca a aká je ich úloha?
* údajové typy
    * Čo sú to primitívne údajové typy?
    * Čo obsahuje premenná primitívneho údajového typu, ak jej nebola priradená žiadna hodnota?
    * Prečo potrebujeme uviesť suffix pri typoch long, float a double?
    * Čo sa stane pri priradení hodnoty premennej primitívneho dátového typu k inej premennej rovnakého primitívneho typu?
    * Čo sú to referenčné údajové typy?
    * Aký je rozdiel medzi primitívnymi a referenčnými údajovými typmi?
    * Akú úlohu hrajú vnútorné premenné a metódy pri referenčných typoch?
    * Čo obsahuje premenná referenčného údajového typu, ak jej nebola priradená žiadna hodnota?
* odovzdávanie hodnôt
    * Aký je rozdiel medzi odovzdávaním hodnoty a odovzdávaním referencie pri volaní metód?
    * Ako sa odovzdáva argument referenčného typu v C# pri volaní metódy?
    * Ako je umožnený prístup k pôvodnému objektu odovzdanému metóde?
    * Ako môžeme meniť hodnotu odovzdanej referencie v rámci metódy?
* trieda object
    * Čo je nadtriedou všetkých tried v C#?
    * Aké metódy definuje trieda object, ktoré môžu byť neskôr prepísané v podtriedach?
    * Ako funguje defaultné porovnávanie dvoch objektov?
    * Ako je zabezpečená možnosť kopírovania objektov?
    * Ako funguje defaultná implementácia kopírovania objektov?
* modifikátory prístupu
    * Čo je úlohou modifikátorov prístupu?
    * Aké modifikátory prístupu definuje jazyk C# a ktoré môžu byť použité pre triedy resp. pre vnútorné premenné?
    * Aký je význam modifikátorov prístupu private, protected, public a internal?
    * Aký je defaultný modifikátor prístupu?
    * Aká je viditeľnosť vnútorných prvkov pri dedení v podtriede?
    * Ako by mali byť definované modifikátory prístupu istého vnútorného prvku v nadtriede resp. podtriede?
    * Na základe čoho by sme si mali zvoliť najvhodnejší modifikátor prístupu?
    * Prečo potrebujeme gettery a settery ak vnútorné premenné vieme aj sprístupniť?
    * Aké sú hlavné výhody použitia getterov a setterov?
    * Ako funguje použitie properties a v čom sa líši od použitia get/set metód?
* statické prvky
    * Čo sú to statické prvky?
    * Môže existovať statická trieda v C#? Prečo (nie)?
    * Čo sú to statické metódy?
    * Čo sú to statické premenné?
    * Aký je rozdiel medzi statickými a nestatickými premennými?
    * Ako je umožnený prístup k statickým vnútorným premenným?
    * S akými hodnotami môže pracovať statická metóda (statická, nestatická, parametre)? Prečo?
* konštanty
    * Ako dokážeme vytvoriť konštantnú premennú v C#?
    * Aký je rozdiel medzi kľúčovými slovami readonly a const?
    * Môžeme zmeniť objekt, ktorý je uložený v konštantnej/readonly premennej?
    * V akých prípadoch dokážeme nastaviť hodnotu konštantnej/readonly premennej?
    * Aký zmysel má použitie konštantných hodnôt z pohľadu optimalizácie?
* virtual a sealed
    * Na čo sa používa kľúčové slovo virtual?
    * Čo znamená sealed trieda?
    * Aké sú výhody sealed triedy?
    * Čo znamená sealed metóda?
    * Aké sú výhody sealed metód?
    * Môžeme zmeniť modifikátory virtual resp. sealed v podtriedach?
* inicilizácia objektov
    * Ako vytvárame nové objekty v C#?
    * Ako definujeme spôsob inicializácie nových objektov istej triedy?
    * Koľko konštruktorov môže definovať jedna trieda?
    * Ako a kde sa inicializujú statické vnútorné premenné?
* garbage collection a deštrukcia objektov
    * Čo je to garbage collector a na čo sa používa?
    * Na základe akých pravidiel môže fungovať garbage collector?
    * Ako sa definuje spôsob vymazania objektov istej triedy?
    * Koľko deštruktorov môže mať jedna trieda?
    * Kedy sa zavolá deštruktor triedy?
    * Aké operácie by mal obsahovať deštruktor, na čo sa má použiť?
    * Je odporúčané explicitné volanie deštruktora?
    * Ako môžeme uvoľniť systémové zdroje bez použitia deštruktora?
    * Čo sa stane, ak deštruktor vygeneruje chybu?
* pretypovanie hodnôt
    * Aké sú možné pretypovania pri primitívnych typoch?
    * Ako funguje pretypovanie pri primitívnych typoch?
    * Čo je to boxing resp. unboxing?
    * Aký je rozdiel medzi skutočným a zdanlivým typom?
    * Ako funguje pretypovanie pri pretypovaní na nadtriedu?
    * Ako funguje pretypovanie pri pretypovaní na podtriedu?

## Designové koncepty
* abstraktné triedy
    * Čo je to abstraktná trieda a v čom sa líši od neabstraktnej triedy?
    * Akú rolu hrá abstraktná trieda v hierarchii tried?
    * Ak z abstraktnej triedy nemôžeme vytvoriť inštanciu, aký má zmysel definícia konštruktora?
    * Aký je rozdiel medzi abstraktnou triedou a rozhraním? Kedy je výhodnejšie použiť abstraktnú triedu ako rozhranie?
* rozhrania
    * Čo je to rozhranie (interface)?
    * Akú rolu hrá rozhranie v hierarchii tried?
    * Aké prvky môže obsahovať rozhranie?
    * Čo sú to defaultné metódy rozhrania? Aké sú ich výhody?
    * Môžu byť prepísané defaultné metódy rozhrania?
    * Čo sa stane, ak trieda implementuje dve rôzne rozhrania s dvomi implementáciami rovnakej defaultnej metódy?
    * Aký je rozdiel medzi defaultnou metódou definovanou v rozhraní a v abstraktnej triede?
    * Čo sú to statické metódy rozhrania? Aké sú ich výhody?
    * Môžu byť prepísané statické metódy rozhrania?
    * Aký je rozdiel medzi statickou a defaultnou metódou rozhrania?
    * Čo sa stane, ak trieda implementuje dve rôzne rozhrania s dvomi implementáciami rovnakej statickej metódy?
    * Čo je to marker rozhranie a na čo sa používa?
    * Aký je rozdiel medzi rozhraním a abstraktnou triedou? Kedy je výhodnejšie použiť rozhranie ako abstraktnú triedu?
    * Ak rozhranie môže obsahovať defaultnú implementáciu metód, v čom sa takéto rozhranie líši od abstraktnej triedy?
* viacnásobné dedenie
    * Čo je to diamond problém a ako s ním súvisí viacnásobné dedenie?
    * Prečo C# neumožňuje viacnásobné dedenie?
    * Ako môžeme vyriešiť diamond problém pri implementovaní dvoch rozhraní s rôznou defaultnou implementáciou istej metódy?
* programming to interface
    * Čo je podstata paradigmy programming to interface?
    * Aké sú výhody programming to interface?
    * Ako podporuje programming to interface voľnejšie prepojenie programových prvkov?

## Údajové typy
* neexistujúce hodnoty
    * Ako vieme reprezentovať neexistujúce alebo chýbajúce hodnoty v C#?
    * Čo je to nulovateľný typ?
    * Aké sú výhody použitia nulovateľných typov?
    * Ako ovplyvní použitie nulovateľného typu kompiláciu resp. beh programu?
* records
    * Čo je to record (záznam) a ako sa definuje?
    * Ako funguje porovnávanie dvoch záznamov?
    * Kedy vieme kopírovať záznam do novej premennej?
    * Aký je rozdiel medzi nominálnym a pozičným záznamom?
* štruktúra
    * Čo je to štruktúra a v čom sa líši od triedy?
    * Podporuje štruktúra dedičnosť a implementáciu rozhraní?
    * Ako sa definuje štruktúra?
* enumerácie
    * Čo je to enumerácia?
    * Ako sa definuje a ako funguje enumerácia v C#?
    * Aké sú alternatívy k použitiu enumerácií?
    * Aké sú výhody použitia enumerácií v porovnaní s alternatívnymi riešeniami?
* porovnávanie hodnôt a typov
    * Aké sú rôzne spôsoby porovnávania hodnôt?
    * Ako funguje použitie operátora == pre primitívne resp. referenčné typy?
    * Vysvetlite rozdiel medzi použitím == a Equals() na príklade s reťazcami.
    * Prečo sa neodporúča použitie operátora == pre desatinné čísla? Ako by sa mali porovnávať desatinné hodnoty?
    * Ako funguje metóda Equals()?
    * Kedy je preferované použitie metódy Equals() namiesto operátora porovnávania?
    * Ako a kde sa definuje porovnávanie objektov istej triedy?
* boxing a unboxing
    * Čo je to boxing a unboxing?
    * Čo je hlavná výhoda boxingu?

## Generické programovanie
* generické programovanie
    * Čo je to generické programovanie?
    * Čo je to typový parameter a na čo sa používa?
    * Čo sú hlavné výhody použitia generických riešení?
    * Ako a kedy funguje typová kontrola pri použití generického programovania?
* typový parameter
    * Ako sa dajú určiť všeobecné typy hodnôt pri generickom programovaní?
    * Ako sa dajú určiť vzťahy dedičnosti pri generickom programovaní?
    * Ako funguje dedičnosť generických typov?
    * Ako funguje inferencia typov pri použití generického programovania?
* generické metódy
    * Čo je to generická metóda? V akých triedach môže byť definovaná?
    * Čo sú výhody použitia generických metód resp. generických parametrov?
    * Ako je zabezpečená typová kontrola pri generických metódach?
* kovariancia a kontravariancia
    * Čo je to kovariancia?
    * Je možné v C# pridať do kolekcie generického typu objekt podtriedy daného typu?
    * Čo je to kontravariancia?
    * Majme kolekciu generického typu, ktorý však musí byť nadtriedou istej triedy. Prečo nemôžeme vždy načítať prvok takejto kolekcie do premennej typu odvodenej triedy?
    * Uveďte prípady použitia kovariancie resp. kontravariancie.

## Kolekcie
* Uveďte niekoľko typov kolekcií v C# a popíšte ich charakteristiky, funkcionalitu a jedinečné vlastnosti:
    * Array
    * List
    * Queue
    * Stack
    * LinkedList
    * Dictionary
    * Set
* kolekcie
    * Čo je to kolekcia a ako súvisí s generickým programovaním?
    * Je možné vytvárať kolekcie primitívnych typov v C#?
    * Aký je rozdiel medzi kolekciou a poľom (array)?
    * Ako podporuje rámec .NET použitie kolekcií?
    * Aké základné algoritmy poskytuje .NET pre prácu s kolekciami?
* prechádzanie kolekciami
    * Uveďte a porovnajte rôzne spôsoby iterácie nad kolekciou (for vs foreach/iterator).
    * Kedy a ako je možná aktualizácia kolekcie keď ňou prechádzame?
    * Kedy a ako je možná aktualizácia hodnoty v kolekcii keď ňou prechádzame?
    * Je limitovaný smer prechádzania kolekciou v C#?

## Spracovanie chýb a výnimiek
* chyby a výnimky
    * Čo je to výnimka (exception)?
    * Aké sú základné triedy v C# pre reprezentáciu výnimiek?
    * Ako vieme zadefinovať vlastné výnimky?
* spracovanie výnimiek
    * Ako dokážeme spracovať výnimky v C#?
    * Aké sú najlepšie zvyky a prístupy pri spracovaní výnimiek?
    * Ako prebieha spracovanie viacerých výnimiek v jednom bloku? Aké sú jeho výhody a nevýhody?
    * Ako rozdeliť bloky kódu do jednotlivých čast try-catch-finally?
* finally
    * Ako vieme garantovať, aby sa blok kódu vždy vykonal?
    * Kedy sa zavolá finally blok?
    * Aké operácie by mal obsahovať finally blok, na čo sa má použiť?
    * V ktorých prípadoch sa nevykoná finally blok?
    * Môže obsahovať finally blok return príkaz? Aká je jeho funkcionalita?

## Návrhové vzory
* Štruktúra, vlastnosti, implementácia, prípady použitia, výhody a nevýhody nasledujúcich návrhových vzorov:
    * abstract factory
    * builder
    * factory method
    * prototype
    * singleton
    * adapter
    * bridge
    * composite
    * decorator
    * facade
    * proxy
    * chain of responsibility
    * command
    * iterator
    * mediator
    * observer
    * strategy
    * visitor
