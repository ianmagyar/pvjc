# Cvičenie 5: Projekt sa rozrastá

Na dnešnom cvičení budeme ďalej rozvíjať našu hierarchiu tried, pričom si precvičíme dedičnosť, abstraktné triedy a ukážeme si ďalší návrhový vzor - Command. Ako už samotný názov naznačuje, návrhový vzor zabezpečuje spracovanie príkazov, ktoré budú dávať naši používatelia, a aby interakcia bola intuitívna, pozrieme sa na to, ako dokážeme pracovať s používateľským vstupom.

Pre ďalšiu prácu budete potrebovať mať implementované minimálne triedy `PowerSource` a `Crystal` a ich rozhrania z minulého cvičenia. [Ak vlastný projekt nemáte po ruke, môžete pracovať s týmto projektom.](lab05/lab05-start.zip)

## 1. krok: Observer pt. II

Na minulom cvičení sme už zadefinovali jednoduchú formu návrhového vzoru Observer, ktorá však mala zásadnú chybu: observer objekty nedokázali zistiť, v akom stave sa nachádzal nimi pozorovaný objekt. V ukážkových prípadoch to problém nerobil, keďže doteraz sme rátali s tým, že každý objekt vytvoríme vo vypnutom stave a prepojíme ich ešte pred spustením hry. Následne sme zabezpečili prepnutie stavu observerov pri zmene stavu pozorovaného objektu. Čo sa však stane, ak vypnutý kryštál budeme chcieť napojiť na zapnutý zdroj počas behu programu? Budeme musieť zosynchronizovať ich stavy.

Túto funkcionalitu vieme vyriešiť niekoľkými spôsobmi. Na dnešnom cvičení si ukážeme jednoduché riešenia, kde spolu s notifikáciou príde aj informácia o pozorovanom objekte. Naša implementácia bude čiastočne redundantná, avšak slúži najmä ako ukážka možných riešení takéhoto problému.

Pridajte do konštruktora `Crystal` referenciu na `PowerSource` ku ktorému bude pripojený. Následne z konštruktora viete priamo dať subscribe na tento objekt, ako aj získať aktuálny stav zdroja energie. Takéto riešenie by však umožnilo vytvorenie iba kryštálov, ktoré sú už pripojené na zdroj energie. Preto ošetrite prípad, že konštruktor dostane `null` referenciu, a pridajte aj metódu, ktorá umožní pridať zdroj až po spustení programu.

Ďalšia možnosť je rozšíriť metódu `Notify` definovanú rozhraním `IObserver` o parameter, ktorý bude obsahovať referenciu na pozorovaný objekt. Cez túto referenciu môžete potom získať aktuálny stav pozorovaného objektu. Upravte najprv deklaráciu metódy v rozhraní `IObserver`:

```
void Notify(IObservable observable)
```

