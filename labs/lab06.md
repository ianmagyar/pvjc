# Cvičenie 6: Pohľad pod kapotu

Po pridávaní niekoľkých tried, rozhraní a ďalších konceptov náš projekt pekne rozrastá, čo nám komplikuje život. Vidíme v riešení duplicitný kód a nepríjemnú prácu s objektmi, kde nestačí nám pridať ich do hry, ale musíme dbať aj na ich prípadnú aktualizáciu a ďalšie záležitosti. Nebolo by jednoduchšie, keby nám náš projekt poskytol lepšiu podporu pre často sa opakujúce funkcionality?

Počítačové hry takúto funkcionalitu riešia v hernom engine, ktorý zabezpečuje presne tie časti funkcionality, ktoré sú spoločné pre väčšinu počítačových hier. Na dnešnom cvičení si vytvoríme náznak takéhoto jednoduchého enginu, pričom zjednodušíme aj naše triedy a implementáciu niektorých metód. Cvičenie začneme menším upratovaním, následne použijeme ďalší návrhový vzor na generovanie objektov, a na záver prerobíme prácu so svetom našej hry.

## 1. krok: `AbstractActor`

Na poslednom cvičení sme si ukázali, ako dokážeme spoločnú funkcionalitu niekoľkých tried zapuzdriť do abstraktnej triedy, a tak predísť duplicitným kódom. Konkrétne sme to urobili pre triedy `LightBulb` a `PowerSwitch`, kde funkcionalitu zapájania sme presunuli do abstraktnej triedy `AbstractSwitchable`.

Ak sa ale pozriete na implementáciu všetkých tried, tak vidíte, že stále nám ostáva funkcionalita, ktorá je rovnaká, alebo skoro rovnaká pre všetky triedy, najmä čo sa týka vykresľovania, ale aj reprezentácie herných objektov.

Vytvorte si preto abstraktnú triedu `AbstractActor` v priečinku `Actors`. Vnútornú štruktúru navrhnite tak, aby vhodne podporovala funkcionalitu, môžete pritom vychádzať z existujúcej implementácie tried. Okrem doteraz použitých premenných pridáme aj premennú pre meno objektu, ktorá nám bude slúžiť pre nájdenie konkrétnych objektov v hernom svete počas behu.

Najprv si vytvorte rozhranie `IActor`, s predpísanými metódami:

```
string GetName()
void SetName(string name);
int GetX();
int GetY();
int GetHeight();
int GetWidth();
void SetPosition(int posX, int posY);
void OnAddedToWorld(IWorld world);
IWorld GetWorld();
Texture2D GetTexture();
void SetTexture(Texture2D texture);
bool IntersectsWithActor(IActor other);
void SetPhysics(bool isPhysicsEnabled);
bool IsAffectedByPhysics();
void RemoveFromWorld();
bool RemovedFromWorld();
void Update(GameTime gameTime);
```

Následne napíšte triedu `AbstractActor`, ktorá implementuje rozhranie `IActor` s nasledovnou funkcionalitou:

* konštruktor:

    * `AbstractActor()` - nastavte prázdny `string` pre meno postavy, nech sa predíde problémom. Môžete inicializovať aj ďalšie členské premenné defaultne podľa potreby.
    * `AbstractActor(string name)` - nastavte meno postavy podľa parametra.

* `string GetName()`, `void SetName(string name)` - riadia prístup k mene actora.
* `int GetX()`, `int GetY()` - vráti x-ovú a y-ovú súradnicu objektu (ľavý horný vrchol textúry).
* `int GetHeight()`, `int GetWidth()` - vráti rozmery actora (viete ich získať z textúry).
* `void SetPosition(int posX, int posY)` - nastaví pozíciu postavy.
* `void OnAddedToWorld(IWorld world)` - nastaví referenciu na svet, v ktorom sa actor pohybuje (zatiaľ si vytvorte prázdne rozhranie `IWorld` a triedu `GameWorld`, ktorá toto rozhranie implementuje v novom priečinku `Worlds`). V `AbstractActor` ostáva prázdna.
* `IWorld GetWorld()` - vracia referenciu na svet actora, do triedy pridajte príslušnú členskú premennú.
* `Texture2D GetTexture()`, `void SetTexture(Texture2D texture)` - umožňujú prístup k textúre actora.
* `bool IntersectsWithActor(IActor other)` - zistí kolíziu s iným actorom, ktorý dostane ako parameter. Aby došlo ku kolízii, stačí aby sa textúry prekrývali v jednom bode, pričom predpokladáme, že obidvaja actori majú tvar obdĺžnika (bounding box).
* `void SetPhysics(bool isPhysicsEnabled)`, `bool IsAffectedByPhysics()` - informácie o fyzike, ktorá ovplyvňuje postavu (napr. gravitácia). Na repezentáciu toho, či actora ovplyvňuje fyzika sveta, používajte `bool` členskú premennú.
* `void RemoveFromWorld()` - slúži na nastavenie príznaku, či daný actor má byť zo sveta odstránený. Všetci označení actori budú na konci iterácie v hernej slučke odstránení naraz. Na implementáciu tohto flagu používajte `bool` členskú premennú.
* `bool RemovedFromWorld()` - zistí, či daný actor bol označený na odstránenie zo sveta. Neskôr trieda `GameWorld` zavolá túto funkciu v rámci hernej slučky po zavolaní `Update()`.
* `void Update(GameTime gameTime)` - vytvorte prázdnu implementáciu.
* `void Draw(SpriteBatch spriteBatch)` - rieši jednoduché vykresľovanie textúry.

