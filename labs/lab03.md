# Cvičenie 3: Prvé kroky vo vývoji hry

Od dnešného cvičenia sa budeme zaoberať vývojom jednoduchej hry, kde herný engine už budete mať vytvorený a úlohou bude rozšíriť hru o postavy a hernú logiku. Na dnešnom cvičení sa oboznámime so štruktúrou rozsiahlych projektov, možnosťami využitia externých zdrojov a základmi znovapoužitia kódu cez dedenie.

## 1. krok: Nastavenie projektu

Vo vybranom IDE vytvorte si nový projekt .NET console application. Pri vytvorení projektu dajte už rovno vygenerovať aj hlavnú triedu `Program`.

Následne potrebujete do tohto projektu pridať engine ako balík. Balíky sú menšie celky kódu, ktoré podporujú ďalší vývoj, a môžete si ich stiahnuť z externých serverov alebo pridať ich ručne z vlastného počítača. Pre tento predmet bol vytvorený vlastný balíčkovací server, kde bude dostupná vždy aktuálna verzia herného enginu (pravdepodobne sa ale nebude často meniť). Aby ste si ho vedeli pridať do projektu, postupujte nasledovne (popis je pre Visual Studio, ale obdobne to funguje vo Visual Studio Code):

1. v menu otvorte `Tools -> NuGet Package Manager -> Package Manager Settings`;
2. do package source pridajte (kliknite na zelený znak `+`) `http://hades.cit.fei.tuke.sk:8080/v3/index.json`;
3. kliknite pravým tlačidlom na projekt a zvoľte `Manage NuGet Packages`;
4. v pravom hornom rohu si nastavte package source na `Hades` alebo `all`;
5. nájdite si balíček `Merlin2d` a nainštalujte ho.

## 2. krok: Spustenie hry

V ďalšom kroku vyskúšame, či ste všetko nastavili správne. Obdobne ako v C, každý program v C# musí mať definovaný vsupný bod (main funkcia). Štandardne je to funkcia `static void Main(string[])` v triede `Program` (dá sa to zmeniť v nastaveniach projektu).

Samotné vykresľovanie a hernú slučku sme už pre vás pripravili. Teraz si vytvoríte okno v ktorom pobeží hra.

Podobne ako v prípade každej triedy v objektovo orientovanom programovaní, samotné okno hry bude reprezentované cez triedu a k vytvoreniu hry potrebujeme vytvoriť najprv inštanciu tejto triedy a následne ho spustiť. V našom prípade celú hru bude zastrešovať trieda `GameContainer`, ktorú nájdete v balíku `Merlin2d`. Do `Main` funkcie pridajte nasledovný kód:

```
GameContainer container = new GameContainer(name, width, height); //constructor, creates new instance of the game

//put initialization code here

container.Run(); //start the game, infinite loop, no change is accepted after this until the game ends

```

kde konštruktor triedy `GameContainer` má tri parametre, ktoré môžete nastaviť na vhodné hodnoty:

* `name` - reťazec, názov okna;
* `width` a `height` - celé čísla, rozmery okna v pixeloch.

Podobne ako v C a v Pythone, aj tu je potrebné deklarovať importované knižnice. C# na to využíva tzv. *namespace* (menný priestor). Po jeho importovaní sú dostupné všetky triedy a ich verejné metódy ktoré obsahuje.

`GameContainer` sa vám vyznačil na červeno - prostredie ho ešte nepozná. Keď ukážete myšou na červenou vyznačený kód, objaví sa vedľa neho žiarovka s červeným `X`. Kliknite na ňu a zvoľte prvú možnosť - `using Merlin2d.Game`. Ide o to, že IDE začne vyhľadávať možné definície chýbajúcej triedy, a ponúkne vám všetky možnosti, ktoré nájde.

**Poznámka:** ak niekto pracuje v Linuxe alebo na Macu, tento import musí pravdepodobne spraviť ručne:

```
using System;
using Merlin2d.Game;

namespace Merlin
{
    class Program
    {
        static void Main(string[] args)
        {
        	// your code
```

Ak ste postupovali správne, po spustení projektu sa vám zobrazí čierne okno.

## 3. krok: A Brave New World

Aby sa celá naša hra neodohrávala vo vákuu, v ďalšom kroku pridáme mapu. K tomu si potrebujete stiahnuť niekoľko textúr a ďalších zdrojov, ktoré [nájdete v `resources.zip`](lab03/resources.zip).

