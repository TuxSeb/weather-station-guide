# Demonstration du site web pour la Station météo
Weather station demo website

Ce tutoriel va vous monter comment creer un site web simple, montrant les graphiques des données de la station météo Raspberry Pi.

## Obtenez le code d'enregistrement

1. Pour cela, il vous faut l'accès root sur le Raspberry Pi. Depuis le terminal, tapez: 

    `sudo -s`

2. Navigez vers le dossier web

    `cd /var/www/html`

3. Téléchargez les fichier dans un dossier nommé `demo`

    `git clone https://github.com/raspberrypi/weather-station-www.git demo`
  
4. Téléchargez et décompressez la librairie JavaScript de traçage [flot](http://www.flotcharts.org/) 

    `cd demo/js`

    `wget http://www.flotcharts.org/downloads/flot-0.8.3.zip`

    `unzip flot-0.8.3.zip`


1. Retournez sur le site de démonstration mère Return to the demo site root:

    `cd ..`

vous devriez maintenant être dans `/var/www/html/demo`

##Installer et connecter
  
1. Mettre à jour le script du PHP avec le mot de passe MySQL que vous avez choisi lors de l'installation de la base de données :

    `nano data.php`
  
   Trouvez la ligne: `$con=mysqli_connect("localhost","root","raspberry","weather");`
  
    Mettez a jour  `raspberry` vers un autre mot de passe que vous avez choisis.
  
    Appuyez sur `Ctrl O` puis `Enter` pour sauvegarder, et `Ctrl X` pour quitter nano.
  
2. Repetez les instuction précédentes pour `csv.php`.

3. Trouvez l'adresse IP de la station météo
 
     `ifconfig`
  
    L'adresse IP sera sur la seconde ligne juste après `inet addr:`.
    The IP address will be on the second line just after `inet addr:`.

Entrez cette adresse IP dans un navigateur, suivis de `/demo`. Par exemple `http://192.168.0.X/demo`.
  
  Une page devrait s'afficher, montrant différents graphiques. Notez qu'il n'y a pas la direction du vent. 
  Vous pouvez faire glisser le graphique à gauche ou à droite avec le bouton gauche de la souris , ou un zoom avant et arrière avec la molette de la souris .
 

## Télécharger les données

Si vous preferez travavailler avec Micro$oft office (ou équivalent), les données peuvent être extraite dans un fichier CSV et importé directement.
Entrez l'adresse IP de la station météo dans le navigateur, suivis de `/demo/csv.php`. Par exemple: `http://192.168.0.X/demo/csv.php`. Le navigateur va vous donner un fichier CSV à télécharger, qui contiendra une décharge complète de toutes les données dans la base de données MySQL.

Il est également possible de spécifier une plage de dates pour sélectionner des enregistrements pour l'inclusion dans le fichier CSV . Cela se fait en spécifiant un ` from` et / ou paramèté la date de`to` sur la chaîne de requête .

Le format de date est: `"AAAA-MM-JJ HH:MM:SS"`.Les paramètres de temps doivent être placés entre guillemets . Par exemple:

  - `http://192.168.0.X/demo/csv.php?from="2014-01-01"`
  - `http://192.168.0.X/demo/csv.php?to="2014-12-31 23:59:59"`
  - `http://192.168.0.X/demo/csv.php?from="2014-01-01"&to="2015-01-01 12:00:00"`

  Si vous laissez le ` from` et `to` (comme dans l'étape précédente ) , alors il fait une décharge complète de toutes les données dans la base de données MySQL .
 
