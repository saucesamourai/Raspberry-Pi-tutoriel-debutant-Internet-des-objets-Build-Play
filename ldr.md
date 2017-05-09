# Résistance photo-sensible 

## Entre analogique

En génie électrique, il existe deux types d'entrées / sorties (E / S): une première categorie dite analogique et une seconde dite numériques. Les E / S numériques sont assez faciles à comprendre : elles n'ont que deux états : on/off, 0 ou 1.

Concernant les tensions d'entrée sur le Raspberry Pi, toute entrée inférieure à 1,8V est considérée comme off. Si elle est supérieure à 1,8V est considérée comme on. Pour la sortie, 0V est off et 3.3V est on.

Les entrées analogiques sont un peu plus complexes. Contrairement aux entrées numériques, le Rapberry Pi peut recevoir n'importe quel voltage compris entre 0V et 3.3V. En conséquence, le Raspberry Pi ne peut pas detecter de quoi il s'agit exactement.


![](images/analogue-digital.png)

La question naturelle est donc comment pouvons-nous utiliser un Raspberry Pi pour déterminer la valeur d'une entrée analogique et la convertir en un signal utile pour le Raspberry Pi?


### Utilisation d'un condensateur pour lire les entrées analogiques

Les condensateurs sont des composants électriques capables de stocker de l'électricité.

![](images/capacitor.png)

Lorsque le courant alimente un condensateur, ce dernier va commencer à stocker les electrons injectés. La tension à travers le condensateur commencera au niveau bas, et augmentera à mesure que la charge s'accumule.

En mettant une résistance en série avec le condensateur, vous pouvez ralentir la vitesse à laquelle le condensateur se charge. Avec une résistance élevée, le condensateur se chargera lentement, alors qu'avec une faible résistance, la recharge du condensateur se fera plus rapidement.

Il est alors possible de déterminer la valeur de résistance à utiliser en fonction du temps de charge du condensateur pour atteinder la valeur de 1.8V - valeur à partir de laquelle le Raspberry Pi considére l'etat comme actif (on).
En calculant le temps nécessaire pour que 


## Résistance photo-sensible (ligh dependent resistor - LDR)

Une LDR est un type de résistance.

![](images/ldr.png)

When light hits the LDR, its resistance is very low, but when it's in the dark its resistance is very high.

By placing a capacitor in series with an LDR, the capacitor will charge at different speeds depending on whether it's light or dark.

## Creating a light-sensing circuit

1.  Place an LDR into your breadboard, as shown below:

![](images/Laser-tripwire_1-01.png)

1.  Now place a capacitor in series with the LDR. As the capacitor is a polar component, you must make sure the long leg is on the same track as the LDR leg.

![](images/Laser-tripwire_2-01.png)

1.  Finally, add jumper leads to connect the two components to your Raspberry Pi.

![](images/Laser-tripwire_3-01.png)

## Coding a light sensor

Luckily, most of the complicated code you would have to write to detect the light levels received by the LDR has been abstracted away by the `gpiozero` library. This library will handle the timing of the capacitor's charging and discharging for you.

Use the following code to set up the light sensor:

```python
  from gpiozero import LightSensor, Buzzer

  ldr = LightSensor(4)  # alter if using a different pin
  while True:
      print(ldr.value)

```

Run this code, then cover the LDR with your hand and watch the value change. Try shining a strong light onto the LDR.

## What Next?

- You could have a go at using your new knowledge of LDRs to build a [laser-tripwire](https://www.raspberrypi.org/learning/laser-tripwire/).
- Continue to the next worksheet on using a [Passive Infra-red Sensor](pir.md)
