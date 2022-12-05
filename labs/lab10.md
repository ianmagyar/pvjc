# Cvičenie 10: Iterátor a posledné úpravy

Na dnešnom cvičení doimplementujeme poslednú dôležitú funkcionalitu do našej hry, a to inventár spolu s predmetmi, ktoré hráč dokáže zobrať so sebou a použiť ich neskôr. Pred tým než začneme s programovaním, potrebujete však urobiť dve malé úpravy vo vašom riešení:

1. Ak priamo v `SpellDirector` voláte `SpellDataProvider.GetInstance()`, zmeňte implementáciu tak, aby referenciu dostal `SpellDirector` priamo v konštruktore (využite však naďalej *singleton* návrhový vzor v `SpellDataProvider`).
2. Do `SpellDirector` pridajte referenciu na wizarda, ktorému director patrí (nastavte v konštruktore).

Alternatívne si môžete [stiahnuť a pracovať v tomto predpripravenom projekte](lab10/lab10.zip).

## 1. krok: Minor Healing Potion
Vytvorte si menný priestor `Actors.Items` (priečinok `Items`) a v ňom triedu `HealingPotion`, ktorá rozširuje `AbstractActor` a implementuje rozhrania `IItem` (z knižnice `MerlinCore`) a `IUsable` (z minulého cvičenia). Ak `IUsable` ešte nemáte implementované, zadefinujte ho nasledovne:

```
public interface IUsable
{
    void Use(IActor actor);
}
```

V triede zabezpečte, aby sa elixír dal použiť iba raz a doplnil používateľovi X (číslo si zvoľte) života. Nezabudnite, že fľaška sa tým pádom vyprádzni, takže nezabudnite zmeniť animáciu pre prípad, že sa neskôr nezmaže po použití z inventára. Ako animácie môžete použiť sprity [pre plnú fľašku](lab10/healingpotion.png) a [pre prázdnu](lab10/healingpotion_empty.png).

## 2. krok: Small Red Pouch
Vytvorte si triedu `Backpack` (nech implementuje rozhranie `IInventory` z knižnice `Merlin2d.Game.Items`). Bude sa jednať o našu vlastnú kolekciu predmetov, pričom potrebujete implementovať nasledovnú funkcionalitu:

* `Backpack` bude mať v sebe uchované veci. Jeho veľkosť je konečná, dostane ju v konštruktore. Ak by bola pridaný predmet do plného batohu, vyvolajte vlastnú výnimku `FullInventoryException` (musíte si ju zadefinovať). Pridanie do plného inventára nesmie zhodiť program.
* Predmety uchovávajte v poli (*array*).
* Vždy vieme pristupovať len k prvému (alebo poslednému, vyberte si) prvku - metóda `GetItem()`. Nový predmet je pridaný na koniec.
* Máme k dispozícii iterátor `IEnumerator<Item>`. Na implementáciu iterátora použite [yield return](https://learn.microsoft.com/en-us/dotnet/csharp/language-reference/statements/yield).
* Metódy `ShiftRight` a `ShiftLeft` posunú prvky v batohu. Nezabudnite preniesť prvok medzi koncom a začiatkom.
* Na zobrazenie hráčovho batohu použite `World.ShowInventory(IInventory)`.

Do hry priraďte hráčovi (`Player`) batoh (`Backpack`) a umožnite mu zobrať si veci a uložiť ich do batohu. Pri použití nejakej veci (`Use`) by sa hráč mal snažiť primárne použiť vec, ktorej sa dotýka, avšak ak takú vec vo svete nenájdete, tak použite dostupný predmet z batohu (nezabudnite, že v batohu sa vždy použije buď prvý alebo posledný prvok). Ak batoh je prázdny, alebo dostupný predmet sa momentálne nedá použiť, ignorujte vstup od používateľa. Takisto pridajte klávesovú skratku pre posúvanie objektov v batohu.

**Poznámka:** Pole má v sebe stále rovnaký počet prvkov, a aj keď sú `null`, `yield return` ich vráti. Zabezpečte, aby sa cyklus ukončil, keď sa nájde hodnota `null` (všetky predmety by mali byť uložené vedľa seba).

**Poznámka 2:** [Pri práci s poľami sa môžete inšpirovať metódami ponúkanými samotným jazykom C#.](https://learn.microsoft.com/en-us/dotnet/api/system.array?view=net-6.0)

## 3. This is the End
Do hry si pridajte ešte chýbajúce animácie, sprity nájdete na internete. Takisto pridajte možnosť útoku postavám `Player` a `Skeleton` (funkcionalita je na vás).

Keď už máte funkcionalitu vyriešenú, bolo by vhodné skryť konzolu pri spustení programu, urobíte tak:

* na Windowse: `Project -> Properties -> Application -> Output type: Windows Application`
* na Linuxe/Mac: v `.csproj` súbore si upravte `<OutputType>Exe</OutputType>`

Logout.