**Poznámka:** Pre reprezentáciu pozície naďalej môžete používať `Vector2`, alebo to môžete rozdeliť do dvoch celočíselných premenných.

Po definícii abstraktnej triedy si upravte triedy `Bomb`, `AbstractSwitchable` a `Player` tak, aby dedili od `AbstractActor`. Ak je to potrebné, aktualizujte aj triedy `PowerSwitch`, `LightBulb` a `CrackedLightBulb`. Rozšírte konštruktory tak, aby ste každému objektu vedeli priradiť meno.

Pri implementácii `Draw(SpriteBatch spriteBatch)` ste možno narazili na otázku, ktorú implementáciu máte definovať v abstraktnej triede: vykresľovanie statickej textúry, alebo animácie ako v prípade bomby alebo hráča?

Elegantné riešenie tohto problému je vytvorenie ďalšej abstraktnej triedy `AbstractAnimatedActor`, v ktorej implementujete vykresľovanie animovanej textúry tak, ako sme to robili doteraz. Samozrejme k tomu potrebujete vytvoriť aj ďalšie členské premenné. Následne nastavte triedy `Bomb` a `Player` ako podtriedy `AbstractAnimatedActor`, a aktualizujte ich implementáciu podľa potreby. Pri triede `Bomb` upravte aj volanie metódy `Draw` tak, aby ste bombu vykreslili na už známu pozíciu (použite teda jeden parameter ako v prípade ostatných tried, napríklad pre `Player`).

**Poznámka:** Pri `AbstractAnimatedActor` odporúčame vypočítať rozmery jednej snímky z celkových rozmerov textúry a počtu snímok (na základe počtu riadkov a stĺpcov). Nezabudnite potom aktualizovať aj metódy `GetHeight()` a `GetWidth()`.

## 2. krok: Let's Build a Factory

Rozhranie tried sme už pomerne dobre ujednotili, čo nám umožní zjednodušiť spôsob vytvárania jednotlivých objektov. Tento krok zatiaľ sme robili v metóde `LoadContent()` v triede `Game1`, ale predstavte si, že do vašej hry chcete pridať niekoľko svetov resp. máp. V takomto prípade by bolo vhodnejšie vytváranie objektov riešiť cez načítanie základných údajov o mape a umiestnení objektov v nej. Ako prvý krok k takémuto riešeniu si teraz vytvoríme factory pre naše objekty.

