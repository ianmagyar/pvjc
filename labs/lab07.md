# Cvičenie 7: Nový začiatok, Factory a mapa

Nadišiel čas na vytvorenie vážnejšej hry, ktorú implementujeme v novom projekte. Pri vytvorení projektu postupujte rovnako, ako [pri prvom projekte](https://github.com/ianmagyar/pvjc/blob/master/labs/lab03.md), tentokrát však použijeme novú knižnicu `MerlinCore`. Táto je dosť podobná knižnici, na ktorú ste už boli zvyknutí, ale je tu niekoľko zmien: niektoré veci, ktoré ste mali doteraz implementované (napr. `AbstractActor`) už implementované nie sú, zostali len rozhrania. Tiež bola pridaná podpora pre nové rozhrania (napr. inventár, automatické vytváranie actorov z mapy).

[Celý kód knižnice `MerlinCore` nájdete tu.](https://github.com/Kellei2983/MerlinCore)

Prichystajte si štruktúru projektu podobným štýlom, [ako sa vyžaduje v druhom zadaní](https://github.com/ianmagyar/pvjc/blob/master/assignments/assignment2.md):

* vytvorte si priečinky `Actors`, `Commands` a `Items`
* nezabudnite si pridať priečinok `resources` - je tu ale zmena, animácie budú po novom uložené v priečinku `resources/sprites`. Pred spustením nového projektu nezabudnite nastaviť aj kopírovanie zdrojových spritov a máp (*Copy always*).
* skopírujte si triedy `Player`, `Enemy`, `Gravity` a všetky ich závislosti (nezabudnite upraviť namespaces a urobiť ďalšie potrebné zmeny). Ak nepriateľa ste zatiaľ neimplementovali, pracujte iba s triedami `Player` a `Gravity`. Nezabudnite tiež pridať možnosť ovládať hráča. Ak nemáte vytvorený projekt s implementáciami týchto tried, [môžete si stiahnuť ukážkové riešenie tu.](lab07/Game.zip)

**Poznámka:** Namiesto rozhrania `Command` nová knižnica dodržuje konvencie mien rozhraní a nájdete tu teda rozhranie `ICommand`.

## 1. krok: `AbstractActor`

Vytvorte si abstraktnú triedu `AbstractActor` v priečinku `Actors` ktorá bude implementovať rozhranie `IActor`. Implementujte ju nasledovne, vnútornú štruktúru navrhnite tak, aby vhodne podporovala funkcionalitu:

* konštruktor:

    * `AbstractActor()` - nastavte prázdny `string` pre meno postavy, nech sa predíde problémom
    * `AbstractActor(string name)` - nastavte meno postavy podľa parametra

* `string GetName()`, `void SetName(string name)` - riadia prístup k mene actora
* `int GetX()`, `int GetY()` - vráti x-ovú a y-ovú súradnicu (ľavý horný vrchol animácie)
* `int GetHeight()`, `int GetWidth()` - vráti rozmery actora (viete ich získať z animácie)
* `void SetPosition(int posX, int posY)` - nastaví pozíciu postavy
* `void OnAddedToWorld(IWorld world)` - nastaví referenciu na svet v ktorom sa actor pohybuje - volané automaticky z `GameWorld`
* `IWorld GetWorld()` - vracia referenciu na svet actora
* `Animation GetAnimation()`, `void SetAnimation(Animation animation)` - umožňujú prístup k animácii
* `bool IntersectsWithActor(IActor other)` - zistí kolíziu s iným actorom, ktorý dostane ako parameter. Aby došlo ku kolízii, stačí aby sa animácie prekrývali v jednom bode, pričom predpokladáme, že obidvaja actori majú tvar obdĺžnika (bounding box).
* `void SetPhysics(bool isPhysicsEnabled)`, `bool IsAffectedByPhysics()` - informácie o fyzike ktorá ovplyvňuje postavu (napr. gravitácia)
* `void RemoveFromWorld()` - slúži na nastavenie príznaku, či daný actor má byť zo sveta odstránený. Všetci označení actori sú na konci iterácie v hernej slučke odstránení naraz.
* `bool RemovedFromWorld()` - zistí, či daný actor bol označený na odstránenie zo sveta. `GameWorld` volá túto funkciu v rámci hernej slučky po zavolaní `Update()`.
* `void Update()` označte ako abstraktnú metódu

**Poznámka:** Pre lepšie zorientovanie, nižšie je uvedená implementácia `GameWorld.Update()`, pridávanie a odstraňovanie actorov je robené dávkovo (batch):

```
//GameWorld.cs
internal void Update(long i)
{
    actorsToAdd.ForEach(actor =>
    {
        this.actors.Add(actor);
        actor.OnAddedToWorld(this);
    });
    actorsToAdd.Clear();

    triggers.ForEach(trigger => trigger.Update());
    actors.ForEach(actor => actor.Update());

    actors.RemoveAll(actor => actor.RemovedFromWorld());
    if (gameLevelPhysics != null)
    {
        gameLevelPhysics.Execute();
    }
}
```

Pre testovanie riešenia môžete použiť nasledovný kód v metóde `Main`:

```
GameContainer container = new GameContainer("Game window", 500, 500);

Player player = new Player(100, 300, 2);
player.SetPhysics(true);
container.GetWorld().AddActor(player);
container.GetWorld().SetPhysics(new Gravity());

Console.WriteLine(player.IsAffectedByPhysics());

container.SetMap("resources/maps/map01.tmx");

container.Run();
```

## 2. krok: Let's Build a Factory

Pri spustení sa vám zobrazí správa *Manually added actors were removed, use AddInitAction to manually add actors for debug.* a hráč sa nezobrazí. Je to z toho dôvodu že v novom riešení chceme všetky postavy nastaviť priamo v mape a nepridávať ich manuálne. Keby ste chceli niečo len otestovať, tak pre manuálne pridávanie objektov môžete využiť `AddInitAction` podobne ako pri centrovaní postavy (ukázané neskôr).

Návrhové vzory [*Factory Method*](https://en.wikipedia.org/wiki/Factory_method_pattern) a [*Abstract Factory*](https://en.wikipedia.org/wiki/Abstract_factory_pattern) slúžia na automatické vytváranie objektov. Vytvorte si triedu `ActorFactory` v novom priečinku `Factories`, ktorá nech implementuje rozhranie `IFactory`. Cieľom tejto triedy je automaticky vytvárať nové objekty podľa vstupných parametrov: vieme takto zabezpečiť vytvorenie a pridanie actorov načítaných z mapy. Trieda je príkladom návrhového vzoru *Factory Method*, kde definujete metódu `Create`. Táto metóda vytvorí a vracia konkrétneho actora, ktorého sama vytvorí na základe zadaných parametrov a takisto nastaví všetky potrebné atribúty.

Metóda definuje parametre očakávaný typ objektu, ďalší je jeho meno a následne sú uvedené jeho pozície. V metóde zatiaľ vytvorte inštanciu triedy `Player` na základe druhého parametra (meno postavy, pričom viete že hlavná postava sa bude volať `Merlin`). Alternatívne môžete použiť aj typ postavy (prvý parameter `actorType` - tu očakávame actora typu `Player`) Keďže všetky triedy, ktoré *Factory Method* vytvára musia mať spoločný interface, môžeme na tomto mieste využiť polymorfizmus (návratový typ `IActor`).

Metódu `Create` nemusíte volať nikde, automaticky to urobí samotná hra. Po supstení však hráča stále nemáme, potrebujeme k tomu upraviť aj mapu.

## 3. krok: tmx

Stiahnite si [editor Tiled](https://www.mapeditor.org) a vytvorte si mapu podľa pokynov nižšie (prípadne si môžete pozrieť nejaký na internete). Zatiaľ môžete upravovať iba pôvodnú mapu `map01.tmx`. Implementácia v `MerlinCore` je jednoduchá, takže neumožňuje využitie komplexnejších prvkov, niekoľko obmedzení:

* 1 tileset pre mapu (ak potrebujete použiť rôzne, spojte ich do jedného obrázku)
* nefunguje render order
* Orientation: orthogonal
* custom properties nie sú (tu je to kvôli `IFactory`, ak potrebujete nastaviť špecifické veci pre jedného actora, identifikujte ho špecifickým menom a v ďalšom bode si pridáme anonymnú funkciu (funkcie), ktorá nám takéto veci nastaví).

Mapa musí obsahovať nasledujúce vrstvy:

* `objects` - každý actor
    * `Name`: meno actora, `Type`: názov triedy, `X`, `Y` - súradnica ľavého horného rohu, rozmery sú ignorované - použité sú rozmery z animácie
* `walls` - vyznačené steny, všetko, čo je vymaľované je (neviditeľná) stena, zvyšok je priechodný
* `background` - toto sa zobrazí ako svet v hre (nakreslite sem aj steny)

Zapojte tu svoju tvorivosť. Zatiaľ stačí ale, ak pridáte hráča nasledovným štýlom:

1. hráča pridáte do vrstvy `objects`
2. nakreslite bounding box s ľubovoľnou veľkosťou (v hre veľkosť boxu sa do úvahy neberie, použije sa veľkosť animácie)
3. nastavte meno a typ objektu v *properties*
4. mapu uložte.

[V rámci cvičenia môžete použiť aj túto mapu.](lab07/map01.tmx)

K úspešnému načítaniu potrebujete ešte nastaviť factory pre hru, metóda `Main` môže vyzerať nasledovne:

```
GameContainer container = new GameContainer("Game window", 500, 500); //constructor, creates new instance of the game

container.SetMap("resources/maps/map01.tmx");
container.GetWorld().SetPhysics(new Gravity());
container.GetWorld().SetFactory(new ActorFactory());

container.Run();
```

## 4. krok: Inicializácia

V `IWorld` je dostupná metóda void `AddInitAction(Action<IWorld> action)`, ktorá očakáva anonymnú funkciu (`void func(IWorld)`) (môžete ju volať aj viackrát a tak pridať rôzne funkcie). Tieto anonymné funkcie sa uložia a vykonajú pri inicializácii hry (na začiatku, pri volaní `Run()`). Pomocou takýchto anonymných funkcií viete poslať príkazy, ktoré potrebujete vykonať pri inicializácii (napr. nastavenie špecifických parametrov pre actorov)

Tiež tu viete nastaviť sledovanie actora (napr. hráča) kamerou:

```
world.CenterOn(actor);
```
