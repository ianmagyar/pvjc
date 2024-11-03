# Cvičenie 7: Svet a jeho gravitácia

Na minulom cvičení sme do nášho projektu pridali niekoľko abstrakcií a tým sme zovšeobecnili riešenie tak, aby sme mohli pridávať ďalšie funkcionality. Na druhej strane však práca s objektmi je naďalej neintuitívne, keďže ich musíme vytvárať po jednom a musíme manuálne riešiť aj ich vykresľovanie a aktualizáciu. Na dnešnom cvičení pridáme prirodzenejšiu interakciu s objektmi v hernom svete, zabránime tomu, aby sa niektorí aktori vznášali a sfinalizujeme pohybe hráča.

## 1. krok: What a Wonderful World

Metódu `LoadContents()` sme už zjednodušili (vytváranie actorov neskôr automatizujeme), ale v triede `Game1` nám ešte ostávajú metódy `Update()` a `Draw()`, v ktorých máme iba volania príslušných metód nad vybranými objektmi. Nebolo by však jednoduchšie tieto volania zabaliť do jednotnej štruktúry? Presne to urobíme v tomto kroku.

Minulý týždeň sme už vytvorili rozhranie `IWorld` a triedu `GameWorld`, ktoré ale ostali prázdne. Svet bude v skutočnosti zatiaľ len zoznam actorov, ktorí sa nachádzajú v ňom, v konštruktore inicializujte všetky potrebné členské premenné. Okrem zoznamu aktuálnych actorov, zadefinujte si aj zoznam pre actorov, ktorí majú byť pridaní do sveta. Tento zoznam neskôr použijete v `Update()`.

### 1.1. krok: Rozhranie `IWorld`

Do rozhrania `IWorld` pridajte nasledovné deklarácie:

```
void AddActor(IActor actor);
void RemoveActor(IActor actor);
List<IActor> GetActors();
IActor? GetActor(string name);
void Draw(SpriteBatch spriteBatch);
void Update(GameTime gameTime);
int GetHeight();
int GetWidth();
```

### 1.2. krok: Trieda `GameWorld`

V triede `GameWorld` implementujte metódy rozhrania `IWorld` naledovným spôsobom:

* Pridajte metódy `AddActor(IActor actor)` a `RemoveActor(IActor actor)` pre pridávanie, resp. vymazanie actora zo sveta. Nezabudnite zavolať príslušné metódy z triedy `AbstractActor`.
* Pridajte metódu `List<IActor> GetActors()`, ktorá vráti zoznam všetkých actorov vo svete.
* Implementujte metódu `IActor? GetActor(string name)`, ktorá vráti actora podľa jeho mena alebo vráti `null`.
* Implementujte metódy `Draw(SpriteBatch spriteBatch)` a `Update(GameTime gameTime)`, ktoré iba zavolajú príslušné metódy z tried jednotlivých actorov. V `Update` okrem samotnej aktualizácie stavu objektov pridajte do zoznamu actorov všetky objekty zo zoznamu actorov, ktorí majú byť pridaní do sveta. Podobne vymažte actorov, ktorí majú byť zo sveta vymazaní (použite metódu z `IActor`, ktorá poskytne informáciu o potrebe mazania).
* Posledné dva gettery majú vrátiť rozmery sveta (nemusia byť totožné s rozmermi okna). Pridajte potrebné členské premenné a definujte tieto metódy.

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

### 1.3. krok: Použitie `GameWorld` objektu
Do triedy `Game1` pridajte novú členskú premennú typu `GameWorld`, ktorá umožní prácu s objektmi hry. Upravte metódu `LoadContent` tak, aby ste nevytvárali členské premenné pre každý jeden objekt v svete. Namiesto toho, si vytvorte lokálne premenné, a pridajte ich do `GameWorld` objektu. Niektoré objekty možno naďalej potrebujete spracovať pred pridaním do sveta, napríklad ako pri prepojení zapínača a žiarovky. Takisto prepíšte aj metódy `Update` a `Draw` tak, aby len zavolali príslušné metódy z `GameWorld`.

## 2. krok: There are No Ghosts Here
Od štvrtého cvičenia už dokážeme v našej hre zapínať vypínač stlačením klávesu *E*, avšak od momentu, kedy sme pridali do hry hráča, je takéto riešenie dosť neprirodzené. Upravte teda svoje riešenie tak, aby sme vypínač vedeli zapínať iba v momente, ak sa ho hráč dotýka. K tomu možno potrebujete urobiť hneď niekoľko zmien...

Upravte implementáciu triedy `PowerSwitch` tak, aby sa vypínač dal zapnúť iba v prípade, ak sa ho hráč dotýka. Pri implementácii môžete rátať s tým, že hlavná postava hry bude mať vždy meno *player*.

## 3. krok: Newton's Legacy

Píše sa rok 1687 a Sir Isaac Newton práve publikoval dielo, ktoré zmenilo svet, s názvom *Principia*. Zažite tento slávny moment a objavte gravitáciu!

### 3.1. krok: Aaaand `Action<T>`
Ako aj v predošlom kroku, aj teraz začneme implementáciu s prípravnými prácami. Najprv pridáme pomocnú triedu, ktorá bude zabezpečovať pád smerom k zemi pre vybraných aktorov. V priečinku *Commands* si vytvorte rozhranie `IAction`:

```
public interface IAction<T>
{
    public void Execute(T value);
}
```

