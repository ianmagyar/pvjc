# Cvičenie 1: Nastavenie programátorského rozhrania, základy jazyka C\#

V rámci prvého cvičenia si pripravíte programátorské rozhranie a prezrieme si základy syntaxe jazyka C#. Síce väčšinu týchto konštruktov už poznáte zo skúseností s jazykom C, pozrieme si aj niekoľko riešení špecifických pre C#. Na konci cvičenia by ste mali mať pripravené prostredie na programovanie s nainštalovanými nástrojmi, a napíšete prvé jednoduché programy v C#.

## Programátorské prostredie
Prvým krokom pri programovaní musí byť vždy príprava programátorského prostredia. Ako v prípade ostatných programovacích jazykov, aj pre C# existuje niekoľko možných prostredí a ďalších spôsobov, ako vyvíjať aplikácie. Dve základné možnosti sú **Visual Studio** - zamerané (aj) na C#, a **Visual Studio Code** - s podporou niekoľko jazykov vrátane C#. Visual Studio Code je voľne dostupné riešenie, kým Visual Studio ponúka community edition, ktoré tiež môžete používať. Ďalšia výhoda Visual Studio Code je dostupnosť pre OS iné ako Windows.

Okrem samotného programátorského prostredia budete potrebovať mať nainštalované .NET. Nižšie uvádzame dva spôsoby prípravy programátorského prostredia, môžete si ale zvoliť inú možnosť, v tomto prípade postupujte podľa návodov dostupných na internete. V prípade problémov sa obráťte na vyučujúcich predmetu.

Vzhľadom na obsah ďalších cvičení a nástrojov, ktoré budeme používať (najmä softvérový rámec *MonoGame*), je mierna preferencia pre Visual Studio, avšak prostredie si viete nastaviť aj v iných IDE.

### Visual Studio Code