Aby tieto pomocné súbory boli dostupné v rámci projektu, potrebujete si ich nakopírovať do priečinka s projektom, nech výsledná cesta vyzerá nasledovne: `projectFolder/resources`.

Teraz si v IDE v okienku `Solution Explorer` uvidíte čerstvo pridané súbory, ktoré ešte potrebujete manuálne pridať do projektu. Vyznačte všetky súbory (nestačí priečinok) a v záložke `properties` nastavte `copy to output directory` na `always`. Alternatívne môžete tieto súbory nakopírovať do priečinka `bins` v projektu.

Načítanie mapy budeme riešiť vo funkcii `Main`: na miesto, kde je v komentári napísaná inicializácia, pridajte nasledovný kód:

```
container.SetMap("resources/maps/map01.tmx");
```

Po opätovnom spustení hry namiesto čierneho pozadia teraz uvidíte mapu.

## 4. krok: Actors, Actors Everywhere

S herným prostredím dokážete komunikovať pomocou rôznych rozhraní, jedným z nich je `Actor`. V tejto prvej časti sme pre vás pripravili abstraktnú triedu `AbstractActor`, ktorá už časti tohto rozhrania implementuje.

Vytvorte si novú triedu ktorá bude predstavovať kotlík `Cauldron`, ktorá bude rozširovať `AbstractActor`. Aby projekt bol lepšie štrukturovaný, vytvorte nový priečinok `actors`, do ktorého pridáte nový súbor, v ktorom zadefinujete triedu `Cauldron`. Súbor zvyčajne nesie názov triedy a takto sa vygeneruje defaultná prázdna trieda. Rozširovanie triedy `AbstractActor` vyriešime cez dedenie, t.j. po názve triedy zadáme triedu, od ktorej dedíme nasledovne:

```
public class Cauldron : AbstractActor
{

}
```

V tomto momente vám IDE podčiarkne `Cauldron`. Je to z toho dôvodu, že `AbstractActor` definuje funkcionalitu, ktorú musí implementovať každá podtrieda, trieda `Cauldron` však to ešte nerobí. Ukážte naň myšou a uvidíte tam niekoľko možností, zvoľte *Implement abstract class*. Toto za vás vygeneruje chýbajúce funkcie definované rozhraním `Actor`, ktoré abstraktná trieda `AbstractActor` ešte neimplementuje.

Objaví sa nasledovný kód:

```
public override void Update()
{
    throw new NotImplementedException();
}
```

O výnimkách sa dozviete neskôr, zatiaľ stačí vedieť, že ak sa počas behu dostane program na takéto miesto, bez vhodného ošetrenia spadne.

Vymažte riadok vyvolávajúci výnimku (kde je `throw`) a nechajte tam iba prázdnu definíciu metódy.

V `Main` funkcii si vytvorte nový objekt triedy `Cauldron`.

```
Cauldron cauldron = new Cauldron();
```

Skúste spustiť program, stalo sa niečo? Ak ste postupovali správne, tak sa zobrazila akurát mapa, ale žiadny kotlík. Prečo?