Následne upravte definíciu metódy v triede `Crystal` a volanie v triede `PowerSource` tak, aby ste mohli získať informáciu o stave zdroja (či je zapnutý alebo vypnutý). Pri implementácii narazíte na problém nekompatibility jednotlivých rozhraní triedy `PowerSource`, ktorý viete vyriešiť [pretypovaním](https://learn.microsoft.com/en-us/dotnet/csharp/programming-guide/types/casting-and-type-conversions) alebo použitím [kľúčového slova `as`](https://learn.microsoft.com/en-us/dotnet/csharp/language-reference/operators/type-testing-and-cast#as-operator).


## 2. krok: Abstraktné triedy

Naposledy sme si vytvorili triedy `Crystal` a `PowerSource`, ktoré mali časť funkcionality rovnakú, dnes sa pozrieme na to, ako by sme mohli túto časť dať dokopy.

Vytvorte si triedu `AbstractSwitchable`, ktorá nech rozširuje `AbstractActor` a implementuje `ISwitchable`. Trieda bude definovať spoločnú funkcionalitu tried `Crystal` a `PowerSource`, teda konkrétne metódy `Toggle()`, `IsOn()`, `TurnOn()` a `TurnOff()`, rozdiel môže byť iba v `UpdateAnimation()`, na ktorú sa odkazujú.

Z `Crystal` presuňte definíciu metód `Toggle()`, `IsOn()`, `TurnOn()` a `TurnOff()` do `AbstractSwitchable` a nastavte dedenie medzi triedami (samozrejme ich vymažte aj z `PowerSource`). Chýbajúcu `UpdateAnimation()` deklarujte ako `abstract` a ponechajte implementácie, ktoré už máte v `Crystal` a `PowerSource` - toto zabezpečí, že budú použité zodpovedajúce implementácie, ale nemusíte zbytočne opakovať kód, ktorý je spoločný. Ak definície `UpdateAnimation()` sa nelíšia vo vašej implementácii, môžete ju dať ako súčasť spoločnej funkcionality do `AbstractSwitchable`, tým pádom metóda nebude abstraktná.

Pri prepisovaní metód C# neumožňuje prekrývať ľubovoľné metódy, iba tie, ktoré sú na to označené - keyword `virtual` alebo `abstract`. V prípade potreby doplňte slovíčko do deklarácie metód.

**Poznámka:** Pri niektorých metódach v nadtriede nechceme, aby metóda bola volaná mimo triedy, teda mohli by sme im priradiť prístup `private`. Ak však chceme, aby metóda bola prekrytá podtriedami, musíme ju označiť minimálne ako `protected` (dostupné iba v rámci triedy a jej potomkov).

## 3. krok: Nadtrieda a podtrieda

Vytvorte si triedu `CrackedCrystal`, bude sa jednať o kryštál, ktorý je poškodený a dá sa rozsvietiť len niekoľkokrát. Ako predka (rodiča) zoberte `Crystal`.

Pre zabezpečenie potrebnej funkcionality prekryte metódu `TurnOn()` (nezabudnite upraviť deklaráciu aby obsahovala `virtual`) tak, aby sa kryštál dal zapnúť iba toľkokrát ako je to povolené. Keby hráč chcel takýto kryštál zapnúť ešte raz, nič sa nestane. 

Prekrytím metódy je schovaný pôvodný kód, ale nie je úplne stratený: pomocou kľúčového slova `base` sa k nemu vieme dostať, napríklad:

```
base.TurnOn();
```

Samozrejme pre ukladanie počtu možných zapnutí potrebujete novú členskú premennú (typu `int`), zadefinujte ju teda a pridajte aj nový konštruktor. V konštruktore využite kód, ktorý ste už implementovali v `Crystal`. Konštruktory je možné vzájomne volať pomocou `this` (v rámci jednej triedy) a `base` pre konštruktor nadtriedy:

```
public class DerivedClass : BaseClass
{
    public DerivedClass() : base()
    {
        //magic happens here
    }
    public DerivedClass(string s) : base()
    {
        //some more magic happens here
    }

    public DerivedClass(int x) : this()
    {
        //and magic again
    }
}
```

## 4. krok: Cometh forth Mine Own Apprentice

Nadišiel čas, vševediaci Merlin volá svojho učňa. Predstúp!

Vytvorte si triedu `Player` (rozširuje `AbstractActor`), ako animáciu použite [`player.png`](lab05/player.png). Hlavnou vlastnosťou hráča bude pohyb v hernom svete v rôznych smeroch, pre hráča však nezadefinujeme viac animácií. V prípade, že sa hráč otočí, použijeme `Animation.FlipAnimation()` a keď sa nehýbe, `Animation.Stop()`, zatiaľ však túto funkcionalitu nemusíte implementovať.

## 5. krok: Didst thee Forget How to Moveth?

Ale ako sa hýbať?

Najjednoduchšie by bolo priamo do `Update()` implementovať čítanie klávesnice a upravovať takto pozíciu. Takéto riešenie je však nepraktické a zbytočne by skomplikovalo inak jednoduchú metódu. [Namiesto toho teda využijeme návrhový vzor Command, ktorý vynašli od nás múdrejší návrhári na riešenie presne takýchto problémov.](https://en.wikipedia.org/wiki/Command_pattern)

![](lab05/command.jpg)

Zadefinujte interface `IMovable` (nech sa nachádza v namespace `Merlin.Actors`). Toto rozhranie v sebe nebude mať žiadnu funkcionalitu, jedná sa o tzv. *marker interface* - slúži len na odlíšenie tried. Nech `Player` implementuje rozhranie `IMovable`.

V engine máte definovaný ďalšie rozhranie `Command` s metódou `Execute()`:

```
public interface Command
{
    void Execute();
}
```

Jedná sa o základ návrhového vzoru, ktorý teraz postupne implementujeme.

### 5.1. `Move`

Vytvorte si priečinok `Commands` a pridajte si do neho triedu `Move` (implementuje `Command`).

Nech je konštruktor deklarovaný nasledovne: `public Move(IMovable movable, int step, int dx, int dy)`, kde `step` udáva rýchlosť pohybu a `dx` a `dy` udávajú smer pohybu pohybujúceho sa actora `movable`.

*Svet používa klasické indexovanie súradníc ako v poli, kde ľavý horný roh má [0,0], x-ová súradnica je vodorovná a rastie doprava, y-ová je zvislá a rastie smerom dole.*

**Poznámka:** V konštruktori nezabudnite vykonať typovú kontrolu, či ste dostali objekt, ktorý je aj `IActor`. Ak by to neplatilo, program nesmie pokračovať - vyvolajte chybu (s vhodnou chybovou hláškou). Chybu v C# viete vygenerovať pomocou kľúčového slova [`throw`](https://learn.microsoft.com/en-us/dotnet/csharp/language-reference/keywords/throw):

```
throw new ArgumentException("error message goes here");
```

Ak chybu nikde v kóde neošetríme, toto spôsobí, že program spadne - nadnes je to náš cieľ.

V `Execute()` zabezpečte zmenu pozície `IMovable` actora - využite metódy `GetX()`, `GetY()` a `SetPosition()`.

### 5.2. `Player`

V triede `Player` si na vhodnom mieste (asi konštruktor) inicializujte veci potrebné pre pohyb. Zabezpečte, aby sa v metóde `Update()` vykonalo čítanie z klávesnice a zodpovedajúci pohyb vykonajte pomocou návrhového vzoru command. Využite teda novú triedu `Move`.

*Čítanie z klávesnice je dostupné cez triedu `Input`. Táto implementuje vzor singleton, ktorý umožňuje mať vždy len jednu dostupnú inštanciu - dostanete sa ku nej pomocou `GetInstance()`. Príklad použitia ste mali na minulom cvičení. Okrem `IsKeyPressed()` je dostupná aj metóda `IsKeyDown()`, vyskúšajte si ich, aký je medzi nimi rozdiel?*

*Klávesy sú vymenované v enumerácii `Input.Key`, zatiaľ sú pre nás zaujímavé `Input.Key.UP / DOWN / LEFT / RIGHT` - tieto mapujú šípky.*

V návrhovom vzore command je každý pokyn reprezentovaný objektom, teda pri realizácii pohybu potrebujete vytvoriť objekt, ktorý predstavuje tento pohyb. Implementujte minimálne jeden smer pohybu a svoju implementáciu vyskúšajte. Nezabudnite si hráča pridať do sveta. Následne implementujte ostatné smery pohybu obdobným štýlom a tiež nastavujte animáciu podľa toho, či sa hráč pohybuje a v akom smere.

**Poznámka:** Aj keď v hre pracujeme s pohľadom z boku a reálne sa naša postava nebude pohybovať smerom hore a dole, tieto pohyby sa nám zídu pri definícii zložitejších konceptov ako je skok a gravitácia, ktoré nás čakajú na ďalšom cvičení.

S využitím command dokážeme zabezpečiť aj viaceré nezávislé pohyby ako aj ďalšie pokyny, napríklad vstup od používateľa a gravitáciu, prípadne odkopnutie atď.
