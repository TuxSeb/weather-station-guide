#Utiliser le disque image pour la station météo
Using the weather station disk image

La manière la plus simple pour installer votre Station météo Raspberry Pi par Oracle est d'utiliser l'image pré-installée
The easiest way to get your Raspberry Pi Oracle weather station up and running is to use the pre-built disk image.

## Télécharger et écrire l'image sur une carte SD 
Download and write the image to the SD card

1.	[Télécharger l'image disque](http://downloads.raspberrypi.org/weather_station/images/weather_station-2016-03-24/WeatherStation.zip).

1.	Suivez [Les instructions](https://www.raspberrypi.org/documentation/installation/installing-images/README.md) pour l'écrire sur la carte SD qui est fournie avec la station météo (ou n'importe quelle carte SD qui a au minimum 8GB).
2.	any other SD card that is at least 8Gb).

1. Inserez la carte SD dans le Raspberry Pi qui est connécté à la station météo et allumez le.
Put the SD card into the Raspberry Pi in your weather station and power it up.

## Un peu de ménage

Le Pi va alors booter vers une invite de texte
The Pi will boot to a text prompt:

1. Identifiez vous avec comme `user (utilisateur)` Pi et `raspberry` comme mot de passe.

1. Lancez l'outils de configuration:
2. Run the configuration tool:

`sudo raspi-config`

1. Sélectionnez l'option 1 `Expand Filesystem`.

1. Puis sélectionnez `Finish`.

1. et enfin, sélectionnez `No` quand il est demandé de redemarrer.

1. C'est une bonne idée de **changer votre mot de passe ** tant que vous êtes là. Séléctionnez l'option 2. C'est le mot de passe qui vous allez avoir besoin pour vous connecter dans votre Raspberry Pi comme utilisateur 'pi'.
2. It's a good idea to **change your password** while you're here. Select option 2 `Change User Password`. This is the password you will need to log into your Raspberry Pi as user 'pi'.


## exécution au démarrage et vous connection automatique aux données

1. Allez dans le dossier Weatherstation via le terminal

    `cd WeatherStation` 

1. Mettez en place le ( cron ) tâche planifiée

    `crontab WeatherStation.cron`  

## Vérifiez que le temps est correct
Check that the time is correct

1. Vérifiez la date et le temps
2. Check date and time:

    `date`

1. Si la date est mauvaise, corrigez-là:
2. If the date is wrong, fix it:

     `sudo date -s 'yyy-mm-dd hhh:mm:ss'` 

Par exemple, pour définir la date et le temps au 24 Mars 2016 12:24:56, tapez la commande suivante

     `sudo date -s '2016-03-24 12:34:56'`

  
1. Puis redemarrez

    `sudo reboot`

## Set up your weather station to upload to the Oracle Apex database

At this stage, you have a weather station that reads its sensors and stores the data at regular intervals in a database on the SD card. But what if the SD card gets corrupted? How do you backup your data? And how do you share it with the rest of the world?

Oracle has set up a central database to allow all schools in the Weather Station project to upload their data. It's safe there and you can download it in various formats, share it, and even create graphs and reports. Here's how to do it.

##  Enregistrez votre école
Register your school

Firstly, you'll need to [register your school](oracle.md) and add your weather station. Come back here when you have your weather station passcode.

<a name="credimage"></a>

## Update credential files with your weather station details

This is the one thing we couldn't put in the disk image: your weather station name and password from when you [registered with Oracle's Apex database](). You have to put these manually into the credentials file as follows:


1. Go to your home directory:

    `cd ~`

1. Edit the credentials file using the nano editor:

    `nano credentials.json`

Change the lines: 

``` java
{
    mysql : { host : "localhost", user : "pi", pass: "raspberry", database : "weather" }
}
```

to:

``` java
{
    mysql : { host : "localhost", user : "pi", pass: "raspberry", database : "weather" },
    cloud : { url : "https://apex.oracle.com/pls/apex/raspberrypi/weatherstation/submitmeasurement",
              user: "MyStation", pass: "MyPass"}
}
```

**Double-check curly braces and commas are in the right place!**


Change **MyStation** and **MyPass** to your weather station name and passcode. Make sure you type them in exactly, as they are case-sensitive.


1. Save the file with `CTRL-O`, and press `Enter` then `CTRL-X` to quit nano.
 
## Check that it's working

### Base de données locale et enregistrement

1. 
2. S'enregister dans la base de donnée [Mot de passe: tiger]:

    `mysql -u root -p weather` 

1. Utiliser la commande suivante `SELECT CREATED, LEVEL, TEXT FROM LOG ORDER BY CREATED;`

1. Résultat attendu 

### Oracle remote database

1. connectez-vous au [compte de la base de données Oracle](oracle.md) de votre école. 

2. Allez a "Mesures météorologiques. Vous devriez voir les lectures de la station. La base de données enregistre toutes les 10 minutes sur place , et les envoies à Oracle une fois par heure. Si aucune lecture est affichée, revenez un peu plus tard .

3. Go to 'Weather measurements'. You should see the station readings. The database logs every 10 minutes locally, and uploads to Oracle once per hour. If no readings are showing, check back a little later.

![](images/weather-readings.png)

Vous pouvez télécharger les données dans différents formats et aussi créer des graphiques en utilisant le menu:
You can download your data in various formats and also make charts using the menu:

![](images/wsmenu.png)
