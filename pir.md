# Utiliser un détecteur de mouvement infrarouge (en anglais, Passive infrared motion sensor ou PIR)

Les humains et les animaux émettent des radiations en permanence. Rien de grave à cela cependant, car le type de radiation que nous émettons sont des rayonnements infrarouges inoffensifs aux niveau que nous émettons. En fait, tout objet dont la température est supérieur au zéro absolu (- 273,15 °C) émet un rayonnement infrarouge.

Le détecteur infrarouge (PIR) détecte les changements de rayonnement infrarouge qu'il reçoit. Lorsqu'un changement significatif de volume de radiation reçu est détecté, le capteur émet une impulsion électrique. Ceci signifie qu'un capteur PIR peut détecter le mouvement d'un humain ou d'un animal devant lui. 

![pir](images/pir_module.png)

## Câbler un détecteur de mouvement infrarouge
Lorsque le PIR détecte un mouvement, il émet une impulsion électrique mais trop faible pour être décelée. Elle doit donc être amplifiée. C'est pourquoi le PIR est alimenté.

Si vous ne trouvez pas 

The pulse emitted when a PIR detects motion needs to be amplified, and so it needs to be powered. There are three pins on the PIR; they should be labelled `Vcc`, `Gnd`, and `Out`. If these labels aren't clear, they are sometimes concealed beneath the Fresnel lens (the white cap), which you can temporarily remove to see the pin labels.

![wiring](images/pir_wiring.png)

1. As shown above, the `Vcc` pin needs attaching to a `5V` pin on the Raspberry Pi.
1. The `Gnd` pin on the PIR sensor can be attached to *any* ground pin on the Raspberry Pi.
1. Lastly, the `Out` pin needs to be connected to any of the GPIO pins.

## Detecting motion

You can detect motion with the PIR using the code below:

```python
from gpiozero import MotionSensor

pir = MotionSensor(4)

while True:
    if pir.motion_detected:
        print("You moved")
```

## What Next?

- Now you know how to use a PIR, why not have a go at building a [Parent Detector](https://www.raspberrypi.org/learning/parent-detector)
- Continue to the next worksheet on using an [Ultrasonic Distance Sensor](distance.md)
