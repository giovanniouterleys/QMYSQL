#Utiliser MySQL avec QT Creator
Dans le but d’interagir avec les bases de données en C++ avec l’IDE QT Creator, il est important de mettre de compiler les librairies (DLL)  MySQL qui nous permettront d'interagir avec une base de donnée sous WAMPSERVER 64.


### Éditer les PATHS:

Pour compiler des libraries, nous devons utiliser qmake, c’est une variante du programme Make. Son intérêt est “d’assembler” des fichiers pour ensuite créer principalement des exécutables. Il utilise des fichiers appelés makefile qui spécifient comment construire les fichiers cibles.

Pour commencer, rendez-vous sur la page Système, puis Paramètres système avancés:


![alt text](https://github.com/giovanniouterleys/QMYSQL/blob/giovanniouterleys-images/systeme.JPG)


Un onglet vient de s’ouvrir, cliquez sur “Variables d’environnement…” (en bas à droite) 

Vous devriez voir une fenêtre similaire à ça: 

![alt text](https://github.com/giovanniouterleys/QMYSQL/blob/giovanniouterleys-images/path.JPG)


Cherchez “Path” dans le menu déroulant en bas. Puis double-cliquez dessus.

### Pour Windows 10:

Appuyez sur “Nouveau” puis ajoutez la ligne: 

``` C:\Qt\Qt5.12.10\5.12.10\mingw73_32\bin``` 

### Pour Windows 7: 

Quand vou avez cliquez sur Path, une petite fenêtre à dû apparaître, déplacez vous à la fin du TextField puis placez:

``` C:\Qt\Qt5.12.10\5.12.10\mingw73_32\bin``` 

####Attention, il est important de bien séparer les paths entre-eux, avant d’ajouter la ligne vérifiez bien que le dernier caractère de la ligne soit un ```;```

Grâce à cette méthode, nous pouvons appeler qmake partout. 


Maintenant, nous devons télécharger MySQL pour récupérer des outils essentiels à la compilation de notre librairie. 

Rendez-vous sur le site: https://downloads.mysql.com/archives/community/

Puis allez dans archive est sélectionnez dans Products version, la version 5.5.10. Téléchargez le produit en 32 bits.

![alt text](https://github.com/giovanniouterleys/QMYSQL/blob/giovanniouterleys-images/mysqlCDownload.JPG)


Lancez l’installation du logiciel, après certains onglets, vous devez apparaître sur une fenêtre semblable à ça: 


![alt text](https://github.com/giovanniouterleys/QMYSQL/blob/giovanniouterleys-images/mysql1.JPG)

Déroulez le menu “Development Components” avec le petit + puis à Embedded server library, sélectionnez “Will be installed on local hard drive”. De la même façon, mettre une croix devant toutes les autres fonctionnalités sauf Client C API library. Ensuite, cliquez sur le bouton Browse… en dessous et modifiez le répertoire d'installation de telle sorte qu'il n'y ait pas d'espace dans le chemin ! On arrive donc à ceci : 

![alt text](https://github.com/giovanniouterleys/QMYSQL/blob/giovanniouterleys-images/mysql2.JPG)

N’oubliez pas de modifier le chemin du logiciel, l’idéal est C:\MySQL, ça sera bien plus pratique pour après.

Après un clic sur Next, laisser l'installateur travailler. Une fois l'installation terminée, décocher Launch the MySQL Instance Configuration Wizard et quitter.

Ouvrez le CMD, puis 
Tapez cette commande (attention, suivant votre version de QT,  5.12.10 peut changer):

```  cd C:\Qt\Qt5.12.10\5.12.10\Src\qtbase\src\plugins\sqldrivers ```

Désormais, tapez:

``` qmake sqldrivers.pro ```

Grâce à cette commande nous allons créer un fichier qtsqldrivers-config qui va permettre à qmake de compiler la librairie avec le compilateur mgw32.

![alt text](https://github.com/giovanniouterleys/QMYSQL/blob/giovanniouterleys-images/qmake1.JPG)



Ensuite, rentrez dans le dossier mysql avec la commande: 

``` cd mysql ``` 


En parallèle, ouvrez un explorateur de fichier, et rendez vous dans le chemin où ce situe mysql soit:
``` C:\Qt\Qt5.12.10\5.12.10\Src\qtbase\src\plugins\sqldrivers\mysql ```

Puis ouvrez mysql.pro est commentez la ligne:


 ```QMAKE_USE += mysql```
 
 de telle manière
 
  ```#QMAKE_USE += mysql```
  
  soit en image: 

![alt text](https://github.com/giovanniouterleys/QMYSQL/blob/giovanniouterleys-images/ex.JPG)


pour éviter pb de compilation 


Désormais renseignez la commande suivant:

```qmake "INCLUDEPATH+=C:\\MySQL\\include" "LIBS+=C:\\MySQL\\lib\\libmysql.lib" mysql.pro```


Pour comprendre cette commande, on indique au compilateur d’inclure un chemin au .pro de mysql et aussi on lui indique d’ajouter une librairie se situant dans le dossier lib de notre dossier MySQL. 

Puis tapez:

```mingw32-make -f Makefile.Debug```

Là nous allons créer notre DLL pour le mod debug!

Répétez la commande cette fois ci avec le mod release:


```mingw32-make -f Makefile.Release```

On devrait avoir ça:
![alt text](https://github.com/giovanniouterleys/QMYSQL/blob/giovanniouterleys-images/result.JPG)

Ici le compilateur se met en charge de compiler notre programme en créant nos DLL.

Maintenant, rendez-vous dans: 

```C:\Qt\Qt5.12.10\5.12.10\Src\qtbase\src\plugins\sqldrivers\plugins\sqldrivers```

Vous devrez apparaître 4 fichiers:

![alt text](https://github.com/giovanniouterleys/QMYSQL/blob/giovanniouterleys-images/libsresults.JPG)