Návrhové vzory [*Factory Method*](https://en.wikipedia.org/wiki/Factory_method_pattern) a [*Abstract Factory*](https://en.wikipedia.org/wiki/Abstract_factory_pattern) slúžia na automatické vytváranie objektov. Vytvorte si triedu `ActorFactory` v novom priečinku `Factories`, ktorá nech implementuje rozhranie `IFactory`. Rozhranie nech deklaruje iba jednu metódu:

```
IActor Create(ContentManager content, String actorType, String actorName, int x, int y);
```

Cieľom tejto triedy je automaticky vytvárať nové objekty podľa vstupných parametrov: vieme takto zabezpečiť vytvorenie a pridanie actorov načítaných z mapy. Trieda je príkladom návrhového vzoru *Factory Method*, kde definujete metódu `Create`. Táto metóda vytvorí a vracia konkrétneho actora, ktorého sama vytvorí na základe zadaných parametrov a takisto nastaví všetky potrebné atribúty.

Metóda definuje parametre očakávaný typ objektu, ďalší je jeho meno a následne sú uvedené jeho pozície. V metóde zatiaľ umožnite vytváranie objektov typu `Player`, `Bomb`, `PowerSwitch`, `LightBulb` a `CrackedLightBulb` na základe prvého parametra. Keďže všetky triedy, ktoré *Factory Method* vytvára, musia mať spoločný interface, môžeme na tomto mieste využiť polymorfizmus (návratový typ `IActor`). Pre ďalšie parametre konštruktorov tried si nastavte ľubovoľné predvolené hodnoty. `ContentManager` nastavte priamo ako členskú premennú factory.

Následne upravte implementáciu `LoadContents()` tak, aby nevolala priamo konštruktory jednotlivých tried.

## 3. krok: What a Wonderful World

Metódu `LoadContents()` sme už zjednodušili (vytváranie actorov neskôr automatizujeme), ale v triede `Game1` nám ešte ostávajú metódy `Update()` a `Draw()`, v ktorých máme iba volania príslušných metód nad vybranými objektmi. Nebolo by však jednoduchšie tieto volania zabaliť do jednotnej štruktúry? Presne to urobíme v tomto kroku.

Vytvorte si triedu `GameWorld` v priečinku `Worlds` vo vašom projekte, a pridajte do nej nasledujúcu funkcionalitu (tú neskôr rozšírime):

* Svet bude v skutočnosti zatiaľ len zoznam actorov, ktorí sa nachádzajú v ňom, v konštruktore inicializujte všetky potrebné členské premenné. Okrem zoznamu aktuálnych actorov, zadefinujte si aj zoznam pre actorov, ktorí majú byť pridaní do sveta. Tento zoznam neskôr použijete v `Update()`.
* Pridajte metódy `AddActor(IActor actor)` a `RemoveActor(IActor actor)` pre pridávanie, resp. vymazanie actora zo sveta. Pri vymazaní nezabudnite využiť podobnú funkcionalitu z triedy `AbstractActor`.
* Pridajte metódu `List<IActor> GetActors()`, ktorá vráti zoznam všetkých actorov vo svete.
* Implementujte metódu `IActor? GetActor(string name)`, ktorá vráti actora podľa jeho mena alebo vráti `null`.
* Implementujte metódy `Draw(SpriteBatch spriteBatch)` a `Update(GameTime gameTime)`, ktoré iba zavolajú príslušné metódy z tried jednotlivých actorov. V `Update` okrem samotnej aktualizácie stavu objektov pridajte do zoznamu actorov všetky objekty zo zoznamu actorov, ktorí majú byť pridaní do sveta. Podobne vymažte actorov, ktorí majú byť zo sveta vymazaní (použite metódu z `IActor`, ktorá poskytne informáciu o potrebe mazania).

**Poznámka:** V C# vieme aplikovať rôzne filtre na zoznamy pomocou funkcií, ktoré dostanú ako vstupný parameter filter (tzv. *lambda výraz* alebo referenciu na funkciu).

Najprv si potrebujeme načítať zodpovedajúci menný priestor (`using System.Linq`), následne vieme aplikovať tzv. *extension methods* na rôzne kolekcie, napríklad:

```
List<int> values;
//add some values into the list

List<int> negativeValues = values.Where(x => x < 0).ToList(); //select values smaller than 0, the result is then converted into a list (it is IEnumerable by default)
int smallestValue = values.OrderBy(x => x).First(); //select the smalest value (order them and select the first one)
int firstPositive = values.First(x => x > 0); //select the first positive value from the list
int numberOfPositiveValues = values.Where(x => x > 0).Count(); //count the number of positive values

List<IActor> actors//init and stuff...

IActor actor = actors.Find(a => a.GetName().Equals("player")); //select single entry
```

Samozrejme, dostupných metód je oveľa viac, ako sme tu ukázali, môžete si ich pozrieť napr. cez dopĺňanie kódu alebo na internete.

Existuje aj verzia cyklu `foreach`, ktorá akceptuje lambda výraz, tu by to fungovalo nasledovne:

```
//void DoStuff(IActor actor);
actors.ForEach(a => DoStuff(a));
```

[Viac o lambda výrazoch nájdete v Microsoft dokumentácii.](https://learn.microsoft.com/en-us/dotnet/csharp/language-reference/operators/lambda-expressions)

Do triedy `Game1` pridajte novú členskú premennú typu `GameWorld`, ktorá umožní prácu s objektmi hry. Upravte metódu `LoadContent` tak, aby ste nevytvárali členské premenné pre každý jeden objekt v svete. Namiesto toho, si vytvorte lokálne premenné, a pridajte ich do `GameWorld` objektu. Niektoré objekty možno naďalej potrebujete spracovať pred pridaním do sveta, napríklad ako pri prepojení zapínača a žiarovky. Takisto prepíšte aj metódy `Update` a `Draw` tak, aby len zavolali príslušné metódy z `GameWorld`.
