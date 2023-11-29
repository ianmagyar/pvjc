# Cvičenie 9: Buildery

Na poslednom cvičení sme využili rôzne stratégie na nastavovanie rýchlosti postáv v našej hre. Okrem pohybu trieda `AbstractCharacter` riešila aj život a zraniteľnosť postáv, túto funkcionalitu sme však veľmi neriešili. Na dnešnom cvičení sa pozrieme na to, ako dokážeme efektívne implementovať zranenie hráčov, a pridáme do nášho projektu aj trošku kúzla.

## 1. krok: Message

Keďže budeme aktualizovať život postáv cez rôzne efekty, bolo by vhodné, aby sme ich účinky videli priamo na obrazovke. Práve preto v prvom kroku pridáme výpis aktuálnej hodnoty života hráča do nášho okna. Pre prácu s textom priamo v Monogame si musíme vytvoriť vlastné fonty, resp. ich reprezentáciu vo frameworku, teda *SpriteFont*. Spustite MGCB Editor a pridajte do projektu nový item (nie existujúci, ako sme robili doteraz s textúrami). Vo vyskakujúcom okne si vyberte možnosť *SpriteFont Description* a nový súbor pomenujte *Health*.

Vytvorí sa nový súbor typu `.spritefont`, ktorý popisuje font, ktorý sa má používať. Je to reálne XML súbor, ktorý môžete upravovať v ľubovoľnom editore, nastavte vami vybraný font (riadok 14) a veľkosť písma (riadok 20).

