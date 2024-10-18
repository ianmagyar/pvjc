# Cvičenie 5: Projekt narastá

Na dnešnom cvičení budeme ďalej rozvíjať našu hierarchiu tried, pričom si precvičíme dedičnosť, abstraktné triedy a hlavnú postavu.

Pre ďalšiu prácu budete potrebovať mať implementované minimálne triedy `PowerSwitch` a `LightBulb` a ich rozhrania z minulého cvičenia. [Ak vlastný projekt nemáte po ruke, hotové kódy môžete prevziať z tohto projektu.](lab05/game_l04.zip)

## 1. krok: Observer pt. II

Na minulom cvičení sme už zadefinovali jednoduchú formu návrhového vzoru *Observer*, ktorá však mala zásadnú chybu: observer objekty nedokázali zistiť, v akom stave sa nachádzal nimi pozorovaný objekt. V ukážkových prípadoch to problém nerobil, keďže doteraz sme rátali s tým, že každý objekt vytvoríme vo vypnutom stave a prepojíme ich ešte pred spustením hry. Následne sme zabezpečili prepnutie stavu observerov pri zmene stavu pozorovaného objektu. Čo sa však stane, ak vypnutú žiarovku budeme chcieť napojiť na zapnutý vypínač počas behu programu? Budeme musieť zosynchronizovať ich stavy.

Túto funkcionalitu vieme vyriešiť niekoľkými spôsobmi. Na dnešnom cvičení si ukážeme jednoduché riešenia, kde spolu s notifikáciou príde aj informácia o pozorovanom objekte. Naša implementácia bude čiastočne redundantná, avšak slúži najmä ako ukážka možných riešení takéhoto problému.

### 1.1. Rozšírenie konštruktora
Pridajte do konštruktora `LightBulb` referenciu na `PowerSwitch`, ku ktorému bude pripojený. Následne z konštruktora viete priamo dať subscribe na tento objekt, ako aj získať aktuálny stav vypínača. Takéto riešenie by však umožnilo vytvorenie iba žiaroviek, ktoré sú už pripojené na vypínač. Preto ošetrite prípad, že konštruktor dostane `null` referenciu, a pridajte aj metódu, ktorá umožní pridať zdroj až po spustení programu.

### 1.2. Úprava rozhrania
Ďalšia možnosť je rozšíriť metódu `Notify` definovanú rozhraním `IObserver` o parameter, ktorý bude obsahovať referenciu na pozorovaný objekt. Cez túto referenciu môžete potom získať aktuálny stav pozorovaného objektu. Upravte najprv deklaráciu metódy v rozhraní `IObserver`:

```
void Notify(IObservable observable)
```

