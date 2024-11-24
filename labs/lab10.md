# Cvičenie 10: Kúzla a buildery 2

Minulý týždeň sme urobili prípravné práce pre pridanie podpory kúziel do našej hry. Konkrétne sme deklarovali niekoľko rozhraní a tried pre návrhový vzor *builder*, a vytvorili sme efekty, ktoré budeme ďalej využívať na vytvorenie kúziel. Na dnešnom cvičení nám ostáva dokončiť implementáciu návrhového vzora pomocou *builder* a *director* tried. Na úvod budeme pracovať so statickými hodnotami, neskôr však rozšírime implementáciu o načítanie údajov priamo zo súboru.

[Vypracované úlohy z predošlých cvičení nájdete na tomto odkaze.](lab10/Project1_after09.zip)

## 1. krok: Builders
Implementujte `SpellEffectFactory`, `ProjectileSpellBuilder`, `SelfCastSpellBuilder` a `SpellDirector` podľa návrhového vzoru *builder*.

*Builder* budeme využívať nasledovne:

1. *wizard* (nemusí byť samotný hráč) si vytvorí iba `SpellDirector`
2. *director* si vytvorí zodpovedajúcich *builder*ov
3. *director* si načíta zo súboru kúzla
4. *director* vie, ktorý *builder* sa má použiť
5. *director* povie postupne *builder*ovi všetky efekty, ktoré treba pridať do vytváraného kúzla (môže ich byť niekoľko)
6. *director* povie *builder*ovi, akú textúru nastaviť (ak sa jedná o projektil)
7. *director* spočíta cenu a povie ju *builder*ovi (selfcast *builder* si ju zníži o nejaké percento)
8. *wizard* dostane kúzlo, ktoré môže použiť (nie však používať - na ďalšie použitie si treba znovu vytvoriť novú inštanciu kúzla, inak by mohol existovať iba jeden projektil daného typu v hre).

### 1.1. `SpellEffectFactory`
Prvým krokom vytvorenie kúzla je inicializácia efektov. Namiesto toho, aby sme na inicializáciu takýchto objektov použili stále konštruktor, vytvoríme si *factory* objekt podobne, ako sme to robili v prípade *actor*ov. Do triedy `SpellEffectFactory` teda pridajt metódu `ICommand GetEffect(string effectName)`, ktorá na základe názvu efektu vygeneruje potrebný objekt. Ak ste efekty implementovali všeobecne, môžete tu využiť kódovaný názov, napríklad `DoT-3x5`, ktorý reprezentuje efekt `DamageOverTime`, ktorý pôsobí po dobu 3 sekúnd a každú sekundu uberie postave 5 životov. Cenu efektov môžete nastaviť staticky.

### 1.2. Buildery
Implementáciu pokračujeme definíciou tried `SelfCastSpellBuilder` a `ProjectileSpellBuilder`. Obe triedy nech majú prístup k `SpellEffectFactory` objektu, a takisto udržiavajte cenu kúzla a zoznam efektov v členských premenných. Môžete upraviť aj konštruktory `SelfCastSpell` a `ProjectileSpell` tak, aby zobrali parameter reprezentujúci celkovú cenu kúzla.

Ostatné metódy implementujte nasledovne:

* `ISpellBuilder AddEffect(string effectName)` - pridá jeden efekt do zoznamu efektov kúzla, na vytvorenie príslušného objektu použite *factory*.
* `ISpell CreateSpell(IWizard wizard)` - metóda reálne vytvorí a vráti objekt typu `SelfCastSpell`/`ProjectileSpell` na základe pred tým nastavených hodnôt.
* `ISpellBuilder SetSpellCost(int cost)` - nastaví celkovú cenu kúzla.
* `ISpellBuilder SetTexture(Texture2D animation)` - nastaví textúru kúzla (iba pre `ProjectileSpellBuilder`).

**Poznámka:** Opätovné použitie návratového typu `ISpellBuilder` v rozhraní `ISpellBuilder` nám umožňuje postupnú definíciu kúzla zápisom:

```
builder.AddEffect("use cycle").AddEffect("for effects").SetAnimation(animation).SetSpellCost(cost).CreateSpell()
```

Takýto zápis sa nazýva *fluent API*, a je možný aj pri `ISpell.AddEffect()`.

### 1.3. Director
Nech `SpellDirector` v sebe obsahuje dva slovníky: `Dictionary<string, int>`, kde bude cena jednotlivých efektov a `Dictionary<string, SpellInfo>`, kde budú uložené zoznamy efektov pre jednotlivé kúzla. Vďaka nim už môžete vhodným spôsobom nastaviť celkovú cenu kúzla ako aj jednotlivých efektov. Zatiaľ si tieto slovníky inicializujte ručne, neskôr hodnoty načítame zo súboru.

Do triedy pridajte konštruktor `SpellDirector(IWizard wizard, ContentManager content)`, kde prvý parameter vyjadruje čarodejníka, ktorý bude vytvárať kúzla na základe daného *director*a a druhý parameter je už známy objekt pre načítanie textúr a ďalších zdrojov (budeme používať pre `ProjectileSpell`).

Následne implementujte metódu `ISpell Build(string spellName)` s nasledovnou funkcionalitou:

1. načítajte informácie o kúzle na základe jeho mena
2. na základe typu kúzla si vyberte správny *builder* objekt
3. pridajte všetky efekty spojené s kúzlom a nezabudnite spočítať aj celkovú cenu jednotlivých efektov
4. nastavte celkovú cenu kúzla
5. vytvorte a vráťte objekt kúzla (samozrejme pomocou príslušného *builder* objektu)

### 1.4. Čarodejník
Upravte triedu `Player` tak, aby implementovala rozhranie `IWizard`, a pridajte potrebnú funkcionalitu. Triedu najprv rozšírte o členskú premennú typu `SpellDirector` a pridajte aj počítadlo many. Implementujte metódy `ChangeMana()` a `GetMana()` pre prácu s touto hodnotou, a definujte aj metódu `Cast()`:

* ak hráč nemá potrebnú manu, kúzlo sa nevytvorí.
* spracujte kúzlo, ktoré čarodejník vyčaroval (dostanete ako parameter): ak je to `SelfCastSpell`, pridajte všetky efekty do zoznamu efektov hráča. V prípade `ProjectileSpell` kúzlo musíte pridať do sveta ([ako textúru môžete použiť napríklad tento obrázok](lab10/ball.png)) a pridajte aj funkcionalitu - nech projektil poletí až kým nenarazí do actora, na ktorého začne pôsobiť (všetko v metóde `Update()` v triede `ProjectileSpell`).
* aktualizujte manu hráča.

## 1.5. Look at What I Can Do!

Pridajte niekoľko kúziel do projektu, a nastavte vstup z klávesnice tak, aby stlačením vybraných klávesov ste vedeli otestovať funkcionalitu týchto kúziel. Túto funkcionalitu implementujte priamo v metóde `Update()` v triede `Player`.