Ako už deklarácia napovedá, jedná sa o niečo podobné ako `ICommand`, ale je tu jeden rozdiel - v rámci metódy `Execute` odovzdávame parameter. Tento vieme využiť pri našej implementácii pádu, kde už nebudeme potrebovať vytvárať stále nový objekt pre každého aktora ale použijeme stále ten istý. Keďže chceme, aby implementácia bola všeobecná, využijeme tu typový parameter `T`. To znamená, že síce vieme, že objekty `IAction` budú vždy pracovať s rovnakým typom objektov, nevieme zatiaľ, aký to bude konkrétny typ. Práve preto konkrétny typ nahradíme premennou `T`, ktorú naplníme hodnotou až pri volaní.

Pridajte si do *Commands* aj triedu `Fall`, nech implementuje `IAction`. Máme tu dva spôsoby definície (použite druhý):

```
public class Fall : IAction<IActor>
{
    public void Execute(IActor value)
    {
        //magic with actor
    }
}
```

```
public class Fall<T> : IAction<T> where T : IActor
{
    public void Execute(T value)
    {
        //magic with something, that implements IActor
    }
}
```

Druhý zápis určí, že `Fall` bude pracovať s objektmi všeobecného typu, vieme ale, že tento typ bude implementovať rozhranie `IActor` (`T : IActor`). Takýmto spôsobom sa vyhneme unboxingu a samozrejme aj zbytočnému vytváraniu množstva objektov, čo je hlavná výhoda návrhového vzoru *Action*. To však neznamená, že *Command* je zlý, ešte sa nám bude hodiť.

Implementujte v triede `Fall` pád. Pri implementácii môžete vychádzať z triedy `Move`, pričom viete, že `Fall` bude mať vždy rovnaké hodnoty kroku a smeru. Pri definícii metódy `Execute` zabezpečte to, aby aktor nikdy nepadal mimo herného sveta, teda svoj pád ukončí nejneskôr ak dosiahne spodok sveta (prekážky a steny pridáme neskôr). Metódu pre aktualizáciu pozície actora pridajte priamo do rozhrania `IActor` (a do triedy `AbstractActor`).

### 3.2. krok: `IPhysics` a `Gravity`
Gravitácia je fyzikálny jav, podporu ktorých sme už pripravili v triede `GameWorld` aj `AbstractActor`, spomeňte si na metódy typu `IsAffectedByPhysics()`, resp. `SetPhysics()`. Teraz si vytvoríme triedu `Gravity`, ktorá bude reprezentovať základnú fyziku platnú v našej hre.

Najprv znova začneme definíciou rozhrania, tentokrát to bude `IPhysics` (naďalej pracujeme v priečinku *Commands*). Jedná sa o rozšírený `ICommand`, je tu navyše metóda `SetWorld`. Táto sa zavolá automaticky pri nastavení fyziky sveta a dostanete v nej referenciu na `IWorld`, v triedach implementujúcich rozhranie si ju uložíme, keďže ju budeme potrebovať.

```
public interface IPhysics : ICommand
{
    void SetWorld(IWorld world);
}
```

Následne implementujte triedu `Gravity`, ktorá implementuje toto rozhranie. Niekoľko poznámok:
* Každý actor implementuje 2 metódy - `bool IsAffectedByPhysics()` a `void SetPhysics(bool isPhysicsEnabled)` - pomocou nich dokážeme regulovať, či bude daný actor ovplyvnený fyzikou alebo nie - môžeme takto nechať nejaký predmet lietať vo vzduchu.
* `IWorld` má v sebe metódu `GetActors()` - táto vráti zoznam všetkých aktorov vo svete.
* Na pád využite `Fall`.
* V cykle, napríklad `foreach`, si implementujte pád, nezabudnite zohľadniť, či je daný predmet ovplyvnený gravitáciou.

### 3.3. krok: Falling Down
Teraz už konečne môžeme gravitáciu pridať do našej hry, k tomu potrebujete urobiť dve zmeny:

1. pridajte gravitáciu do triedy `GameWorld`, nezabudnite aktualizovať aj metódu `Update()`;
2. upravte hráča (a prípadne ďalšie triedy) tak, aby bol ovplyvnený fyzikou (využite metódu `void SetPhysics(bool isPhysicsEnabled)`).

Teraz si môžete skontrolovať vašu implementáciu, ak ste postupovali správne, tak hráč začne padať hneď na začiatku hry bez ohľadu na to, kde ho inicializujete. Zároveň pohyb hore pomocou šípky už nebude fungovať, keďže gravitácia tento pohyb zneguje. Pohyb dole je vyriešený tiež pádom, takže tieto dva smery môžete vymazať z implementácie.

## 4. krok: Might As Well Jump!
Síce pohyb hore sme už zrušili, náš hráč sa ale niekedy potrebuje dostať vyššie vo svete. Práve preto by bolo vhodné pridať možnosť skoku. Je na vás, ako túto funkcionalitu zabezpečíte, ale základný princíp je asi jasný:

* po stlačení niektorého klávesu (napríklad *Space*) hráč vyskočí;
* pri skákaní hráč postupuje smerom hore až kým nedosiahne maximálnu výšku skoku;
* následne začne padať.

Aby ste vedeli prekonať gravitáciu, tak potrebujete vypnúť fyziku pre hráča pri začatí skoku, a zase ju zapnúť pri dosiahnutí maximálnej výšky skoku. Takisto môžete pridať funkcionalitu, že ak hráč narazí do niečoho, tak jeho skok končí. Výška skoku musí byť konečná v každom prípade. V prípade záujmu (a/alebo nudy) môžete implementovať aj dvojskok.