Následne upravte definíciu metódy v triede `LightBulb` a volanie v triede `PowerSwitch` tak, aby ste mohli získať informáciu o stave vypínača (či je zapnutý alebo vypnutý). Pri implementácii narazíte na problém nekompatibility jednotlivých rozhraní triedy `PowerSwitch`, ktorý viete vyriešiť [pretypovaním](https://learn.microsoft.com/en-us/dotnet/csharp/programming-guide/types/casting-and-type-conversions) alebo použitím [kľúčového slova `as`](https://learn.microsoft.com/en-us/dotnet/csharp/language-reference/operators/type-testing-and-cast#as-operator).

## 2. krok: Abstraktné triedy

Naposledy sme si vytvorili triedy `LightBulb` a `PowerSwitch`, ktoré mali časť funkcionality rovnakú, dnes sa pozrieme na to, ako by sme mohli túto časť dať dokopy.

Vytvorte si abstraktnú triedu `AbstractSwitchable`, ktorá nech implementuje `ISwitchable`. Trieda bude definovať spoločnú funkcionalitu tried `LightBulb` a `PowerSwitch`, teda konkrétne metódy `Toggle()`, `IsOn()`, `TurnOn()` a `TurnOff()`.

Z `LightBulb` presuňte definíciu metód `Toggle()`, `IsOn()`, `TurnOn()` a `TurnOff()` do `AbstractSwitchable` a nastavte dedenie medzi triedami (samozrejme tieto metódy vymažte aj z `PowerSwitch`). Presuňte aj všetky podporné členské premenné. Teoreticky by ste mohli do abstraktnej triedy pridať aj metódu `Draw()`, tú ale budeme riešiť na ďalšom cvičení.

Pri prepisovaní metód C# neumožňuje prekrývať ľubovoľné metódy, iba tie, ktoré sú na to označené - keyword `virtual` alebo `abstract`. V prípade potreby doplňte slovíčko do deklarácie metód.

**Poznámka:** Pri niektorých metódach v nadtriede nechceme, aby metóda bola volaná mimo triedy, teda mohli by sme im priradiť prístup `private`. Ak však chceme, aby metóda bola prekrytá podtriedami, musíme ju označiť minimálne ako `protected` (dostupné iba v rámci triedy a jej potomkov).

## 3. krok: Nadtrieda a podtrieda

Vytvorte si triedu `CrackedLightBulb`. Jedná sa o žiarovku, ktorá je poškodená, a dá sa rozsvietiť iba niekoľkokrát. Ako predka (rodiča) zoberte `LightBulb`.

Pre zabezpečenie potrebnej funkcionality prekryte metódu `TurnOn()` (nezabudnite upraviť deklaráciu aby obsahovala `virtual`) tak, aby sa žiarovka dala zapnúť iba toľkokrát ako je to povolené. Keby hráč chcel takúto žiarovku zapnúť ešte raz, nič sa nestane. 

Prekrytím metódy je schovaný pôvodný kód, ale nie je úplne stratený: pomocou kľúčového slova `base` sa k nemu vieme dostať, napríklad:

```
base.TurnOn();
```

Samozrejme pre ukladanie počtu možných zapnutí potrebujete novú členskú premennú (typu `int`), zadefinujte ju teda a pridajte aj nový konštruktor. V konštruktore využite kód, ktorý ste už implementovali v `LightBulb`. Konštruktory je možné vzájomne volať pomocou `this` (v rámci jednej triedy) a `base` pre konštruktor nadtriedy:

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

**Poznámka:** Nezabudnite funkcionalitu upraviť tak, aby využívala volanie `TurnOn()` ak ste tak ešte neurobili.

## 4. krok: Cometh forth Mine Own Apprentice

Nadišiel čas, aby sme do našej hry pridali hlavnú postavu.

Vytvorte si triedu `Player`, ako animáciu zatiaľ použite [`player.png`](lab05/player.png). Hlavnou vlastnosťou hráča bude pohyb v hernom svete v rôznych smeroch, pre hráča však nezadefinujeme viac animácií. V prípade, že sa hráč otočí, animáciu otočíme. To síce pridá výpočtovú záťaž do našej hry, tá by ale mala byť zvládnuteľná pre naše počítače. Ak sa naša postavička hýbať nebude, zastavíme aktualizáciu spritov, zatiaľ však túto funkcionalitu nemusíte implementovať.

Do triedy okrem podpory animácie pridajte aj podporu pre interné ukladávanie aktuálnej pozície hráča ako aj pre aktualizáciu týchto údajov (metóda `UpdatePosition(int dx, int dy)` podľa diagramu nižšie). Inšpirovať sa môžete triedami `Bomb` a `LightBulb`. Opakujúci sa kód budeme riešiť na ďalšom cvičení. Ak ste postupovali správne, tak pri vykreslení hráča bude stále prebiehať animácia, aj keď postava ostane stáť na svojom mieste.

**Poznámka:** [Ak si chcete vytvoriť vlastnú postavu, môžete tak urobiť na tejto stránke.](https://sanderfrenken.github.io/Universal-LPC-Spritesheet-Character-Generator/#?body=Body_color_light&head=Human_male_light)
