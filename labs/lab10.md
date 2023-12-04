# Cvičenie 10: Súbory, steny a prekážky

Témou dnešného cvičenia je práca so súbormi, hlavne pre prípady, kde teraz v riešení máme surovo napísané neprogramové hodnoty v kóde. Konkrétne sa jedná o mapu, resp. prípravu sveta, ako aj údajov o rôznych kúzlach.

[Ak ste kúzla neimplementovali vo vašom projekte, stiahnite si ukážkové riešenie a pridajte potrebné súbory.](lab10/Game_lab09.zip)

## 1. krok: Načítanie mapy

Pre definíciu mapy existuje hneď niekoľko možností, my budeme používať jednoduché riešenie, ktoré nám ponúka [softvér Tiled](https://www.mapeditor.org). Stiahnite a nainštalujte si túto aplikáciu.

### 1.1. Where's My Map?

Následne vo vašom projekte pridajte do priečinka *Content* súbory [`map01.tmx`](lab10/map01.tmx), [`tileset_basic.tsx`](lab10/tileset_basic.tsx) a [`tileset_basic.png`](lab10/tileset_basic.png). Z týchto troch iba posledný potrebujete pridať do projektu aj cez MGCB Editor, ostatné iba nastavte v IDE tak, aby sa nakopírovali do správneho priečinku pri spustení programu (napr. vo Visual Studio pravý klik na súbor, *Properties*, a následne potrebujete nastaviť hodnotu *Copy to Output* na *Copy always*).

Otvorte si mapu pomocou *Tiled* a oboznámte sa s reprezentáciou mapy, najmä sa pozrite na jednotlivé vrstvy mapy.

### 1.2. `WorldMap`

V ďalšom kroku si vytvoríte triedu `WorldMap`, ktorá bude slúžiť na načítanie mapy a tým aj pozadia z uloženého súboru `.tmx`. Nepotrebujete k tomu nič programovať, iba do priečinka *World* pridajte nasledovnú triedu (ak potrebujete, upravte namespace). Kód využíva balík `TiledCS`, ktorý si viete nainštalovať cez *NuGet*.

```
using Microsoft.Xna.Framework;
using Microsoft.Xna.Framework.Content;
using Microsoft.Xna.Framework.Graphics;
using MyGame.Factories;
using System;
using System.Collections.Generic;
using System.Linq;
using TiledCS;

namespace MyGame.Worlds
{
    public class WorldMap
    {
        private TiledMap _map;
        private TiledTileset _tileset;
        private Texture2D _tilesetTexture;
        private TiledLayer objectLayer;
        private TiledLayer wallLayer;

        private int _tileWidth;
        private int _tileHeight;
        private int _tilesetTilesWide;
        private int _tilesetTilesHeight;

        private List<TiledObject> objects;
        private GameWorld world;
        private IFactory aFactory;

        public WorldMap(string mapPath, string tilesetPath, string tilesetTexture, ContentManager content, GameWorld world, IFactory factory)
        {
            this.world = world;
            world.SetMap(this);
            aFactory = factory;

            _map = new TiledMap(mapPath);
            _tileset = new TiledTileset(tilesetPath);
            _tilesetTexture = content.Load<Texture2D>(tilesetTexture);

            _tileWidth = _tileset.TileWidth;
            _tileHeight = _tileset.TileHeight;
            _tilesetTilesWide = _tileset.Columns;
            _tilesetTilesHeight = _tileset.TileCount / _tileset.Columns;

            objectLayer = _map.Layers.First(l => l.name == "objects");
            wallLayer = _map.Layers.First(l => l.name == "walls");
            objects = objectLayer.objects.ToList<TiledObject>();

            foreach (TiledObject obj in objects)
            {
                world.AddActor(aFactory.Create(
                    obj.@class, obj.name, (int)obj.x, (int)obj.y
                ));
            }
        }

        public void DrawMap(SpriteBatch spriteBatch)
        {
            spriteBatch.Begin();

            for (var i = 0; i < _map.Layers[0].data.Length; i++)
            {
                int gid = _map.Layers[0].data[i];

                if (gid == 0)
                {

                }
                else
                {
                    int tileFrame = gid - 1;

                    var tile = _map.GetTiledTile(_map.Tilesets[0], _tileset, gid);

                    int column = tileFrame % _tilesetTilesWide;
                    int row = (int)Math.Floor((double)tileFrame / (double)_tilesetTilesWide);

                    float x = (i % _map.Width) * _map.TileWidth;
                    float y = (float)Math.Floor(i / (double)_map.Width) * _map.TileHeight;

                    Rectangle tilesetRec = new Rectangle(_tileWidth * column, _tileHeight * row, _tileWidth, _tileHeight);

                    spriteBatch.Draw(_tilesetTexture, new Rectangle((int)x, (int)y, _tileWidth, _tileHeight), tilesetRec, Color.White);
                }
            }

            spriteBatch.End();
        }

        public bool IsWall(int x, int y)
        {
            return false;
        }
    }
}

```

Rozšírte aj triedu `GameWorld` o metódu (pridajte aj členskú premennú):

```
public void SetMap(WorldMap map)
{
    this.map = map;
}
```

Programovanie vás čaká v triede `Game1`, v ktorej potrebujete urobiť nasledovné zmeny:

1. zadefinujte novú členskú premennú typu `WorldMap`;
2. vytvorte inštanciu `WorldMap` a uložte ju v metóde `LoadContent()`, nezabudnite vhodne nastaviť všetky parametre konštruktora (pre textúru stačí zadať názov, pri mapách a tilesete treba zadať relatívnu cestu);
3. v metóde `Draw()` zabezpečte správne vykreslenie mapy a dbajte aj na správne poradie vykresľovania.

Ak ste postupovali správne, vykreslí sa vám nielen mapa, ale aj postava bez potreby na to, aby ste aktorov pridávali ručne v `LoadContent`e. Vymažte preto manuálne vytváranie objektov v tejto metóde, a prípadne upravte mapu pridávaním nových boxov (objektov) pre rôznych aktorov. Na veľkosti boxov nezáleží, načíta sa iba trieda, meno, a pozícia ľavého horného rohu.

Ak chcete celý svet vidieť naraz, tak do `Game1.Initialize()` pridajte nasledovný kód (rozmery sveta upravte aj v konštruktore `GameWorld`):

```
_graphics.IsFullScreen = false;
_graphics.PreferredBackBufferWidth = 800;
_graphics.PreferredBackBufferHeight = 592;
_graphics.ApplyChanges();
```

### 1.3. Another Brick in the Wall

Náš svet už síce vyzerá krajšie, avšak máme tu jeden jasný problém: náš hráč stále dokáže prejsť stenami, a gravitácia ho pritiahne až na spodok obrazovky. V mape však máme *layer* stien, ktorá sa nám načíta aj do členskej premennej `wallLayer` v triede `WorldMap`. V tomto kroku pridáme kolíziu so stenami, pričom postupujte nasledovne:

1. Implementujte metódu `IsWall(int x, int y)` vo `WorldMap`e, ktorá vracia `true` ak *tile* pripadajúca pozícii `[x, y]` reprezentuje stenu. Tento údaj viete načítač z `wallLayer`, pričom potrebujete vypočítať index danej *tile* (v premennej `_map` nájdete všetky potrebné parametre ako veľkosť *tilov*, ale aj ich počet po riadkoch resp. stĺpcoch). Informácie o *tiloch* nájdete v jednorozmernej sekvencii `wallLayer.data`, pričom hodnota je 0, ak pozícia je voľná, a nenulová, ak je tam stena. Pri práci s vrstvami sa môžete inšpirovať metódou `DrawMap()`.
2. Pridajte deklaráciu metódy `bool IntersectsWithWall(IActor actor)` do `IWorld` a následne túto metódu implementujte v `GameWorld` triede. Metóda nech vráti `true`, ak sa aktor, ktorý dostane ako parameter, dotýka steny hociktorým smerom.
3. Upravte `Move` tak, aby aktora presunul iba v prípade, ak jeho nová pozícia nepredstavuje stenu (resp. by sa nedotýkal steny po presune).
4. Upravte `Fall` tak, aby hráč nepadal až po spodok obrazovky, ale iba po najbližšiu platformu (vodorovnú stenu).

Vaše riešenie otestujte a dolaďte.

## 2. krok: File (Not) Found

Minulý týždeň ste si pripravili nástroje na vytváranie kúziel pomocou návrhového vzoru *builder*. Síce riešenie vyžadovalo niekoľko pomocných tried, v konečnom dôsledku ste nemuseli definovať triedy pre každé jedno kúzlo, iba pre dva typy: `ProjectileSpell` a `SelfCastSpell`. Konkrétne kúzla boli už popísané v rámci `SpellDirector` a skladali sa z niekoľkých efektov, pričom každý z nich implementoval `ICommand`.

### 2.1. Potrebné súbory
V tomto kroku upravíme definíciu kúziel a to tak, že zoznam efektov a cenu načítame zo súborov. Vytvorte si teda dva súbory: `spell.csv` a `effects.json` v priečinku `Content` (nezabudnite nastaviť kopírovanie podobne, ako ste to robili pre mapu a tileset). Súbor `spell.csv` bude obsahovať [hodnoty oddelené čiarkou](https://en.wikipedia.org/wiki/Comma-separated_values) a bude definovať nasledovné atribúty (zároveň prvý riadok súboru):

```
name,spelltype,texturepath,texturewidth,textureheight,effect1,effect2,effect3
```

Súbor `effects.json` definuje zoznam efektov pre podporu jednotlivých kúziel a predstavuje `JArray`:

```
[
    {
        "name":"DoT",
        "cost":5
    },
]
```

Obsah súborov upravte podľa vašej implementácie rôznych kúziel, v projekte nezabudnite nastaviť ich kopírovanie.

### 2.2. Načítanie dát
Po vytvorení zdrojových súborov potrebujeme ešte zadefinovať spôsob načítania ich obsahu, k čomu zadefinujeme rozhranie `ISpellDataProvider` a triedu `SpellDataProvider`. Rozhranie nech deklaruje metódy:

```
public interface ISpellDataProvider
{
    public Dictionary<string, SpellInfo> GetSpellInfo();
    public Dictionary<string, int> GetSpellEffects();
}
```

Pre načítanie konkrétnych údajov si vytvoríme triedu `SpellDataProvider`, ktorá implementuje rozhranie `ISpellDataProvider` a návrhový vzor *singleton*. Pre jednoduchosť môžete predpokladať, že súbory budú mať stále rovnaký názov a relatívnu cestu (v rámci projektu).

### 2.3. Načítanie `SpellInfo`
Keby sme postupne načítali len riadky zo súboru `spell.csv`, tak by sme dostali reťazce, ktoré však potrebujeme využiť pre vytvorenie vnútornej reprezentácie pomocou `SpellInfo`. Mohli by sme do `SpellInfo` pridať konštruktor, kde by sa načítaný riadok parsoval, alebo by sme si mohli tento riadok rozdeliť už pri načítaní a nastaviť jednotlivé vlastnosti priamo. Čo keby sme sa ale rozhodli napísať niečo takéto:

```
SpellInfo spellInfo = "name,spelltype,animationpath,animationwidth,animationheight,effect1,effect2,effect3"
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

### 2.4. Načítanie efektov
Čo sa týka efektov, tu máme mapovanie medzi `JArray` a doteraz neznámym typom.

C# pozná typ `dynamic` (všetko je riešené počas behu), ale je podstatne pomalší. Hodí sa použiť vtedy, keď nevieme pri kompilácii povedať, čo štruktúra bude obsahovať. My to ale vieme, takže nám stačí zadefinovať si novú triedu, ktorú nepotrebujeme, aby bola dostupná nikde inde, iba počas parsovania JSONu.

Vytvorte si v `SpellDataProvider` vnorenú triedu `private class SpellEffect`, ktorá bude slúžiť ako model pre čítanie daného JSONu. Na samotné čítanie použite:

```
JsonConvert.DeserializeObject<List<CustomerJson>>(json);
```

### 2.5. Skrášlenie riešenia
V prípade, že sa objaví pri čítaní hociktorého súboru neplatná hodnota, vyvolajte vhodnú výnimku s informatívnou chybovou správou (asi najvhodnejšie `ArgumentException`). Zabezpečte, aby sa načítali všetky platné riadky (nech program nespadne!). Ak by sa vyskytla iná (neočakávaná chyba), program má spadnúť.

## 3. krok: A čo mapy?

V našom projekte ostali ešte surové cesty práve pri načítavaní mapy. Ak chcete, môžete si vytvoriť konfiguračný súbor, ktorý v sebe bude obsahovať tieto informácie (cestu k súborom potrebným pri konštruktore `WorldMap`). Následne vytvorte si pomocnú triedu, ktorá tieto informácie načíta, a upravte aj volanie konštruktora. Takto ak chcete zmeniť mapu, nepotrebujete upraviť kód, iba konfiguračný súbor.