1. Stiahnite si a nainštalujte aktuálnu stable alebo insiders verziu [Visual Studio Code](https://code.visualstudio.com).
2. Stiahnite si a nainštalujte [.NET SDK](https://dotnet.microsoft.com/en-us/download) - knižnica *MonoGame* podporuje už aj verziu .NET 8.
3. Spustite aplikáciu Visual Studio Code.
4. Kliknite na ikonu Extensions (alebo kliknite na View -> Extensions).
5. Vyberte si a nainštalujte rozšírenie pre C# (*C# extension*, *C# Dev Kit extension*).
6. Nainštalujú sa všetky potrebné knižnice.

### Visual Studio 2022 - Community Edition
1. Stiahnite si [Visual Studio Installer](https://visualstudio.microsoft.com/vs/).
2. Po spustení installera si vyberte pomocné nástroje pre vývoj aplikácií. Minimálne potrebujete *.NET Desktop Development*, ale odporúčame nainštalovať aj *ASP.NET and Web Development* a *.NET Multi-platform App UI Development*. Z pravého zoznamu nechajte vybraté opcionálne ponuky (*Development tools for .NET*, *.NET Framework 4.8 development tools*, *Entity Framework 6 tools*, *.NET profiling tools*, *IntelliCode*, *Just-In-Time debugger*, *Live Share*, *ML.NET Model Builder*, *Blend for Visual Studio*).
3. Odštartujte inštalačný proces, všetko sa pre Vás pripraví automaticky.

Po úspešnej inštalácii môžete vytvoriť prvú jednoduchú konzolovú aplikáciu **Hello, World!** pre kontrolu nastavenia programátorského prostredia. V dokumentácii týchto prostredí sú dostupné návody pre [Visual Studio Code](https://learn.microsoft.com/en-us/dotnet/core/tutorials/with-visual-studio-code) ako aj pre [Visual Studio](https://learn.microsoft.com/en-us/visualstudio/get-started/csharp/tutorial-console) (stačí urobiť prvú časť).

### Ďalšie nástroje
Počas semestra budeme pracovať so softvérovým rámcom [MonoGame](https://www.monogame.net), ktorý slúži na vývoj jednoduchých, ale aj profesionálnych počítačových hier. Síce *MonoGame* budeme používať až od 3. cvičenia, môžete si ho nainštalovať už teraz, aby ste boli pripravení na celý semester. Návody pre jednotlivé operačné systémy a IDE nájdete na [web stránke MonoGame](https://docs.monogame.net/articles/getting_started/index.html).

Následne si môžete overiť inštaláciu vytvorením prázdneho *MonoGame* projektu podľa návodov pre [Visual Studio](https://docs.monogame.net/articles/getting_started/2_choosing_your_ide_visual_studio.html) alebo [Visual Studio Code](https://docs.monogame.net/articles/getting_started/2_choosing_your_ide_vscode.html?tabs=windows).

## Základná syntax jazyka C\#
V ďalšej časti cvičenia si prejdeme základnú syntax jazyka C#, ktorá vám už bude známa z programovania v jazyku C. Špecifickejšie konštrukty pre podporu objektovo-orientovaného programovania budeme preberať postupne počas semestra. Ako už určite viete, pre napísanie jednoduchých programov potrebujete poznať päť základných konštruktov:

1. premenné;
2. operátory;
3. vetvenia;
4. iterácie;
5. funkcie.

#### 1. Premenné
Keďže C# je (zväčša) staticky typovaný jayk, pri definícii premenných potrebujete určiť aj ich typ. V prípade primitívnych typov to môžeme urobiť napríklad takto:

```
int number = 5;
```

Pri vytváraní objektov musíme zavolať konštruktor, napríklad:

```
Student st = new Student("Janko Hraško");
```

Alternatívne, v C# existuje aj možnosť použiť kľúčové slovo `var`, pomocou ktorého sa typ premennej inferuje na základe pravej strany pridelenia (typ ale stále bude statický). [O `var` a jeho použití sa viac dočítate v dokumentácii jazyka C#.](https://learn.microsoft.com/en-us/dotnet/csharp/language-reference/keywords/var) Na cvičeniach a v zadaniach túto formu zatiaľ nebudeme používať.

```
var number = 5;
var st = new Student("Janko Hraško");
```

#### 2. Operátory
Operátory zvyčajne delíme na aritmetické, priraďovacie, porovnávacie a logické.

Základné aritmetické operátory sú:

* `+` - sčítanie;
* `-` - odčítanie;
* `*` - násobenie;
* `/` - delenie (celočíselné pre `int`, desatinné pre desatinné čísla);
* `%` - zvyšok po celočíselnom delení.

Priraďovací operátor je `=`, respektíve skrátené formy `+=`, `-=` a pod.

Pre porovnávanie slúžia operátory:

* `==` - rovná sa;
* `!=` - nerovná sa;
* `>` - väčšie;
* `<` - menšie;
* `>=` - väčšie alebo rovné;
* `<=` - menšie alebo rovné.

Základné logické operátory sú:

* `&&` - logický AND (a zároveň);
* `||` - logický OR (alebo)
* `!` - unárny NOT (negácia).

Ďalšie operátory nájdete [v dokumentácii jazyka C#](https://learn.microsoft.com/en-us/dotnet/csharp/language-reference/operators/).

#### 3. Vetvenia
Ak chod programu závisí od vyhodnotenia niekoľkých podmienok, funkcionalitu musíme vyriešiť cez vetvenia. V C# máme k dispozícii dve základné možnosti: konštrukt `if-else` a `switch`.

```
if (x > 10) {
	// do something
}
else if (x > 5) {
	// do something else
}
else {
	// do something else entirely
}
```

```
switch (x)
{
	case 1:
		// do something
		break;
	case 2:
	case 3:
		// do something for 2 and 3
		break;
	default:
		// do this for other values
		break;
}
```

Pri používaní vetvenia `if` nie je nutné uviesť zátvorky `{ }` v prípade, že vetva obsahuje iba jeden príkaz (zvyčajne sa ale uvádzajú pre lepšiu čitateľnosť). V prípade použitia `switch` je potrebné uviesť príkaz `break` na konci každej vetvy pre ukončenie vykonávania vetvenia. [Viac informácií o `switch` nájdete v dokumentácii jazyka C#.](https://learn.microsoft.com/en-us/dotnet/csharp/language-reference/statements/selection-statements#the-switch-statement)

#### 4. Iterácie
V programovaní poznáme dva základné typy iterácií (cyklov):

* aritmetické (ak počet iterácií je daný hodnotou premennej alebo dĺžkou sekvencie),
* logické (ak iteráciu vykonávame až do naplnenia nejakej podmienky).

C# ponúka dve formy pre oba typy. Jednoduchý `for` slúži na vykonávanie cyklu v určitom počte iterácií, kým `foreach` slúži na prechádzanie zoznamom alebo iným sekvenčným typom. Logické iterácie sú `while` a `do ... while`, kde `while` negarantuje vykonávanie tela cyklu a pri použití `do ... while` sa cyklus vykoná aspoň raz.

Syntax týchto cyklov je znázornená nižšie:

```
for (int i = 0; i < 10; i++)
{
	// do something;
}
```

```
foreach (int num in numbers)
{
	// do something
}
```

```
while (condition)
{
	// do something
}
```

```
do
{
	// something
} while (condition);
```

#### 5. Funkcie
Funkcie v C# sa často definujú v rámci triedy, viete ich ale vytvárať aj mimo nich. V triedach okrem známych členov deklarácie funkcií v C# potrebujeme zadefinovať aj modifikátor prístupu (podrobnejšie na ďalších cvičeniach a prednáškach). Funkcie sa definujú nasledovným spôsobom:

```
// visible everywhere
public void Foo(int number)
{
	// define your method here
}
```

V deklarácii funkcie teda najprv máme modifikátor prístupu, potom nasleduje deklarácia návratového typu (`void` naznačuje funkciu bez návratovej hodnoty), názov funkcie (v C# zvyčajne veľkým písmenom) a v zátvorkách môžu byť uvedené parametre aj s typom. Telo funkcie je definované už známym spôsobom v bloku ohraničenom zátvorkami `{ }`. Návratovú hodnotu vieme zadefinovať pomocou kľúčového slova `return`.

**Poznámka:** Ak programujete bez definovania triedy (napríklad v doplňujúcej úlohe), pri definovaní funkcií nie je potrebné uviesť modifikátor prístupu. V tomto prípade použite iba správnu formu definície návratového typu.

## Doplňujúca úloha
Pre získanie jedného bodu do zápočtu vypracujte **minimálne 3 z uvedených 10 úloh**. Vypracovené riešenie (iba jeden súbor `.cs`) ukážte cvičiacemu na cvičení alebo pošlite cez Teams do ďalšieho cvičenia. Ak zatiaľ nemáte nastavené prostredie, môžete použiť online kompilátory. Pre každú úlohu zadefinujte osobitnú funkciu, pri riešení zatiaľ nepotrebujete vytvárať triedy.

1. Zadefinujte funkciu `IsPrime(int number)` s jedným celočíselným parametrom, ktorá vráti, či je číslo prvočíslo (`true`) alebo nie (`false`).
2. Zadefinujte funkciu `Fibonacci(int i)` s jedným celočíselným parametrom, ktorá vráti *i*-tý prvok Fibonacciho postupnosti. Fibonacciho postupnosť sa začína číslami 1, 1, a ďalšie prvky sú vypočítané ako súčet predošlých dvoch prvkov (1, 1, 2, 3, 5, 8, ...).
3. Zadefinujte funkciu `IsTriangle(int a, int b, int c)` s tromi celočíselnými parametrami, ktorá vráti `true` ak zadané tri číselné hodnoty môžu reprezentovať dĺžku strán trojuholníka. V opačnom prípade funkcia vráti `false`.
4. Zadefinujte funkciu `IsRightTriangle(int a, int b, int c)` s tromi celočíselnými parametrami, ktorá vráti `true` ak zadané tri číselné hodnoty môžu reprezentovať dĺžku strán pravouhlého trojuholníka. V opačnom prípade funkcia vráti `false`.
5. Zadefinujte funkciu `WriteMultiples(int number, int limit)` s dvomi celočíselnými parametrami, ktorá vypíše všetky násobky čísla `number` od 1 po `limit` (vrátane). Funkcia nemá návratovú hodnotu.
6. Zadefinujte funkciu `WriteDivisors(int number)` s jedným celočíselným parametrom, ktorá vypíše všetky delitele čísla `number`. funkcia nemá návratovú hodnotu.
7. Zadefinujte funkciu `DigitSum(int number)` s jedným celočíselným parametrom, ktorá vráti súčet číslic čísla `number`.
8. Zadefinujte funkciu `DigitDiff(int number)` s jedným celočíselným parametrom, ktorá vráti rozdiel najmenšej a najväčšej číslice čísla `number`.
9. Zadefinujte funkciu `Factorial(int number)` s jedným celočíselným parametrom, ktorá vráti faktoriál čísla `number` (súčin čísel od 1 po `number` vrátane).
10. Zadefinujte funkciu `WriteFibonacci(int n)` s jedným celočíselným parametrom, ktorá vypíše prvých *n* Fibonacciho čísel.
