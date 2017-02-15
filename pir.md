# Utiliser un détecteur de mouvement infrarouge (en anglais, Passive infrared motion sensor ou PIR)

Les humains et les animaux émettent des radiations en permanence. Rien de grave à cela cependant, car le type de radiation que nous émettons sont des rayonnements infrarouges inoffensifs aux niveau que nous émettons. En fait, tout objet dont la température est supérieur au zéro absolu (- 273,15 °C) émet un rayonnement infrarouge.

Le détecteur infrarouge (PIR) détecte les changements de rayonnement infrarouge qu'il reçoit. Lorsqu'un changement significatif de volume de radiation reçu est détecté, le capteur émet une impulsion électrique. Ceci signifie qu'un capteur PIR peut détecter le mouvement d'un humain ou d'un animal devant lui. 

![pir](images/pir_module.png)

## Câbler un détecteur de mouvement infrarouge
Lorsque le PIR détecte un mouvement, il émet une impulsion électrique mais trop faible pour être décelée. Elle doit donc être amplifiée. C'est pourquoi le PIR est alimenté. Son alimentation électrique se fait via les broches 'Vcc' et 'Gnd'. La broche 'Out' est celle qui va renvoyer l'information lorsqu'un mouvement est détecté.

Si vous n'arrivez pas à trouver les inscriptions des noms des broches 'Vcc', 'Gnd' et 'Out', c'est qu'ils sont probablement cachés sous la lentille de Fresnel (le capot en plastique blanc). Vous pouvez le déclipser en tirant dessus le temps de faire vos branchements.

![wiring](images/pir_wiring.png)

1. La broche 'Vcc' doit être reliée au '5V' du Raspberry Pi.
1. La broche 'Gnd', "masse" en français, doit être reliée à n'importe quelle broche 'GND' du Raspberry Pi.
1. Enfin, la broche `Out`, "sortie" en français, doit être connecté à n'importe quelle broche GPIO.

## Detecter les mouvements

Vous pouvez détecter les mouvements en utilisant le PIR avec le code ci-dessous:

```python
from gpiozero import MotionSensor

pir = MotionSensor(4)

while True:
    if pir.motion_detected:
        print("Vous avez bougé!")
```
D'abord on importe la fonction MotionSensor, puis on crée la variable 'pir' avec cette fonction.
Ensuite on crée une boucle infinie pour afficher "Vous avez bougé!" dès qu'un mouvement est détecté

## Que faire ensuite ?

- Passez au prochain exercice pour faire fonctionner le [module de mesure de distance à ultrasons](distance.md)