## 5. krok: Animácie
Na to, aby sme kotlík uvideli v hre, musíme zadefinovať jeho grafickú reprezentáciu. V 2D počítačovej grafike sa často používajú tzv. [sprity](https://en.wikipedia.org/wiki/Sprite_(computer_graphics)), ktoré sú dvojdimenzionálne bitmapy, ktoré sú zakompované do väčšej scény. V našom prípade sú animácie vytvorené sekvenciou obrázkov uložených vedľa seba v jednom `.png` súbore. Ako vstupné parametre v konštruktore budú cesta k spritesheet a výška a šírka jedného obrazu (výšku a šírku vypočítate na základe `png` súboru).

Stále vo funkcii `Main` vytvorte animáciu pre kotlík a pridajte ju do nášho objektu:

```
Animation animation = new Animation("resources/cauldron.png", 64, 100);
cauldron.SetAnimation(animation); //set animation for cauldron
cauldron.SetPosition(100, 100); //set position of the cauldron in the game world

```

Teraz sme už koltík síce vytvorili, aj sme zadefinovali jeho animáciu, stále ale nie je súčasťou samotnej hry. Potrebujeme ho teda pridať do sveta hry a to nasledovne:

```
container.GetWorld().AddActor(cauldron);
```

Keď teraz spustíte hru, mal by sa na obrazovke zjaviť kotlík, ktorý bude lietať niekde v prostredí. Grafická reprezentácia je však naďalej statická, keďže animáciu ešte potrebujem spustiť pomocou príkazu `animation.Start()`.

## 6. krok: Magic Potion

Kotlík by nám bol sám o sebe dosť nanič, ale môžeme ho použiť na varenie kúzelného elixíru, k tomu ale budeme potrebovať počas hry meniť jeho stav - ak teda nejaký vôbec zadefinujeme.

Engine aktualizáciu objektov rieši úplne jednoducho: každú sekundu 60x zavolá funkciu `Update()` u každého actora pridaného do sveta, a následne vykreslí scénu. Vďaka takémuto riešeniu vieme naše objekty postupne aktualizovať a vykresľovať meniaci sa svet, aby hráč bol takisto informovaný o týchto zmenách.

Pridajte do kotlíka nasledujúcu funkcionalitu:

* úvodná animácia je pre každý kotlík rovnaká - presuňte kód ktorý ju rieši z `Main` funkcie do konštruktora kotlíka.
* pri varení je potrebné udržať teplotu, pridajte teda funkcionalitu, ktorá bude riešiť tento problém:
  * nech je teplota reprezentovaná celým číslom;
  * pridajte metódy `public int GetTemperature()` a `public void ChangeTemperature(int delta)`;
  * zabezpečte programovo, aby kotlík nemohol vychladnúť pod 20°C;
  * nech kotlík každé 2 sekundy schladne o 1°C;
  * ak je teplota kotlíka vyššia ako 60°C, zmeňte animáciu na `cauldron_hot.png` (dostupná v `resources`);
  * ak teplota prekročí 100°C, zmeňte animáciu na `cauldron_broken.png`, nastavte teplotu na 20°C, a nech kotlík ignoruje ďalšiu zmenu teploty;
  * vyskúšajte meniť teplotu tak, aby sa prejavila zmena animácií (zatiaľ z `Main` alebo vhodným nastavením počiatočnej teploty).

**Poznámka:** Pre reprezentáciu vnútorného stavu využívajte členské premenné. Alternatíva je využiť [properties](https://learn.microsoft.com/en-us/dotnet/csharp/programming-guide/classes-and-structs/properties).

**Poznámka(2):** Animácie si môžete dopredu pripraviť a už ich len následne podľa potreby meniť - nenačítavajte stále animáciu nanovo.

**Poznámka(3):** ak si chcete skontrolovať, čo sa deje, viete si do konzoly vypísať správu nasledovne:

```
Console.WriteLine("string");
```

Rôzne spôsoby prevodu čísel na string vám ochotne ukáže Google.

## 7. krok: Is It Hot in Here, or Is It Just Me?

Do hry pridajte triedu `Stove`, ktorá rovnako dedí od `AbstractActor`, s animáciou `resources/stove.png`. Táto bude predstavovať pec na ktorej sa bude kotlík hriať.

Ďalej do tejto triedy pridajte metódu `public void AddCauldron(Cauldron cauldron)`, ktorá nastaví referenciu na kotlík, ktorý bude pec zohrievať. Pec zahreje kotlík každú sekundu o 1°C. Či bol kotlík nastavený viete overiť porovnaním referencie s `null`:

```
if (someObject != null)
{
    //someObject exists
}
```

Ak toto neskontrolujete a kotlík nastavený nebol, program spadne.

Vyskúšajte, či všetko funguje - v `Main` pridajte pec, nastavte jej kotlík a sledujte, ako sa zohrieva. Ak ste postupovali správne, mal by po nejakom čase vykypieť.

Pridajte do `Stove` dve ďalšie metódy: `public void AddWood()` a `public void RemoveWood()` a pridajte k nim zodpovedajúcu funkcionalitu:

* nech každý kus dreva zohrieva kotlík o 1°C za sekundu;
* nech v peci môže byť 0-3 kusy dreva;
* ak v peci nie je žiadny kus dreva, zmeňte animáciu na `stove_cold.png`; ak sa pridá kus dreva, zmeňte animáciu späť;
* predpokladajme, že pec zatiaľ horí stále, keď má drevo (zmeníme to neskôr).
