# L'Anémomètre

Voici l'anémomètre inclu dans le kit de station météo pour Raspberry Pi. Il est utilisé pour mesurer la vitesse du vent.

![Anemomètre](images/anemometer.png)

## Comment cela fonctionne ?

Le vent s'engouffre dans les 3 coupes et les font tournées autour.

Pour expliquer comment l'appareil fonctionne, vous pouvez le demonter avec les étapes suivantes: 

1.Premièrement, tenez la base dans une main et tirer sur le  avec l'autre main. Vous n'avez pas besoin de forcer.

2.Regarder en dessous des coupes et vous verrez un petit cylindre métalliquee sur un coté. c'est un aimant, comme celui dans les seau du pluviomètre. Testez le avec un trombone si vous voulez.

![Anemometer Magnet](images/anemometer_magnet.png)

3.Maintenant utilisez un tournvevis pour enlever les trois vis de la base. La base devrait devrait apparaître facilement. Faites glisser le cable pour le sortir de la voie. Si vous regardez à l'interieur, vous verrez encore notre vieil ami, le commutateur à lames.   

![Anemometer Reed](images/anemometer_reed.png)

## Que-ce que cela signifie ?

Quand les coupes sont dans leurs positions originelles et tournent, l'aimant va tourner dans un cercle serré au desus du commutateur à lames. Pour chaque tour effectué, il y aura deux moment où le switch sera fermé.

Si nous pouvons detecter le nombre de rotations dans un temps impartis, nous pouvons alors calculer la vitesse à laquelle le bras est en train de tourner. Comme un peu d'énergie est perdue quand il y a rotation, un anémomètre sous-estime souvent la vitesse du vent. Pour compenser, on multiplie la vitesse calculée par un facteur de 1.18 (specifique pour cet anémomètre).

L'Algorithme suivant peut être utilisé pour calculer la vitesse du vent:

> Pour chaque période de temps **t**  
> --- **count** = signal de l'anémomètre enregistré 
> --- **rotations** = count / 2  
> --- **distance** = rotations * 2 * pi * radius (9cm)  
> --- **vitesse** = distance / t (**in cm/s**)  
> 
> Pour convertir la vitesse **vitesse** en **km/h**  
> --- vitesse = vitesse / 100000 (**km/s**)  
> --- vitesse = vitesse * 3600 (**km/h**)  
> 
>Pour compenser le facteur anémomètre : 
> --- vitesse = vitesse * 1.18  

## Comment connecter le capteur ? 

Pour connecter l'anémomètre à la station météo, vous devez tout d'abord installer la [Boite de la Station météo](hardware-setup.md).

1.Localisez l'emplacement sur la station météo marqué **WIND** and l'oeillet correspondant.
2.L'anémomètre peut directement être connecté à la station, mais idéallement avec la [wind vane](wind_vane.md)
3.Dévissez l'œillet du boîtier et vissez le bouchon de girouette à travers à l'intérieur de la boîte.

  ![Connecter](images\Fix_Grommit.jpg)

4.Connectez la prise au socket et resserez l'oeillet 

Quand l'anémomètre est connecté, il utilise le **GPIO pin 5** (BCM).

## Quelques lignes de code ...

Le programme suivant utiliser les ports GPIO pour detecter les entrées venant de l'anemomètre, et les convertis en données significatives qui sont par la suite affichées sur l'écran. 

```python

import RPi.GPIO as GPIO 
import time, math

pin = 5
count = 0

def calculate_speed(r_cm, time_sec):
    global count
    circ_cm = (2 * math.pi) * r_cm
    rot = count / 2.0
    dist_km = (circ_cm * rot) / 100000.0 # convert to kilometres
    km_per_sec = dist_km / time_sec
    km_per_hour = km_per_sec * 3600 # convert to distance per hour
    return km_per_hour

def spin(channel):
    global count
    count += 1
    print (count)

GPIO.setmode(GPIO.BCM)
GPIO.setup(pin, GPIO.IN, GPIO.PUD_UP)
GPIO.add_event_detect(pin, GPIO.FALLING, callback=spin)

interval = 5

while True:
    count = 0
    time.sleep(interval)
    print (calculate_speed(9.0, interval), "kph")
```
