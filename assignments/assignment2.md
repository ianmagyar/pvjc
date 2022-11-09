# Zadanie 2

V rámci druhého zadania vytvoríte vlastnú hru pomocou knižnice `MerlinCore`. Funkcionalitu a dej hry si musíte navrhnúť sami.

## Obsah zadania
Riešenie zadania musí spĺňať nasledujúce podmienky:

* vypracovaný obsah cvičení 7-10
* používať aktuálnu verziu knižnice `MerlinCore`
* dodržať štruktúru projektu, ktorá je uvedená nižšie
* **pridať** minimálne
	* 5 nových actorov (okrem tých implementovaných na cvičeniach 3-6)
	* 3 návrhové vzory
	* 3 itemy
	* 2 *SpellEffect*y
* vlastná tvorivá činnosť (hra musí mať dej a zmysluplné úlohy pre hráča)
* dohrateľné kolo/-á
* dodržanie princípu DRY (*Don't Repeat Yourself*)
* dodržanie zapúzdrenia
* dodržanie [štýlovej smernice](https://github.com/ktaranov/naming-convention/blob/master/C%23%20Coding%20Standards%20and%20Naming%20Conventions.md) (okrem bodu 10, `var` nemusíte používať)
* out-of-the-box riešenie, t.j. projekt musí byť spustiteľný po rozbalení zip súboru.

[Knižnica `MerlinCore` s dokumentáciou je dostupná tu.](https://github.com/Kellei2983/MerlinCore)

## Odovzdanie a hodnotenie
Riešenie potrebujete odovzdať do konca 12. týždňa (18. 12. 2022) ako zazipovaný projekt, ak to cvičiaci neurčí inak. Súčasťou odovzdania je aj obhajoba riešenia, kde svojmu cvičiacemu predstavíte Váš projekt a funkcionalitu. Obhajoba slúži aj ako kontrola originality, t.j. návrhové a implementačné rozhodnotia musíte byť schopní odôvodniť. Obhajoby budú prebiehať od 10. týždňa, o termínoch budete informovaní cvičiacimi.

Za zadanie môžete získať maximálne 20 bodov:

* 5 bodov za úlohy z cvičení
* 15 bodov za vlastnú tvorivú činnosť:
	* pridanie novej funkcionality
	* mapa
	* originálnosť nápadu a celkový dojem z hry.

Ako aj pri prvom zadaní, za nedodržanie zásad uvedených vyššie dostanete penalizáciu v percentách, pričom maximálna penalizácia je 25%. Penalizácia sa udeľuje za nedodržanie zapúzdrenia, hrubé porušenie štýlovej smernice, neskompilovateľnosť kódu a pod. Za nevhodné designové rozhodnutia penalizovaní **nebudete**.

Za mimoriadne kvalitne vypracované riešenie môže byť študent odmenený maximálne 5 bodmi do zápočtu (udelenie bodov je v kompetencii cvičiaceho).

Ak zadanie nestihnete obhájiť do konca semestra (23. 12.), máte nárok na opravný zápočet v prvých dvoch týždňoch skúškového obdobia. Ak ste zadanie odovzdali do deadlinu, a nestihli ste iba obhajobu, stále môžete získať body riadne. Avšak ak zadanie ste ani neodovzdali, môžete získať iba minimálny zápočet (t.j. 21 bodov) bez ohľadu na priebežné hodnotenie.

## Štruktúra projektu
```
<SolutionDirectory>/
├─ <ProjectDirectory>/
│  ├─ Actors/
│  ├─ Commands/
│  ├─ Items/
│  ├─ resources/
│  ├─ <ProjectFile>.csproj
│  └─ Program.cs
├─ .gitignore
├─ <SolutionFile>.sln
└─ README
```

## Používanie rozhraní
Ak rozhranie deklaruje konkrétne metódy, použite ich pri riešení problému. Ak niekde chýba medzikrok a potrebujete pridať ďalšie metódy, môžete tak urobiť - zohľadnite to aj v rozhraniach.

Ak má `Class1` implementovať `Interface1`, neznamená to, že ho musí implementovať priamo:

```
public interface Interface1
{

}

public class Class1 : Interface1 //OK
{

}

public interface Interface2 : Interface1
{

}

public class Class1 : Interface2 //OK as well
{

}
```

Všade, kde to nie je nutné, používajte prístup `private`, platí to aj pre členské premenné - k použitiu properties pristupujte rovnako ako k použitiu `public get/set` - iba keď je to odôvodnené, inak použite `private` členskú premennú.
