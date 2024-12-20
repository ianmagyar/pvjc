# Programovanie v jazyku C\#

**Programovanie v jazyku C#** je kurz programovania ponúknutý v zimnom semestri druhého ročníka bakalárskeho štúdia pre študijný program Inteligentné systémy. Predmet nadväzuje na kurzy [Základy algoritmizácie a programovania](https://kurzy.kpi.fei.tuke.sk/zap/), [Programovanie](https://kurzy.kpi.fei.tuke.sk/pvjc/2021/) a [Programovanie v jazyku Python](https://github.com/ianmagyar/introduction-to-python). Venuje sa paradigme objektovo orientovaného programovania a jazyku C#. Úspešný absolvent predmetu je schopný napísať zložitejšie kódy, navrhnúť modulárne softvérové riešenia a aplikovať štandardné návrhové vzory pri riešení problémov.

Informačný list predmetu je dostupný na [školskom portáli](https://maisportal.tuke.sk/portal/studijneProgramy.mais).

Prednášajúci predmetu:

* [Ing. Ján Magyar, PhD.](https://cit.fei.tuke.sk/people-janmagyar/)

Cvičiaci predmetu:

* Ing. Ľubomír Urblík
* Bc. Jakub Suďa

## Obsah
1. [Plán prednášok](#plan)
2. [Hodnotenie](#grading)
3. [Použitá literatúra](#literature)

## Plán prednášok <a name="plan"></a>
Prednášky sú v stredu o 9:10 v miestnosti ZP1 (N9-201). Cvičenia sú v pondelok o 9:10, v utorok o 10:50 a v stredu o 7:30 a 15:10, všetky v miestnosti PK6_108 - PC17. Účasť na cvičeniach je povinná, študent môže mať maximálne tri neúčasti za semester. Neúčasť môžete nahradiť účasťou na inom cvičení alebo inou formou po dohode s cvičiacim.

|              Týždeň             |                         Cvičenie                        |                         Prednáška                       |
|:-------------------------------:|:-------------------------------------------------------:|:-------------------------------------------------------:|
| 1. týždeň<br>23. 9. - 29. 9.    | [Úvodné informácie](lectures/Lecture-00.pdf) <br> [Základy jazyka C#](labs/lab01.md)                | [Základy jazyka C# a OOP](lectures/Lecture-01.pdf)                                 |
| 2. týždeň<br>30. 9. - 6. 10.    | [Riešenie sudoku v jazyku C#](labs/lab02.md)            | [Konštruktory a enkapsulácia](lectures/Lecture-02.pdf)<br>([kód z prednášky](lectures/codes/Lecture02.zip))                             |
| 3. týždeň<br>7. 10. - 13. 10.   | [Prvé kroky vo vývoji hry](labs/lab03.md)               | [Dedičnosť a polymorfizmus](lectures/Lecture-03.pdf)<br>([kód z prednášky](lectures/codes/Lecture03.zip))                               |
| 4. týždeň<br>14. 10. - 20. 10.  | [Prepojenie aktorov a observer](labs/lab04.md)          | [Abstraktné triedy a rozhrania](lectures/Lecture-04.pdf)<br>([kód z prednášky](lectures/codes/Lecture04.zip))                           |
| 5. týždeň<br>21. 10. - 27. 10.  | [Štruktúra projektu a abstraktné triedy](labs/lab05.md) | [Polymorfizmus v OOP](lectures/Lecture-05.pdf)<br>([kód z prednášky](lectures/codes/Lecture05.zip))                                     |
| 6. týždeň<br>28. 10. - 3. 11.   | [Commandy a refaktorizácia](labs/lab06.md)              | [Údajové štruktúry v C#](lectures/Lecture-06.pdf)                                  |
| 7. týždeň<br>4. 11. - 10. 11.   | [Svet a jeho gravitácia](labs/lab07.md)                 | [Generické programovanie](lectures/Lecture-07.pdf)<br>([kód z prednášky](lectures/codes/Lecture07.zip))                                 |
| 8. týždeň<br>11. 11. - 17. 11.  | [Stratégie](labs/lab08.md)                              | [Spracovanie výnimiek](lectures/Lecture-08.pdf)<br>([kód z prednášky](lectures/codes/Lecture08.zip))                                    |
| 9. týždeň<br>18. 11. - 24. 11.  | [Kúzla a buildery pt. I](labs/lab09.md)                 | Tipy a triky v C#                                       |
| 10. týždeň<br>25. 11. - 1. 12.  | [Kúzla a buildery pt. II](labs/lab10.md)                | [UML diagramy, kreačné návrhové vzory](lectures/Lecture-10.pdf)                    |
| 11. týždeň<br>2. 12. - 8. 12.   | [Súbory, steny a prekážky](labs/lab11.md)               | [Štrukturálne návrhové vzory](lectures/Lecture-11.pdf)                             |
| 12. týždeň<br>9. 12. - 15. 12.  | [Iterátor a posledné úpravy](labs/lab12.md)             | [Behaviorálne návrhové vzory](lectures/Lecture-12.pdf)                             |
| 13. týždeň<br>16. 12. - 22. 12. | zápočtový týždeň                                        | Vlákna a synchronizácia                                 |

## Hodnotenie <a name="grading"></a>

* [Zadanie 1](assignments/Zadanie1.pdf) (publikované v 1. týždni; deadline: 27. 10. 2024)
    * [kostra riešenia](assignments/Assignment1.zip)

* [Zadanie 2](assignments/Zadanie2.pdf) (publikované v 6. týždni; deadline: 24. 11. 2024)
    * [kostra riešenia](assignments/Assignment2.zip)

* [Zadanie 3](assignments/Zadanie3.pdf) (publikované v 10. týždni; deadline: 22. 12. 2024)
    * [kostra riešenia](assignments/Assignment3.zip)
    * [popis štruktúry vstupného súboru](assignments/JSON.pdf)

Celkové hodnotenie predmetu je 100 bodov (40 + 60 bodov); študent musí získať viac ako polovicu bodov zo zápočtu a zo skúšky.

Zápočet sa skladá z troch zadaní a 10 bodov za cvičenia: za všetky zadania môžete získať po 10 bodov. Počas semestra máte možnosť pozbierať zvyšné body za vypracovanie bonusovej úlohy na cvičení, resp. krátkymi písomkami.

Skúška má teoretickú a praktickú časť. Praktická časť je za 30 bodov, a budete v nej riešiť jednoduché úlohy na programovanie a implementáciu návrhových vzorov. Teoretická časť za 30 bodov je zameraná na aplikáciu získaných teoretických poznatkov pri riešení problémov a je open notes. Pre úspešné ukončenie predmetu musíte získať minimálne polovicu bodov z praktickej aj teoretickej časti skúšky (teda viac ako 15 bodov). V prípade neúspechu opakujete iba tú časť skúšky, v ktorej ste neuspeli.

[Otázky na skúšku nájdete tu.](exam_questions.md)

## Použitá a odporúčaná literatúra <a name="literature"></a>
* Christian Nagel: *Professional C# and .Net*. (2021 Edition) John Wiley & Sons, Inc. (2022)
* Mark J. Price: *C# 8.0 and .NET Core 3.0: Modern Cross-Platform Development*. (4th Edition) Packt Publishing (2022)
* John Skeet: *C# in Depth*. (Fourth Edition) Manning Publications Co. (2019)
* Bertrand Meyer: *Object-Oriented Software Construction*. (Second Edition) Interactive Software Engineering Inc.
* Erich Gamma, Richard Helm, Ralph Johnson, John Vlissides: *Design Patterns: Elements of Reusable Object-Oriented Software* Addison-Wesley (1995)
* Gaurav Aroraa, Jeffrey Chilberto: *Hands-On Design Patterns with C# and .NET Core*. Packt Publishing (1995)