Následne si v triede `Game1` vytvorte členskú premennú `private SpriteFont healthFont;`, načítajte príslušný súbor v `LoadContent` metóde (podobne ako sme robili pri textúrach), a do metódy `Draw` pridajte vypísanie aktuálneho stavu života hráča cez volanie [`spriteBatch.DrawString()`](https://docs.monogame.net/api/Microsoft.Xna.Framework.Graphics.SpriteBatch.html#Microsoft_Xna_Framework_Graphics_SpriteBatch_DrawString_Microsoft_Xna_Framework_Graphics_SpriteFont_StringBuilder_Microsoft_Xna_Framework_Vector2_Microsoft_Xna_Framework_Color_).

## 2. krok: I Put a Spell on You

Nadišiel čas, aby sme do našej hry pridali trošku kúzla a niektoré naše postavy sme vyzbrojili schopnosťou bojovať so svojimi nepriateľmi. K dosiahnutiu našich cieľov potrebujeme zadefinovať nasledovné komponenty (podrobnejší popis pokračuje nižšie):

* do projektu pridajte priečinok `Spells`
* rozhrania `ISpell`, `IWizard`, `ISpellBuilder`, `ISpellDirector`
* `enum SpellType`
* triedy

    * `SpellInfo` - pomocná trieda popisujúca vlastnosti kúzla
    * `ProjectileSpell : AbstractActor, IMovable, ISpell` - kúzlo typu projektil, ktoré poletí rovno, a keď narazí na nejakú postavu, začne účinkovať
    * `SelfCastSpell : ISpell` - kúzlo, ktorým postava posilní seba
    * `ProjectileSpellBuilder : ISpellBuilder` - trieda pre vytvorenie projektil kúzla
    * `SelfCastSpellBuilder : ISpellBuilder` - trieda pre vytvorenie self-cast kúzla
    * `SpellEffectFactory` (implementujte návrhový vzor *factory method* podobne ako sme to už videli pri aktoroch) - vytvára efekty pre všetky kúzla
    * `SpellDirector : ISpellDirector` - slúži pre riadenie procesu vytvorenia kúziel

Pri vytváraní budeme využívať štruktúru [návrhového vzoru *builder*](https://en.wikipedia.org/wiki/Builder_pattern), ktorý je ďalší z kreačných návrhových vzorov a slúži na rozdelenie procesu vytvorenia zložitých objektov. Logika sa skladá z *director*a, ktorý riadi celý proces vytvárania, a má k dispozícii konkrétne *builder* objekty, ktoré sú zodpovedné za vytvorenie konkrétnych produktov (sú to časti zložitého objektu). *Director* je teda zodpovedný iba za "montáž", t.j. za nastavenie všetkých relevantných parametrov vytvoreného objektu, samotné vytvorenie inštancií je úlohou *builder*ov. Hlavná výhoda takéhoto riešenia je, že *director* môže byť definovaný bez ohľadu na to, inštancie ktorej konkrétnej triedy sa vytvoria, a takto umožňuje meniť vnútornú reprezentáciu komponentov v neskorších fázach vývoja.

### 2.1. Rozhrania a pomocné triedy

V našom riešení budeme definovať nasledovnú funkcionalitu (všeobecné rozhrania a pomocné triedy), pridajte ich do vášho projektu s uvedeným kódom:

```
public interface ISpell
{
    ISpell AddEffect(ICommand effect);
    void AddEffects(IEnumerable<ICommand> effects);
    int GetCost();
    void ApplyEffects(ICharacter target);
}

public interface IWizard : IActor
{
    void ChangeMana(int delta);
    int GetMana();
    void Cast(ISpell spell);
}

public interface ISpellBuilder
{
    ISpellBuilder AddEffect(string effectName);
    ISpellBuilder SetTexture(Texture2D texture); //unused for self-cast spells
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
    public string TextureName { get; set; }
    public int TextureWidth { get; set; }
    public int TextureHeight { get; set; }
}
```

Okrem uvedených komponentov pridajte aj enumeráciu `SpellType`, ktorá bude určovať, či sa jedná o self-cast alebo projektilové kúzlo.

Poznámky k rozhraniam:

* `IWizard`

    * mana reprezentuje silu čarodejníka na použitie kúzla (každé kúzlo má nejakú cenu a keď nemá dostatok sily, tak kúzlo nedokáže použiť)
    * `Cast` reprezentuje vytvorenie kúzla a jeho pridanie do sveta

* `ISpell`

    * reprezentuje kúzlo, kde účinky kúzla budú riešené cez efekty (všetky implementujú rozhranie `ICommand`) - napríklad kúzlo môže spôsobiť, že postave bude ubúdať život každé dve sekundy a zároveň ju kúzlo aj spomalí
    * `GetCost()` vracia cenu kúzla: neskôr bude to súčet cien jednotlivých efektov, zatiaľ ale nastavíme hodnotu staticky
    * `ApplyEffects` sa zavolá v momente, keď kúzlo začne pôsobiť na postavu `target`:

        * v prípade `SelfCastSpell` to bude okamžite (`target` je zároveň čarodejníkom, ktorý kúzlo vytvoril)
        * v prípade `ProjectileSpell` to bude v momente, keď dôjde ku kolízii medzi projektilom a postavou (`target` je postava, ktorú projektil trafil, svojho tvorcu ignoruje - na začiatku sa budú dotýkať)

* `ISpellBuilder`

    * `SetSpellCost` volajte zatiaľ len so statickou hodnotou, neskôr tam pridáte súčet jednotlivých účinkov

* `ISpellDirector`

    * deklaruje funkcionalitu *director*a, zatiaľ potrebujete jednu konkrétnu triedu, ktorá na základe názvu kúzla (parameter `spellName`) vytvorí a vráti objekt kúzla

## 2.2. It's a Kind of Magic

V ďalšom kroku začneme s implementáciou tried `ProjectileSpell` a `SelfCastSpell` (obe implementujú `ISpell`), ktoré nech majú v sebe konštruktor, v ktorom dostanú referenciu na toho, kto ich vyčaroval, a zoznam efektov ako `IEnumerable<ICommand>`. Môžete začať rozmýšľať o tom, ako by ste projektily neskôr pridali do sveta takým spôsobom, aby sa aj zobrazili na obrazovke (self-casty neuvidíme). 

Aby sme kúzla reálne mohli využívať, vytvorte si efekty: damage over time, speed change, heal, heal over time a samozrejme podľa chuti ďalšie. Všetky pridajte do priečinka `Commands`, a všetky nech implementujú rozhranie `ICommand`. Každý efekt má svoju cenu, výsledná cena kúzla bude súčet cien jednotlivých efektov. Túto cenu však nevieme priamo načítať z rozhrania `ICommand`, zatiaľ si tam nastavte statickú hodnotu. V prípade self-castu túto hodnotu vynásobte nejakým desatinným číslom menším ako 1 (čarodejník potrebuje menej many ako v prípade projektilu, keďže kúzlo aplikuje na seba).

## 2.3. Builders
Implementujte `ProjectileSpellBuilder`, `SelfCastSpellBuilder` a `SpellDirector` podľa návrhového vzoru *builder*. Nech `SpellDirector` v sebe obsahuje dva slovníky: `Dictionary<string, int>`, kde bude cena jednotlivých efektov a `Dictionary<string, SpellInfo>`, kde budú uložené zoznamy efektov pre jednotlivé kúzla. Vďaka nim už môžete vhodným spôsobom nastaviť celkovú cenu kúzla ako aj jednotlivých efektov. Zatiaľ si tieto slovníky inicializujte ručne, nabudúce si ukážeme ako si ich načítať zo súboru.

Upravte triedu `Player` tak, aby implementovala rozhranie `IWizard`, a pridajte potrebnú funkcionalitu.

*Builder* budeme využívať nasledovne:

1. *wizard* (nemusí byť samotný hráč) si vytvorí iba `SpellDirector`
2. *director* si vytvorí zodpovedajúcich *builder*ov
3. *director* si načíta zo súboru kúzla (budúce cviko, zatiaľ si ich tam pridajte ručne)
4. *director* vie, ktorý *builder* sa má použiť
5. *director* povie postupne *builder*ovi všetky efekty, ktoré treba pridať do vytváraného kúzla (môže ich byť niekoľko)
6. *director* povie *builder*ovi, akú textúru nastaviť (ak sa jedná o projektil)
7. *director* spočíta cenu a povie ju *builder*ovi (selfcast *builder* si ju zníži o nejaké percento)
8. *wizard* dostane kúzlo, ktoré môže použiť (nie však používať - na ďalšie použitie si treba znovu vytvoriť novú inštanciu kúzla, inak by mohol existovať iba jeden projektil daného typu v hre).

**Poznámka:** Opätovné použitie návratového typu `ISpellBuilder` v rozhraní `ISpellBuilder` nám umožňuje postupnú definíciu kúzla zápisom:

```
builder.AddEffect("use cycle").AddEffect("for effects").SetTexture(texture).SetSpellCost(cost).CreateSpell()
```

Takýto zápis sa nazýva *fluent API*, a je možný aj pri `ISpell.AddEffect()`.

## 2.4. Look at What I Can Do!

Pridajte niekoľko kúziel do projektu, a nastavte vstup z klávesnice tak, aby stlačením vybraných klávesov ste vedeli otestovať funkcionalitu týchto kúziel.
