# Cvičenie 4: Prepojenie aktérov a návrhový vzor Observer

Na minulom cvičení sme prepojili pec a kotlík cez priamu väzbu (tzv. asociácia), na dnešnom si ukážeme iný prístup, ktorý stavia na výhody paradigmy OOP. Konkrétne ide o návrhový vzor Observer, ktorý je prvým zo sady návrhových vzorov, ktoré budeme preberať a implementovať na cvičeniach.

Keď sme doteraz chceli sledovať zmenu hodnoty (na minulom cvičení, alebo na predmetoch Programovanie a Programovanie v jazyku Python), museli sme stále dopytovať na aktuálnu hodnotu a porovnať ju s lokálnou kópiou predošlej hodnoty. Takýto prístup je síce funkčný, pri väčšom počte dopytov (ak od danej hodnoty je závislých väčší počet ďalších hodnôt) však využívame veľkú časť výpočtovej kapacity čisto na kladenie často zbytočných otázok. Bolo by teda vhodnejšie náš prístup otočiť: namiesto toho, aby sme sa pýtali na prípadnú zmenu hodnoty, budeme aktívne informovať o tejto zmene iba v momente, keď k zmene dôjde. Podobný prístup poznáte zo života pri odbere noviniek, resp. notifikácií. V OOP toto riešenie umožňuje práve použitie vzora Observer, ktorého diagram vidíte nižšie, [podrobnejšie informácie nájdete napríklad na Wikipedii](https://en.wikipedia.org/wiki/Observer_pattern).

![](lab04/observer.png)

## 1. krok: Let There Be Light

Triedy `Cauldron` a `Stove` zatiaľ necháme na pokoji a návrhový vzor implementujeme na nových príkladoch. [Resource súbory k novým animáciám sú dostupné tu.](lab04/resources04.zip) Nové súbory rozbaľte do priečinka `resources` do projektu, a nezabudnite tiež nastaviť kopírovanie súborov. [Podrobnejší popis k animáciám nájdete v predošlom cvičení (krok 3 a 5)](https://github.com/ianmagyar/pvjc/blob/master/labs/lab03.md).

### 1.1. `Crystal`

Vytvorte si triedu `Crystal`, ktorá bude reprezentovať svietiaci kryštál (a rozširuje `AbstractActor`). V konštruktore si pripravte dve animácie: `crystal_on.png`, `crystal_off.png`. Takisto umožnite umiestnenie spritu do hry na istú pozíciu na základe parametrov konštruktora.

Pridajte do triedy metódu `Toggle()` a implementujte ju nasledovne:

* nech sa pri jej zavolaní zmení stav kryštálu medzi zapnutým a vypnutým - stav objektu bude popísaný stavom kryštálu: zapnutý vs. vypnutý (na začiatku bude kryštál vypnutý);
* pri zmene stavu zmeňte animáciu.

### 1.2. `PowerSource`

Vytvorte si triedu `PowerSource`, ktorá predstavuje zdroj energie (znovu rozširuje `AbstractActor`). Trieda je veľmi podobná kryštálu, pridajte teda dve animácie `source_on.png` a `source_off.png`.

Znovu definujte metódu `Toggle()` s prakticky rovnakou implementáciou ako v prípade `Crystal`.

### 1.3. Spoločné rozhranie

Ako môžete vidieť, máme dve (zatiaľ) skoro rovnaké triedy, ktoré sa líšia iba v názve a animáciách - princíp DRY (*Don't Repeat Yourself*) nám hovorí, že by sme tieto veci mali dať do spoločnej triedy - C# však neumožňuje dediť z viacerých tried súčasne. Náš problém by sme mohli vyriešiť vytvorením triedy, ktorá by slúžila ako medzikrok v hierarchii: nová trieda by dedila od `AbstractActor` a implementovala by spoločnú funkcionalitu `Toggle` s vnútornou reprezentáciou; následne by triedy `Crystal` a `PowerSource` dedili od novej triedy. Toto by nám ale skomplikovalo hierarchiu tried, a práve preto vytvoríme iba spoločné rozhranie, ktoré bude deklarovať spoločnú funkcionalitu, ktorú ale budú implementovať aj naďalej jednotlivé triedy. 

**Poznámka:** Nové verzie C# prišli s tzv. default interface methods - rozhranie môže za určitých podmienok implementovať funkcionalitu metódy, túto možnosť ale nebudeme využívať, keďže je to netypické pre OO jazyky a best practice hovorí o veľmi špecifickom využití tohto rozšírenia.

Vytvorte si rozhranie `ISwitchable` deklarajúce spoločné metódy:

```
public interface ISwitchable
{
    void Toggle();
    void TurnOn();
    void TurnOff();
    bool IsOn();
}
```

Nech obidve triedy implementujú toto rozhranie, syntax je nasledovná. Potrebujete implementovať dodatočné metódy `TurnOn()`, `TurnOff()` a `IsOn()`, pričom môžete využiť už implementované riešenia.

```
public Crystal : AbstractActor, ISwitchable
{
    //magic happens here
```

Funkcionalitu otestujte vytvorením kryštálu a zdroja energie, umiestnite ich do sveta a zmeňte ich stav pred spustením hry.

## 2. krok: I See You

Ďalej budeme pokračovať implementáciou návrhového vzoru Observer, ku ktorému budeme potrebovať dve ďalšie rozhrania:

```
public interface IObserver
{
    void Notify();
}

```

```
public interface IObservable 
{
    void Subscribe(IObserver observer);

    void Unsubscribe(IObserver observer);
}

```

Vaše riešenie upravte nasledovne:

* nech `Crystal` implementuje `IObserver`;
* `PowerSource` nech implementuje `IObservable`;
* v `PowerSource` si udržiavajte referenciu na kryštál - keď sa zmení stav `PowerSource`, dajte o tom kryštálu vedieť. Ako typ referencie využite rozhranie `IObserver`, a na notifikáciu príslušnú metódu.

Funkcionalitu viete otestovať úpravou kódu vo funkcii `Main()`: zaregistrujte kryštál ako odberateľa zdroja energie, a na začiatku programu zapnite iba zdroj energie. Kryštál sa zapne automaticky.

## 3. krok: My Command Is Your Wish

Písať kód, ale nič nevidieť nie je veľmi praktické, a aby sme nemuseli testovanie riešiť surovým kódom, pridáme trošku interaktivity do nášho riešenia. Konkrétne sa jedná o prácu so vstupom z klávesnice, ktorú máte implementovanú v engine. Rozšírte metódu `Update()` triedy `PowerSource` tak, že pridáte do nej podmienku:

```
if (Input.GetInstance().IsKeyPressed(Input.Key.E))
```

Ak táto podmienka platí, zavolajte `Toggle()`. Ako môžete vidieť, v podmienke voláme metódu `GetInstance()`, ktorá je typickou metódou návrhového vzoru Singleton, tak ako ste to už videli na prednáške. Singleton sa v tomto prípade používa kvôli tomu, aby sme nemali dva objekty pozorujúce stlačenie klávesov.

Po úspešnej implementácii môžete po spustení hry zdroj energie vypínať a zapínať stlačením klávesu *E*.

## 4. krok: Power! Unlimited Power!

Momentálne dokážeme napájať na zdroj energie iba jeden kryštál, čo je síce super, ale pravdepodobne jeden kryštál nemá až taký odber, že by sme nevedeli pripojiť aj druhý. V C ste pre udržiavanie niekoľkých hodnôt rovnakého typu používali polia (*array*). Má ich aj C# ale oveľa praktickejšie je použiť triedu, ktorá nám funkcionalitu polí zaobaľuje. C# má takýchto tried celú sadu a sú implementované v mennom priestore `System.Collections`. Dnes budeme používať `List<T>`, teda zoznam, do ktorého vieme dynamicky pridávať a odstrániť prvky.

V popise triedy `T` predstavuje tzv. typový parameter - používa sa, keď vopred nevieme, akého typu bude parameter na vstupe, ale už potrebujeme implementovať funkcionalitu. V tomto prípade vieme, že chceme mať zoznam a nevieme akého typu budú jednotlivé prvky, no funkcionalita zoznamu bude rovnaká bez ohľadu na konkrétny typ. Typový parameter doplníme pri vytváraní konkrétneho zoznamu, ako môžete vidieť na príkladoch nižšie:

```
List<int> list; //list of integer values
List<Crystal> crystals; //list of objects of type Crystal
...
List<string> listOfStrings = new List<string>(); //create a new instance
...
```

Upravte `PowerSource` tak, aby bolo možné pridávať a odoberať observerov - pre zoznam observerov používajte triedu `List`:

* na pridanie použite `List.Add(T value)`;
* na odobratie môžete použiť `List.Remove(T value)` - pred tým si ale skontrolujte, či daný prvok už v zozname je (aby sme predišli chybám). Ako na to, pozrite si dostupné metódy pomocou dopĺňania kódu (`Ctrl + Space`) alebo v dokumentácii.
* v rámci `TurnOn()` / `TurnOff()` resp. `Toggle()` upozornite všetky observer kryštály o zmene stavu zdroja energie.

Pri aktualizácii môžete využívať iteráciu `foreach`:

```
foreach (type val in listOfTypes)
{
    // do something with val
}
```

## Doplňujúca úloha

Pre získanie ďalšieho boda do zápočtu nepotrebujete urobiť nič iné, iba poslať svojmu cvičiacemu do nasledujúceho cvičenia (v 5. týždni) váš projekt s implementovanými triedami `Cauldron`, `Stove`, `Crystal` a `PowerSource` s funkcionalitou z cvičení 3 a 4.
