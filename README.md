# Programovanie v jazyku C\#

**Programovanie v jazyku C#** je kurz programovania ponúknutý v zimnom semestri druhého ročníka bakalárskeho štúdia pre študijný program Inteligentné systémy. Predmet nadväzuje na kurzy [Základy algoritmizácie a programovania](https://kurzy.kpi.fei.tuke.sk/zap/), [Programovanie](https://kurzy.kpi.fei.tuke.sk/pvjc/2021/) a [Programovanie v jazyku Python](https://github.com/ianmagyar/introduction-to-python). Venuje sa paradigme objektovo orientovaného programovania a jazyku C#. Úspešný absolvent predmetu je schopný napísať zložitejšie kódy, navrhnúť modulárne softvérové riešenia a aplikovať štandardné návrhové vzory pri riešení problémov.

Informačný list predmetu je dostupný na [školskom portáli](https://maisportal.tuke.sk/portal/studijneProgramy.mais).

Prednášajúci predmetu:

* [Ing. Ján Magyar, PhD.](http://www.cloudai.sk/people-janmagyar/)

Cvičiaci predmetu:

* [Ing. Ján Magyar, PhD.](http://www.cloudai.sk/people-janmagyar/)
* [M.Eng. Dominik Vranay](https://www.cloudai.sk/people-dominikvranay/)

## Obsah
1. [Plán prednášok](#plan)
2. [Hodnotenie](#grading)
3. [Použitá literatúra](#literature)

## Plán prednášok <a name="plan"></a>
Prednášky sú v stredu o 9:10 v miestnosti ZP1 (N9-201). Prítomnosť nie je povinná.

Cvičenia sú v pondelok o 9:10, v utorok o 10:50 a v stredu o 7:30 a 15:10, všetky v miestnosti PK6_108 - PC17. Účasť na cvičeniach je povinná, študent môže mať maximálne tri neúčasti za semester. Neúčasť môžete nahradiť účasťou na inom cvičení alebo inou formou po dohode s cvičiacim.

|              Týždeň             |                         Cvičenie                        |                         Prednáška                       |
|:-------------------------------:|:-------------------------------------------------------:|:-------------------------------------------------------:|
| 1. týždeň<br>25. 9. - 1. 10.    | [Úvodné informácie](lectures/Lecture-00.pdf) <br> [Základy jazyka C#](labs/lab01.md)                    | [Úvod](lectures/Lecture-00.pdf) <br> [Základy jazyka C# a OOP](lectures/Lecture-01.pdf)                       |
| 2. týždeň<br>2. 10. - 8. 10.    | [Riešenie sudoku v jazyku C#](labs/lab02.md)            | [Konštruktory a enkapsulácia](lectures/Lecture-02.pdf) <br> ([kód z prednášky](lectures/codes/Lecture02.zip))                             |
| 3. týždeň<br>9. 10. - 15. 10.   | [Prvé kroky vo vývoji hry](labs/lab03.md)               | [Dedičnosť a polymorfizmus](lectures/Lecture-03.pdf) <br> ([kód z prednášky](lectures/codes/Lecture03.zip))                               |
| 4. týždeň<br>16. 10. - 22. 10.  | [Prepojenie aktérov a observer](labs/lab04.md)                           | [Abstraktné triedy a rozhrania](lectures/Lecture-04.pdf) <br> ([kód z prednášky](lectures/codes/Lecture04.zip)                           |
| 5. týždeň<br>23. 10. - 29. 10.  | [Štruktúra projektu a abstraktné triedy](labs/lab05.md)                  | [Polymorfizmus v OOP](lectures/Lecture-05.pdf) <br> ([kód z prednášky](lectures/codes/Lecture05.zip)                                     |
| 6. týždeň<br>30. 10. - 5. 11.   | sviatok/voľno                                           | sviatok/voľno                                           |
| 7. týždeň<br>6. 11. - 12. 11.   | Gravitácia a ďalší nepriatelia                          | [Údajové štruktúry a typy v C#](lectures/Lecture-06.pdf)      |
| 8. týždeň<br>13. 11. - 19. 11.  | Factory a mapa                                          | Generické programovanie                                 |
| 9. týždeň<br>20. 11. - 26. 11.  | Stratégie                                               | Spracovanie výnimiek                                    |
| 10. týždeň<br>27. 11. - 3. 12.  | Builder                                                 | Vzťahy medzi triedami, kreačné návrhové vzory           |
| 11. týždeň<br>4. 12. - 10. 12.  | Súbory, steny a prekážky                                | Štrukturálne návrhové vzory                             |
| 12. týždeň<br>11. 12. - 17. 12. | Inventár                                                | Behaviorálne návrhové vzory                             |
| 13. týždeň<br>18. 12. - 24. 12. | zápočtový týždeň                                        | zápočtový týždeň                                        |

## Hodnotenie <a name="grading"></a>

* [Zadanie 1](assignments/A1.pdf) (publikované v 2. týždni; deadline: 27. 10. 2023)
    * [kostra riešenia](assignments/Assignment1.zip)

* [Zadanie 2](assignments/A2.pdf) (publikované v 6. týždni; deadline: 24. 11. 2023)
    * [kostra riešenia](assignments/Assignment2.zip)

* Zadanie 3 (publikované v 10. týždni; deadline: 22. 12. 2023)

Celkové hodnotenie predmetu je 100 bodov (40 + 60 bodov); študent musí získať viac ako polovicu bodov zo zápočtu a zo skúšky.

Zápočet sa skladá z troch zadaní a 10 bodov za cvičenia: za všetky zadania môžete získať po 10 bodov. Počas semestra máte možnosť pozbierať zvyšné body za vypracovanie bonusovej úlohy na cvičení, resp. krátkymi písomkami.

Skúška má teoretickú a praktickú časť. Praktická časť je za 30 bodov, a budete v nej riešiť jednoduché úlohy na programovanie a implementáciu návrhových vzorov. Teoretická časť za 30 bodov je zameraná na aplikáciu získaných teoretických poznatkov pri riešení problémov a je open notes. Pre úspešné ukončenie predmetu musíte získať minimálne polovicu bodov z praktickej aj teoretickej časti skúšky (teda viac ako 15 bodov). V prípade neúspechu opakujete iba tú časť skúšky, v ktorej ste neuspeli.

## Použitá a odporúčaná literatúra <a name="literature"></a>
* Christian Nagel: *Professional C# and .Net*. (2021 Edition) John Wiley & Sons, Inc. (2022)
* Mark J. Price: *C# 8.0 and .NET Core 3.0: Modern Cross-Platform Development*. (4th Edition) Packt Publishing (2022)
* John Skeet: *C# in Depth*. (Fourth Edition) Manning Publications Co. (2019)
* Bertrand Meyer: *Object-Oriented Software Construction*. (Second Edition) Interactive Software Engineering Inc.
* Erich Gamma, Richard Helm, Ralph Johnson, John Vlissides: *Design Patterns: Elements of Reusable Object-Oriented Software* Addison-Wesley (1995)
* Gaurav Aroraa, Jeffrey Chilberto: *Hands-On Design Patterns with C# and .NET Core*. Packt Publishing (1995)
