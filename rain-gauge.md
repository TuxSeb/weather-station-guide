# Le Pluviomètre

Voici le pluviomètre fournis avec le Kit de Station météo pour Rapsberry Pi:

![Pluviomètre](images/rain_gauge.jpg)

## Comment ça fonctionne ?

Nous pouvons explorer le pluviomètre et son fonctionnement en enlevant le seau , pour cela il faut presser les clips sur le coté, le couvercle devrait alors s'enlever.

![](images/rain_gauge_open.jpg)

Cette jauge de pluie est basiquement un reservoir basculant à vidage automatique. La pluie est colléctée et est acheminée dans le seau. Une fois que l'eau de pluie à été collecté, le seau va basculer, et l'eau va être drainée en dehors de la base, et le réservoir opposé va monter en position

Le produit [datasheet](https://www.argentdata.com/files/80422_datasheet.pdf) nous dit que 0.2794 mm de pluies feront pencher le seau. Nous pouvons alors multiplier cela par le nombre de basculement pour calculer the montant de la chutte de pluie.

Si vous regardez la fin du cable RJ11 du côté du pluviomètre, vous pourrez aperçevoir seulement deux cables dedans: un vert et un rouge. Maintenant regardez la crête entre les deux seau. Dedans vous verrez un petit aimant cylindrique qui pointe vers la parroi arrière. Dans cette, il y a une pièce éléctronique intelligente appellé *commutateur à lames*, Photo ci-dessous.

![](images/reed_switch.jpg)

Le commutateur à lames à a l'interieur deux contacts métaliques qui se touchent quand ils sont sous l'influence d'un aimant. Alors éléctroniquement, cela fonction dans les même condition quand un bouton est connecté au Raspberry Pi. Quand le seau penche, l'aiment passe le commutateurs à lames, l'amenant à se fermer momentanément.

Le dessus de la parroi arrière se détache si vous voullez voir à l'intérieur, pour cela il suffis de tire délicatementr sur le bout plat. À l'interieur, il y a une petite planche de circuit que vous pouvez retirer pour l'examiner. Au millieu vous verrez le commutateur à lames. Remettez le circuit et le couvercle avant de continuer.

## Comment faire pour connecter les capteurs ?

1. Pour connecter le pluviomètre à la station météo, vous allez tout d'abord avoir besoin d'intaller la boite de la [station météorologique] (hardware-setup.md).

1. Localisez le socket sur la station météo marqué **Capteur de pluie** et le grommet correspondant.
1. Dévissez l'œillet du boîtier et vissez le bouchon de jauge de pluie à travers à l'intérieur de la boîte :

  ![Connecting](images\Fix_Grommit.jpg)

1. Connectez la prise sur la carte, et serrez l'oeillet.

Une fois connecté, le pluviomètre utilise le **GPIO pin 6** (BCM).

## Enchantillon de code

Le programme suivant utilise le GPIO de gestion d'interuption pour détecter les données provenant du pluviomètre et les convertirs en mesures significatives qui sont affichées sur l'écran.

```python
  #!/usr/bin/python
  import RPi.GPIO as GPIO

  pin = 6
  count = 0

  def bucket_tipped(channel):
      global count
      count = count + 1
      print (count * 0.2794)

  GPIO.setmode(GPIO.BCM)
  GPIO.setup(pin, GPIO.IN, GPIO.PUD_UP)
  GPIO.add_event_detect(pin, GPIO.FALLING, callback=bucket_tipped, bouncetime=300)
```
