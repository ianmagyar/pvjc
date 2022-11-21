# Cvičenie 8: Stratégie a buildery

Na minulom cvičení ste vytvorili nový projekt, do ktorého ste pridali triedu `AbstractActor` pre definíciu spoločnej funkcionality actorov. Na tomto cvičení rozšírime niektorých actorov o stratégiu pohybu a do hry pridáme aj kúzla pomocou nových návrhových vzorov.

## 1. krok: Gimme a Plan

`Player` a `Enemy` zatiaľ majú spoločné rozhranie triedy cez `AbstractActor`, majú však aj špeciálne vlastnosti ktoré ich rozlišujú od ostatných actorov. Na rozdiel od predmetov (kotlík, pec, kryštál, zdroj) sa totiž hýbu a okrem toho by v ozajstnej hre podobné postavy mali aj život. Naším cieľom v tomto kroku je pridať tieto vlastnosti cez best practice návrhové vzory.

Vytvorte si rozhranie `ISpeedStrategy`, ktoré deklaruje jednu metódu: `double GetSpeed(double speed)`. Následne si vytvorte triedy `NormalSpeedStrategy` a `ModifiedSpeedStrategy` ktoré toto rozhranie implementujú. Tieto prvky budú kostrou [návrhového vzoru *strategy*](https://en.wikipedia.org/wiki/Strategy_pattern), ktorý je jeden z behaviorálnych návrhových vzorov. Využíva sa v prípade, keď v čase písania kódu ešte nevieme, pomocou akého algoritmu bude niektorá časť funkcionality implementovaná, resp. počas behu programu sa môže meniť konkrétna implementácia, ktorú chceme používať.

![](lab08/strategy.jpg)

Základná štruktúra návrhového vzoru spočíva v spoločnom rozhraní (v našom prípade `ISpeedStrategy`) rôznych tried, ktoré implementujú takzvané stratégie (`NormalSpeedStrategy` a `ModifiedSpeedStrategy`). Následne triedu, ktorá bude využívať naše stratégie (postavy, implementujeme neskôr), rozšírime o členskú premennú stratégie (typu `ISpeedStrategy`) a takto budeme môcť využívať metódy definované v tomto spoločnom rozhraní (`GetSpeed()`).

K implementácii celého návrhového vzoru už nám chýba len úprava postáv. Je však jasné, že všetky postavy budú mať niečo spoločné v tom, ako riešia pohyb, a práve preto tútu funkcionalitu vieme dať do spoločnej nadtriedy. Vytvorte si teda rozhranie `ICharacter`, ktoré nech rozširuje `IMovable`. Vytvorte si aj abstraktnú triedu `AbstractCharacter`, ktorá bude implementovať `ICharacter`, `IMovable` (priamo alebo nepriamo) a samozrejme bude rozširovať `AbstractActor`.

Nech rozhranie `ICharacter` deklaruje nasledujúce metódy:

```
void ChangeHealth(int delta);
int GetHealth();
void Die();
void AddEffect(ICommand effect);
void RemoveEffect(ICommand effect);
```

kde *effect* reprezentuje veci, ako napríklad periodické dopĺňanie života / many, DoT efekty, spomalenie, zrýchlenie a pod. Efekty budú uložené v zozname a budú vykonané počas `Update()`, konkrétne typy efektov vytvoríte pri implementácii kúziel.

Metódy `ChangeHealth` a `GetHealth` pridávajú život postavy, konkrétnu funkcionalitu riešite už podľa vašich predstáv. V metóde `Die()` zatiaľ len označte postavu na vymazanie zo sveta (funkcionalita v `AbstractActor`), neskôr to opravíme.

Do existujúceho rozhrania `IMovable` pridajte nasledujúce metódy:

```
void SetSpeedStrategy(ISpeedStrategy strategy);
double GetSpeed(double speed);
```

Triedu `Move` upravte tak, aby pracovala s desatinnými číslami. Rýchlosť sa už nebude udávať v konštruktore, ale bude pýtaný od `IMovable` objektu. Keďže pozícia actora musí byť celé číslo, zabezpečte, aby sa rozdiel akumuloval a následne pripočítal keď dosiahne 1.

Obidve triedy `SpeedStrategy` implementujte tak, aby používali návrhový vzor *strategy* a tiež modifikujte `AbstractCharacter` tak, aby využíval spomínané triedy na získanie aktuálnej rýchlosti hráča: v `GetSpeed` sa odovzdáva pôvodná rýchlosť postavy, a stratégia vráti aktuálnu rýchlosť (napríklad ak postava je zranená, musí ísť pomalšie). Konkrétne fungovanie tried `NormalSpeedStrategy` a `ModifiedSpeedStrategy` je na vás.

Upravte triedy `Player` a `Enemy` tak, aby rozširovali `AbstractCharacter`. `Enemy` premenujte na `Skeleton`. Pre jednoduchosť práce s animáciami si pridajte `enum ActorOrientation`, ktorý bude obsahovať dve hodnoty: `FacingLeft` a `FacingRight`. Využite ho na určenie orientácie actora vo svete.

## 2. krok: I Put a Spell on You

Nadišiel čas, aby sme do našej hry pridali trošku kúzla a niektoré naše postavy sme vyzbrojili schopnosťou bojovať so svojimi nepriateľmi. K dosiahnutiu našich cieľov potrebujeme zadefinovať nasledovné komponenty (podrobnejší popis pokračuje nižšie):

* do projektu pridajte priečinok `Spells`
* rozhrania `IWizard`, `ISpell`, `ISpellBuilder`, `ISpellDirector`
* `enum SpellType`
* triedy

	* `SpellInfo` - pomocná trieda popisujúca vlastnosti kúzla
	* `ProjectileSpell : AbstractActor, IMovable, ISpell` - kúzlo typu projektil, ktoré poletí rovno a keď narazí na nejakú postavu, začne účinkovať
	* `SelfCastSpell : ISpell` - kúzlo, ktorým postava posilní seba
	* `ProjectileSpellBuilder : ISpellBuilder` - trieda pre vytvorenie projektil kúzla
	* `SelfCastSpellBuilder : ISpellBuilder` - trieda pre vytvorenie self-cast kúzla
	* `SpellEffectFactory` (implementujte návrhový vzor *factory method* podobne ako sme to videli na [minulom cvičení](https://github.com/ianmagyar/pvjc/blob/master/labs/lab07.md)) - vytvára efekty pre všetky kúzla
	* `SpellDirector : ISpellDirector` - slúži pre riadenie procesu vytvorenia kúziel

Pri vytváraní budeme využívať štruktúru [návrhového vzoru *builder*](https://en.wikipedia.org/wiki/Builder_pattern), ktorý je ďalší z kreačných návrhových vzorov a slúži na rozdelenie procesu vytvorenia zložitých objektov. Logika sa skladá z *director*a, ktorý riadi celý proces vytvárania, a má k dispozícii konkrétne *builder* objekty, ktoré sú zodpovedné za vytvorenie konkrétnych produktov (sú to časti zložitého objektu). *Director* je teda zodpovedný iba za "montáž", t.j. za nastavenie všetkých relevantných parametrov vytvoreného objektu, samotné vytvorenie inštancií je úlohou *builder*ov. Hlavná výhoda takéhoto riešenia je to, že *director* môže byť definovaný bez ohľadu na to, inštancie ktorej konkrétnej triedy sa vytvoria, a takto umožňuje meniť vnútornú reprezentáciu komponentov v neskorších fázach vývoja.

V našom riešení budeme definovať nasledovnú funkcionalitu (všeobecné rozhrania a pomocné triedy):

```
public interface IWizard : IActor
{
    void ChangeMana(int delta);
    int GetMana();
    void Cast(ISpell spell);
}

public interface ISpell
{
    ISpell AddEffect(ICommand effect);
    void AddEffects(IEnumerable<ICommand> effects);
    int GetCost();
    void ApplyEffects(ICharacter target);
}

public interface ISpellBuilder
{
    ISpellBuilder AddEffect(string effectName);
    ISpellBuilder SetAnimation(Animation animation); //unused for self-cast spells
    ISpellBuilder SetSpellCost(int cost);
    ISpell CreateSpell(IWizard wizard); // reference to the wizard doing the spell
}

public interface ISpellDirector
{
    ISpell Build(string spellName);
}

public class SpellInfo
{
    public string Name { get; set; }
    public SpellType SpellType { get; set; }
    public IEnumerable<string> EffectNames { get; set; }
    public string AnimationPath { get; set; }
    public int AnimationWidth { get; set; }
    public int AnimationHeight { get; set; }
}
```

Poznámky k rozhraniam:

* `IWizard`

	* mana reprezentuje silu čarodejníka na použitie kúzla (každé kúzlo má nejakú cenu a keď nemá dostatok sily, tak kúzlo nedokáže použiť)
	* `Cast` reprezentuje vytvorenie kúzla a jeho pridanie do sveta

* `ISpell`

	* reprezentuje kúzlo, kde účinky kúzla budú riešené cez efekty (všetky implementujú rozhranie `ICommand`) - napríklad kúzlo môže spôsobiť, že postave bude ubúdať život každé dve sekundy a zároveň ju kúzlo aj spomalí
	* `GetCost()` vracia cenu kúzla: neskôr bude to súčet cien jednotlivých efektov, zatiaľ ale nastavíme statickú hodnotu
	* `ApplyEffects` sa zavolá v momente, keď kúzlo začne pôsobiť na postavu `target`:

		* v prípade `SelfCastSpell` to bude okamžite (`target` je zároveň čarodejníkom, ktorý kúzlo vytvoril)
		* v prípade `ProjectileSpell` to bude v momente keď dôjde ku kolízii medzi projektilom a postavou (`target` je postava, ktorú projektil trafil, svojho tvorcu ignoruje - na začiatku sa budú dotýkať)

* `ISpellBuilder`

	* `SetSpellCost` volajte zatiaľ len so statickou hodnotou, neskôr tam pridáte súčet jednotlivých účinkov

* `ISpellDirector`

	* deklaruje funkcionalitu *director*a, zatiaľ potrebujete jednu konkrétnu triedu, ktorá na základe názvu kúzla (parameter `spellName`) vytvorí a vráti objekt kúzla

Poznámky k ďalším komponentom:

* Nech `SpellType` určuje, či sa jedná o self-cast alebo projektil.
* Nech `ProjectileSpell` a `SelfCastSpell` majú v sebe konštruktor, v ktorom dostanú referenciu na toho, kto ich vyčaroval, a zoznam efektov ako `IEnumerable<ICommand>`.
* Vytvorte si efekty (triedy, všetky implementujú `ICommand`): damage over time, speed change, heal, heal over time a samozrejme podľa chuti ďalšie. Všetky pridajte do priečinka `Commands`.
* Každý efekt má svoju cenu, výsledná cena kúzla bude súčet cien jednotlivých efektov (pre self-cast nech je táto cena upravená, napr. 70% celkovej ceny).
* Implementujte `ProjectileSpellBuilder`, `SelfCastSpellBuilder` a `SpellDirector` ako návrhový vzor *builder* a nech `SpellDirector` v sebe obsahuje dva slovníky: `Dictionary<string, int>` kde bude cena jednotlivých efektov a `Dictionary<string, SpellInfo>` kde budú uložené zoznamy efektov pre jednotlivé kúzla. Zatiaľ si tieto slovníky inicializujte ručne, nabudúce si ukážeme ako si ich načítať zo súboru.

*Builder* budeme využívať nasledovne:

* *wizard* (nemusí byť samotný hráč) si vytvorí iba `SpellDirector`
* *director* si vytvorí zodpovedajúcich *builder*ov
* *director* si načíta zo súboru kúzla (budúce cviko, zatiaľ si ich tam pridajte ručne)
* *director* vie, ktorý *builder* sa má použiť
* *director* povie postupne *builder*ovi všetky efekty, ktoré treba pridať do vytváraného kúzla (môže ich byť niekoľko)
* *director* povie *builder*ovi, akú animáciu nastaviť (ak sa jedná o projektil)
* *director* spočíta cenu a povie ju *builder*ovi (selfcast *builder* si ju zníži o nejaké percento)
* *wizard* dostane kúzlo, ktoré môže použiť (nie však používať - na ďalšie použitie si treba znovu vytvoriť novú inštanciu kúzla, inak by mohol existovať iba jeden projektil daného typu v hre).

**Poznámka:** Opätovné použitie návratového typu `ISpellBuilder` v rozhraní `ISpellBuilder` nám umožňuje postupnú definíciu kúzla zápisom:

```
builder.AddEffect("use cycle").AddEffect("for effects").SetAnimation(animation).SetSpellCost(cost).CreateSpell()
```

Takýto zápis sa nazýva *fluent API*, a je možný aj pri `ISpell.AddEffect()`.
