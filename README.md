# Labor 08

## Feladatok
1. Írjunk egy szöveges fájlkereső alkalmazást, megy egy adott könyvtárban megkeresi az összes állományt és visszatéríti azokat melyek tartalmazzák a keresett karakterláncot. Ha a felhasználó nem specifikálja (üres) a keresési karakterláncot, térítsük vissza az összes
állományt. Az állományok nevét és méretét jelenítsük meg tábálázat formájában.
   - A könyvtár kiválasztásához használjuk [```QFileDialog```](https://doc.qt.io/qt-5/qfiledialog.html) osztályt illetve, a ```QDir::currentPath()``` függvényt az aktuális könyvtár lekéréséhez.
   - A könyvtárak kezelésére, illetve a könyvtár struktúra bejárásához használjuk a beépített [```QDir```](https://doc.qt.io/qt-5/qdir.html) és [```QDirIterator```](https://doc.qt.io/qt-5/qdiriterator.html) osztályokat.
   - A szöveges fájlok feldolgozásához, tartalmuk beolvasásához a [```QFile```](https://doc.qt.io/qt-5/qfile.html) és [```QTextStream```](https://doc.qt.io/qt-5/qtextstream.html) osztályokat használjuk. Mivel a fájlok feldolgozása sok időt vehet igénybe, a [```QProgressDialog```](https://doc.qt.io/qt-5/qprogressdialog.html) segítségével jelezzük a felhasználó felé, hány százalékát dolgoztuk fel a fájloknak, hol tartunk.
   - A keresés eredményét egy [```QTableWidget```](https://doc.qt.io/qt-5/qtablewidget.html) objektum segítségével jelenítjük meg, a fájlokról információkat (pl. méret) a [```QFileInfo```](https://doc.qt.io/qt-5/qfileinfo.html) segítségével kérhetünk le.
   - [Egy lehetséges megoldás, lépésről lépésre](http://doc.qt.io/qt-5/qtwidgets-dialogs-findfiles-example.html)
      ![findfiles-example](https://user-images.githubusercontent.com/78269344/112871250-bf75eb80-90c7-11eb-98cb-ac22d026205e.png)

2. Listázzuk ki egy könyvtárban hány futtatható alkalmazás (```application/x-executable```), illetve hány C/C++ forrásállomány található (```text/x-c++hdr, text/x-c++src, text/x-csrc```). A fájl típusát a [```QMimeType```](https://doc.qt.io/qt-5/qmimetype.html) segítségével tudjuk lekérdezni, a lehetséges [MIME típusok listája](https://mediatemple.net/community/products/dv/204403964/mimetypes)

3. Írjunk egy bináris képekben kereső alkalmazást, ahol a keresendő bájtsorozatot megadhatjuk hex vagy bináris kódolásban. Az adatok beolvasásához használjunk [```QDataStream```](https://doc.qt.io/qt-5/qdatastream.html)-et. A fájl típusát a [```QMimeType```](https://doc.qt.io/qt-5/qmimetype.html) segítségével tudjuk lekérdezni (```image/*```) .

### Qt és MySql adatbázis összekötése --> [demó](https://github.com/szabolcscsaholczi/Qt_lab10_Database.git)
* Hozzuk létre a tábláinkat/adatainkat a PhpMyAdmin segítségével.
* Módosítsd a ```.pro``` fájlt:
```c++
         QT       += core gui sql
```
* Szükséges könyvtárak:
```c++
      #include <QtSql>
      #include <QSqlDatabase>
      #include <QSqlQuery>
```
* Összekötés: 
```c++
       QSqlDatabase db = QSqlDatabase::addDatabase("QMYSQL");
    
       db.setHostName("localhost");  
       db.setUserName("root");       // az alapvető felhasználónév xamppban 
       db.setPassword("");           // alapvetően nincs beállítva jelszó 
       db.setDatabaseName("qt_database");
       
       if (db.open()) 
       {
           QMessageBox::information(this, "Kapcsolódás", "Kapcsolódás sikeres.");
       }
       else
       {
           QMessageBox::information(this, "Kapcsolódás", "Kapcsolódás sikertelen.");
       }
```
* Lekérdezések:
```c++
         string userid;

         QSqlQuery mySqlQuery;
         mySqlQuery.exec("SELECT * FROM users");

         if (mySqlQuery.size() > 0) {
             while (mySqlQuery.next()) {
                 userid = mySqlQuery.value("id").toString().toUtf8().constData();
                 username = mySqlQuery.value("name").toString().toUtf8().constData();
             }
         }

```
