# Cvičenie 12: Iterátor a posledné úpravy

Na dnešnom cvičení doimplementujeme poslednú dôležitú funkcionalitu do našej hry, a to inventár spolu s predmetmi, ktoré hráč dokáže zobrať so sebou a použiť ich neskôr.

[Vypracované úlohy z predošlých cvičení nájdete na tomto odkaze.](lab12/Project1_after11.zip)

## 1. krok: Minor Healing Potion
Vytvorte si v priečinku `Items` triedu `HealingPotion`, ktorá rozširuje `AbstractAnimatedActor` a implementuje rozhrania `IItem` a `IUsable`. Rozhrania zadefinujte nasledovne:

```
public interface IItem : IActor
{

}

public interface IUsable
{
    void Use(IActor actor);
}
```

V triede zabezpečte, aby sa elixír dal použiť iba raz a doplnil používateľovi X (číslo si zvoľte) života. Nezabudnite, že fľaška sa tým pádom vyprádzni, takže zmeňte animáciu pre prípad, že sa elixír nezmaže po použití z inventára ani zo sveta. Ako animácie môžete použiť sprity [pre plnú fľašku](lab12/healingpotion.png) a [pre prázdnu](lab12/healingpotion_empty.png). V predpripravenom projekte ich už máte pridané.

Použitie elixíru vyriešte cez kláves *E*, podobne ako sme to robili pri vypínači. Hráč teda elixír môže použiť iba v prípade, ak sa ho dotýka. Môžete upraviť aj implementáciu `PowerSwitch`, aby implementovala rozhranie `IUsable`, nie je to však povinné.

Takisto nezabudnite rozšíriť `ActorFactory` pre možnosť vytvárania objektov nového typu. Ak chcete do sveta pridať elixír, musíte tak urobiť v mape.

## 2. krok: Small Red Pouch
Do priečinka *Items* pridajte ešte rozhranie `IInventory` s nasledovným obsahom:

```
public interface IInventory : IEnumerable<IItem>
{
    IItem GetItem();

    void AddItem(IItem item);

    void RemoveItem(IItem item);

    void RemoveItem(int index);

    void ShiftLeft();

    void ShiftRight();

    int GetCapacity();
}
```

Vytvorte si triedu `Backpack` (nech implementuje rozhranie `IInventory`). Jedná sa o našu vlastnú kolekciu predmetov, pričom potrebujete implementovať nasledovnú funkcionalitu:

* `Backpack` bude mať v sebe uchované veci. Jeho veľkosť je konečná, dostane ju v konštruktore. Ak by bol pridaný predmet do plného batohu, vyvolajte vlastnú výnimku `FullInventoryException` (musíte si ju zadefinovať). Pridanie do plného inventára nesmie zhodiť program.
* Predmety uchovávajte v poli (*array*).
* Vždy vieme pristupovať len k prvému (alebo poslednému, vyberte si) prvku - metóda `GetItem()`. Nový predmet je pridaný na koniec.
* Máme k dispozícii iterátor `IEnumerator<Item>`. Na implementáciu iterátora použite [yield return](https://learn.microsoft.com/en-us/dotnet/csharp/language-reference/statements/yield).
* Metódy `ShiftRight` a `ShiftLeft` posunú prvky v batohu. Nezabudnite preniesť prvok medzi koncom a začiatkom.

**Poznámka:** Pole má v sebe stále rovnaký počet prvkov, a aj keď sú `null`, `yield return` ich vráti. Zabezpečte, aby sa cyklus ukončil, keď sa nájde hodnota `null` (všetky predmety by mali byť uložené vedľa seba).

**Poznámka 2:** [Pri práci s poľami sa môžete inšpirovať metódami ponúkanými samotným jazykom C#.](https://learn.microsoft.com/en-us/dotnet/api/system.array?view=net-6.0)

## 3. krok: Pick It Up and Put It Down
Rozšírte triedu `Player` tak, aby hráč mal svoj batoh. Do hry môžete pridať aj vykresľovanie batohu (napríklad do ľavého dolného rohu okna). Alternatívne môžete vykresliť iba práve dostupný prvok.

Pridajte možnosť hráčovi zobrať si predmety zo sveta a položiť ich naspäť cez vybrané klávesy. Takisto umožnite hráčovi presúvať si prvky v batohu. Funkcionalitu používania objektov (cez kláves *E*) upravte tak, aby sa prvotne použil predmet, ktorého sa hráč dotýka, ak taký neexistuje (alebo sa nedá použiť), nech sa použije aktuálne dostupný objekt z batohu. Do mapy sveta pridajte niekoľko elixírov, a skontrolujte funkcionalitu vášho riešenia. 
