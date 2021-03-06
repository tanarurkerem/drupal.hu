# Drupal.hu karbantarto kezikonyv

## Build status

Stabil ag: [![Build Status](https://travis-ci.org/drupalhu/drupal.hu.svg?branch=stable-acquia)](https://travis-ci.org/drupalhu/drupal.hu)


## Hogyan fejlesszek a drupal.hu kodjan?

### Adatbázis beszerzése

Le tudsz tölteni egy [tisztított adatbázis dump](http://dev.drupal.hu/files/druphungary-dev.tar.gz)ot,
ami nem tartalmaz érzékeny adatokat. (Ha találsz benne jelezd nekünk, hogy törölni tudjuk.)

### Uj kontributor beallitasa

1. [Forkold](http://help.github.com/fork-a-repo/) a hivatalos repot: https://github.com/drupalhu/drupal.hu

2. Klonozd a projectet a gepedre (a USERNAME a te githubos userneved)

	```
	$ git clone git@github.com:USERNAME/drupal.hu.git
	```

3. Add hozza a hivatalos repot remote-kent, a neve legyen "upstream".

	```
	$ cd drupal.hu
	$ git remote add upstream git://github.com/drupalhu/drupal.hu.git
	```

4. Keszits egy uj branchot a munkadhoz

	```
	$ git checkout -b nagyonmeno-feature
	```

5. Fejlessz az uj branchedben

6. Minden valtoztatast committolj ebbe az uj branchbe

	```
	$ git add .
	$ git commit -m "Use english commit messages and use full sentences that describe the change."
	```

7. Pushold a kodot a githubon levo forkodba.

	```
	$ git push origin nagyonmeno-feature
	```

8. 5-7 lepeseket ismeteld, ameddig keszen nincs amin dolgoztal.

9. Huzd le az upstream-rol a tobbiek esetleges valtoztatasait (ha kozben masok is dolgoznak a projecten, akkor a hivatalos repo tartalma megvaltozhat).

	```
	$ git fetch upstream
	```

10. Frissitsed a helyi stabil kodot.

	```
	$ git checkout stable-acquia
	$ git pull upstream stable-acquia
	```

11. Rebase-ld a te branchedben levo valtoztatasokat az upstream acquia-stable aganak tetejere

	```
	$ git checkout nagyonmeno-feature
	$ git rebase stable-acquia
	```

12. Rebase kozben conflictok konnyen bekovetkezhetnek. Ezek feloldasa utan ``git add .`` frissiti az indexet, es folytatni lehet a rebase-t:

	```
	$ git rebase --continue
	```

13. Pushold a kododat a sajat origin repodba. Miutan a rebase atpakolja a committokat es gyakorlatilag ujakat hoz letre, ezert valoszinuleg --force-al lehet majd csak pusholni.

    ```
    $ git push --force origin nagyonmeno-feature
    ```

14. Keszits egy Pull Requestet a hivatalos repo oldalan a forkodban talalhato uj branchedre, hogy a karbantartok beolvaszthassak a kodot.

### Tovabbi kontribociok menete

1. Mielott megkezded a munkat huzd le az upstream valtoztatasait, es rebaseld a sajat repodat. Ezaltal nem kell felesleges merge committokkal szennyezni a sajat logodat.
2. Valassz ki egy branchot amin dolgozni akarsz
3. Implementald a valtoztatasokat, valtoztasd a kodot
4. Committold a helyi, sajat repository-dba, es pushold a sajat githubos forkodba.
5. Keszits egy Pull Requestet a hivatalos repo oldalan

## Instant tesztkörnyezete Docker és Fig segítségével

FIGYELMETETÉS - Jelenleg teszt üzemmódban van ez a rész, ha kipróbálod és visszajelzel, segíted a munkánkat.

### Telepítés

A tetszkörnyezet használatához a Docker-re(OSX és Windows boot2docker) és a Fig-re van szükség. Telepítsd azokat. (hamarosan kibővítjük ezt a leírást)

### Elindítás és parancssor

A repo gyökér könyvtárában (ahol a .git és a fig.yml fájlok vannak) add ki a következő parancsot:

  ```
  fig up -d
  ```

Legeslegelső futtatásnál várnod kell, mert le fogja tölteni a két image-t a netről. Ez olyan 500Mb. Türelem.

Ezután már csak létre kell hoznod egy drupal adatbázist és beimportálnod bele az adatokat, vagy indítani egy install.php-t.
Hogy hogyan? Lásd a következő bekezdés.

Ha szeretnél parancsokat futtatni a környezetben, pl. drush, vagy mysql, vagy php vagy bármi más, add ki a következő parancsot:

  ```
  docker exec -i -t drupalhu_web_1 bash
  ```

Ez olyan mint az ssh, de nagyon nem az. :)



### Használat

Ha minden jól megy, akkor a docker host 80-es portján eléred a webszervert. Ha Linuxot használsz(és root-ként futtattad), akkor localhoston, ha Mac-et, akkor a boot2docker ip-jén éred el a webszervert. (Windowson még nem teszteltem.)

#### Mysql szerver adatai
A telepítés során a mysql szerver felhasználó _root_, jelszó _root_, a host pedig _mysql_.

#### Fájlok helye a konténerben
A docroot a /var/www/html útvonalon érhető el a környezetben, míg a repo gyökere a /home/dev mappában található.

