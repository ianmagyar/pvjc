# Cvičenie 9: Súbory a operátory

Na dnešnom cvičení si dokončíme kúzla a pridáme si ešte zopár rozšírení, ktoré potrebujeme na vyplnenie sveta. Nič zložité, prisaháme.

Programovať môžete vo vlastnom projekte alebo [si stiahnite tento projekt.](lab09/week9.zip)

## 1. krok: File (Not) Found

Minulý týždeň ste si pripravili nástroje na vytváranie kúziel pomocou návrhového vzoru *builder*. Síce riešenie vyžadovalo niekoľko pomocných tried, v konečnom dôsledku ste nemuseli definovať triedy pre každé jedno kúzlo, iba pre dva typy: `ProjectileSpell` a `SelfCastSpell`. Konkrétne kúzla boli už popísané v rámci `SpellDirector` a skladali sa z niekoľkých efektov, pričom každý z nich implementoval `ICommand`.

## 1.1. krok: Potrebné súbory
V tomto kroku upravíme definíciu kúziel a to tak, že zoznam efektov a cenu načítame zo súborov. Vytvorte si teda dva súbory: `spell.csv` a `effects.json` v priečinku `resources` (nezabudnite nastaviť kopírovanie). Súbor `spell.csv` bude obsahovať [hodnoty oddelené čiarkou](https://en.wikipedia.org/wiki/Comma-separated_values) a bude definovať nasledovné atribúty (zároveň prvý riadok súboru):

```
name;spelltype;animationpath;animationwidth;animationheight;effect1,effect2,effect3
```

Konkrétne kúzlo, ktoré postavu zraní a spomalí môže byť zadefinované nasledovne:

```
iceball;projectile;resources/sprites/iceball.png;32;32;Damage-10,SpeedChange-20,
```

Súbor `effects.json` definuje zoznam efektov pre podporu jednotlivých kúziel a predstavuje `JArray`:

```
[
    {
        "name":"directdamage",
        "cost":5
    },
]
```

Obsah súborov upravte podľa vašej implementácie rôznych kúziel.

## 1.2. krok: Načítanie dát
Po vytvorení zdrojových súborov potrebujeme ešte zadefinovať spôsob načítania ich obsahu, k čomu zadefinujeme rozhranie `ISpellDataProvider` a triedu `SpellDataProvider`. Rozhranie nech deklaruje metódy:

```
public interface ISpellDataProvider
{
    public Dictionary<string, SpellInfo> GetSpellInfo();
    public Dictionary<string, int> GetSpellEffects();
}
```

Pre načítanie konkrétnych údajov si vytvoríme triedu `SpellDataProvider`, ktorá implementuje rozhranie `ISpellDataProvider` a návrhový vzor *singleton*. Pre jednoduchosť môžete predpokladať, že súbory budú mať stále rovnaký názov a relatívnu cestu (v rámci projektu).

## 1.3. krok: Načítanie `SpellInfo`
Keby sme postupne načítali len riadky zo súboru `spell.csv`, tak by sme dostali reťazce, ktoré však potrebujeme využiť pre vytvorenie vnútornej reprezentácie pomocou `SpellInfo`. Mohli by sme do `SpellInfo` pridať konštruktor, kde by sa načítaný riadok parsoval, alebo by sme si mohli tento riadok rozdeliť už pri načítaní a nastaviť jednotlivé vlastnosti priamo. Čo keby sme sa ale rozhodli napísať niečo takéto:

```
SpellInfo spellInfo = "name;spelltype;animationpath;animationwidth;animationheight;effect1,effect2,effect3"
```

C# hneď začne protestovať, keďže nerozumie, ako spolu súvisí `SpellInfo` a `string`. V `SpellInfo` však [vieme naprogramovať operátor](https://learn.microsoft.com/en-us/dotnet/csharp/language-reference/operators/user-defined-conversion-operators), ktorý nám zabezpečí konverziu:

```
public static implicit operator SpellInfo(string data)
{
    //magic & stuff
    return spellInfo;
}
```

Na prvý pohľad to vyzerá super, ale implicitný operátor nemusí byť správne pochopený. Dávajte si na to pozor. Možno vhodnejšie by bolo použiť [`explicit`](https://blog.devgenius.io/implicit-and-explicit-operators-c-30d28fb573e0), kde pretypovanie je potrebné zadefinovať ručne. Ďalšia možnosť je samozrejme využiť podpornú knižnicu alebo modul pre spracovanie `csv` súborov. Nech sa rozhodnete pre ktorýkoľvek variant, spustite sa do toho a implementujte metódu `GetSpellInfo()` pre triedu `SpellDataProvider`.

**Poznámka:** `String` viete rozdeliť pomocou `String.Split()`.

## 1.4. krok: Načítanie efektov
Čo sa týka efektov, tu máme mapovanie medzi `JArray` a doteraz neznámym typom.

C# pozná typ `dynamic` (všetko je riešené počas behu), ale je podstatne pomalší. Hodí sa použiť vtedy, keď nevieme pri kompilácii povedať, čo štruktúra bude obsahovať. My to ale vieme, takže nám stačí zadefinovať si novú triedu, ktorú nepotrebujeme, aby bola dostupná nikde inde, iba počas parsovania JSONu.

Vytvorte si v `SpellDataProvider` vnorenú triedu `private class SpellEffect`, ktorá bude slúžiť ako model pre čítanie daného JSONu. Na samotné čítanie použite:

```
JsonConvert.DeserializeObject<List<CustomerJson>>(json);
```

## 1.5. krok: Skrášlenie riešenia
V prípade, že sa objaví pri čítaní hociktorého súboru neplatná hodnota, vyvolajte vhodnú výnimku s informatívnou chybovou správou (asi najvhodnejšie `ArgumentException`). Zabezpečte, aby sa načítali všetky platné riadky (nech program nespadne!). Ak by sa vyskytla iná (neočakávaná chyba), program má spadnúť.

Dokončite funkcionalitu kúziel tak, aby ich mohol hráč po stlačení tlačidla vyčarovať. Môžete si ich nastaviť napríklad na čísla na klávesnici.

## 2. krok: Watch Where You Step
Ako narastá funkcionalita nášho riešenia, potrebujeme vytvárať ďalšie väčšie a väčšie svety, k čomu by bolo vhodné vytvoriť oddelené priestory, medzi ktorými však hráč dokáže prechádzať počas hry. Pre takéto riešenie nám skvele poslúži reprezentácia dverí a spínačov.

Vytvorte si triedy `Door` (dvere), `Switch` (spínač) a `PressurePlate` (tlakový senzor). Konkrétna funkcionalita je na vás, základ by ale mal zabezpečiť, aby sa dvere (`Door`) otvorili/zatvorili pri prepnutí spínača (`Switch`), resp. boli otvorené, keď je stlačený tlakový senzor (`PressurePlate`): stojí na ňom `ICharacter`. Pre inšpiráciu môžete využiť prvé cvičenia a prepojenie objektov `Cauldron` a `Stove`, prípadne `Crystal` a `PowerSource` (nezabudnite na návrhový vzor *observer*).

Pridajte si interface `IUsable` s metódou `void Use(IActor user)`. Nech `Player` dokáže pri stlačení niektorého klávesu použiť `IUsable` prvok, ak je s ním v kontakte (pretínajú sa animácie).

Nepriechodnosť prostredia zabezpečte pridaním a odstránením stien na súradniciach dverí pomocou

```
World.SetWall()
World.Iswall()
World.GetTileHeight()
World.GetTileWidth()
```

Súradnice si musíte prerátať na pozíciu dlaždice v mriežke.

## 3. krok: Did I Ever Tell You the Definition of Insanity?
Síce je pekné, ak hráč dokáže ovládať `Player` objekt, hra by však bola nudná, ak by naňho nečakali žiadne nebezpečia a bol by nesmrteľný. A keďže každá postava je zraniteľná a jej správanie závisí od toho, v akom stave je, pre reprezentáciu tejto skutoťnosti je vhodné využiť [návrhový vzor *state*](https://en.wikipedia.org/wiki/State_pattern). V tomto vzoru budú existovať rôzne implementácie tej istej funkcionality (podobne ako pri *strategy*) a jednotlivé varianty budú zastrešovať osobitné triedy. Hlavná trieda si potom pri volaní otáznej funkcionality (v našom prípade `Update()`) zavolá príslušnú implementáciu z práve platného stavu.

Vytvorte si triedy `LivingState` a `DyingState`, ktoré reprezentujú stav hráča, prípadne podobné riešenie môžete využiť pre `Skeleton`. Keď je hráč nažive, všetko funguje ako doteraz. Keď zomrie (zavolá sa `Player.Die()`), nastavte aktuálny stav na `DyingState` a zmeňte aj animáciu. `DyingState` by mal znemožniť hráča ovládať. Hráča teraz už nemusíte vymazať zo sveta okamžite, môžete však tak urobiť po uplynutí istého času (využite `Update` v triede `DyingState`).

Aktuálny stav hráča viete na obrazovku zobraziť pomocou:

```
Message msg = new Message(String text, int x, int y); //optional arguments: fontSize, duration
GetWorld().ShowMessage(msg);
```

## 4. krok: Box, Box
Do hry pridajte triedu `Box` (nech sa jedná o `ICharacter`), ktorá reprezentuje krabicu, ktorú hráč môže tlačiť vo svete. Samozrejme ak by hráč krabicu tlačil oproti stene, tak krabica a ani hráč sa nepohnú, a nezabudnite aj na to, že každá krabica musí byť ovplyvnená gravitáciou.

**Poznámka:** Môžete pridať aj funkcionalitu, kde hráč môže hodiť krabicu na protivníka a takto ho zraniť/zabiť.
