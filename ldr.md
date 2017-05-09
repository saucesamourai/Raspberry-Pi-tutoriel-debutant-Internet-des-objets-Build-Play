# Résistance photo-sensible 

## Entrée analogique

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

Une LDR est un type spécifique de résistance.

![](images/ldr.png)


Lorsque la lumière atteint la LDR, la résistance de cette dernière diminue, mais dans le noir, sa résistance est très élevée.

En plaçant un condensateur en série avec la  LDR, le condensateur se chargera à différentes vitesses selon qu'il soit exposé à de la lumiere ou non.

## Créer un circuit photo-sensible

1.  Installer votre LDR sur votre circuit comme montré ci-dessous:

![](images/Laser-tripwire_1-01.png)

1.  Maintenance ajouter le condensateur en serie avec la LDR. Comme le condensateur est un composant bi-polaire, assurer vous que la longue patte est sur la même patte que celle de la LDR.

![](images/Laser-tripwire_2-01.png)

1.  Enfin, ajouter un cable afin de connecter les deux élements à votre Raspberry Pi.

![](images/Laser-tripwire_3-01.png)

## Coder un capteur de lumiere

La majeure partie du code à écrire pour détecter les différents niveaux lumineux reçus par la LDR est issue de la bibliothèque `gpiozero`. Cette bibliothèque traitera la charge et la décharge du condensateur pour vous.

Utiliser le code suivant pour mettre en route votre capteur de lumiere:

```python
  from gpiozero import LightSensor, Buzzer

  ldr = LightSensor(4)  # alter if using a different pin
  while True:
      print(ldr.value)

```
Compiler votre code puis passer votre main sur la LDR. Vous devriez pouvoir observer la valeur changeée en fonction de l'intensité lumineuse à laquelle la LDR est exposee. 

## Et ensuite ?

- You pouvez mettre en pratique vos nouvelles connaissances pour construire un [laser-tripwire](https://www.raspberrypi.org/learning/laser-tripwire/).
- Passez à l'étape suivante avec un [Detecteur de mouvement infrarouge](pir.md)
