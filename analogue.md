![Build & Play, les meilleurs tutoriels DIY pour débuter et progresser dans l'univers de l'Internet des Objets](BuildnPlay_small.png)

# Entrées analogiques

Les broches GPIO de Raspberry Pi sont des broches numériques : les sorties ne peuvent prendre que deux états (on / off) et les entrées ne peuvent lire que des états binaires. Cependant, à l'aide d'une puce ADC (convertisseur analogique-numérique), il devient possible de lire la valeur des périphériques d'entrée analogique tels que les potentiomètres.

## SPI

Les valeurs analogiques sont communiquées au Pi en utilisant un protocole appelé protocole SPI. Bien que cela fonctionne déja avec GPIO Zero, vous pouvez obtenir de meilleurs résultats si vous utilisez le protocole SPI.

1. Ouvrez votre terminal et installez le package 'spidev' 

    ```bash
    sudo apt-get install python3-spidev python-spidev
    ```

1. Ouvrez le panneau de configuration de votre Raspberry Pi depuis le menu principal et activez le protocole SPI dans la section Interfaces

    ![Enable SPI](images/rcgui.png)

1. Clickez sur **OK** et rebooter votre Raspberry Pi.

## Connecter votre puce ADC (MCP3008)

Le MCP3008 est une puce ADC fournissant huit canaux d'entrée. Les huit connecteurs d'un côté sont connectés aux broches GPIO de Pi, et les huit autres sont disponibles pour connecter des périphériques d'entrée analogiques.

Placez la puce MCP3008 dans votre circuit et branchez la soigneusement comme indiqué sur le schéma suivant:

![MCP3008 wiring](images/mcp3008.png)

Alternativement, vous pouvez utiliser le tableau [Analog Zero] (http://rasp.io/analogzero/), qui fournit la puce MCP3008 sur un circuit prêt à l'emploi ainf de vous éviter un câblage compliqué.

## Ajoutez un potentiomètre

Maintenant que la puce ADC est connectée au Pi, vous pouvez connecter les périphériques aux canaux d'entrée. Un potentiomètre est un bon exemple d'un dispositif d'entrée analogique: il s'agit simplement d'une résistance variable. Le Raspberry Pi lit la tension (comprise entre 0V et 3.3V).

![Potentiomètre](images/potentiometer.jpg)

A potentiometer's pins are ground, data, and 3V3. This means you connect it to ground and a supply of 3V3, and read the actual voltage from the middle pin.

Le potentiomètre comprend trois broches: la masse, la donnée et 3V3. Cela signifie qu'il vous faut le connecter à la masse et à une alimentation de 3V3 afin de pouvoir lire la tension réelle sur la broche du milieu.

1. Placez un potentiomètre sur le circuit et branchez le d'un côté à la masse, de l'autre sur la broche de 3V3 et enfin à la broche centrale sur le premier canal d'entrée comme indiqué:

    ![Add a potentiometer](images/mcp3008-pot.png)

## Codez

Maintenant que votre potentiomètre est connecté, sa valeur peut être lue depuis Python !

1. Ouvrez **Python 3** depuis le menu principal.

1. Dans le terminal, importez la classe `MCP3008` de la librairie GPIO Zero

    ```python
    from gpiozero import MCP3008
    ```

1. Créer un objet représentant Create an object représentant le potentiomètre:

    ```python
    pot = MCP3008(0)
    ```

    Il est important de noter que le `0` représente le canal 0 de l'ADC. Il existe 8 canaux (de 0 à 7). 

1. Essayez de lire sa valeur:

    ```python
    print(pot.value)
    ```

    Vous devriez voir un nombre entre 0 et 1, correspondant à la rotation de la molette du potentiomètre.

1. Maintenant lisez la valeur dans une boucle:

    ```python
    while True:
        print(pot.value)
    ```

    Tournez la molette afin de voir la valeur changée.

## PWMLED

Maintenant que vous avez pu lire les valeurs du potentiomètre, nous allons le connecter à un autre périphérique GPIO. 

1. Ajoutez une LED à votre circuit et connectée là à la broche 21 des GPIO:

    ![Add LED](images/mcp3008-pot-led.png)

1. Dans votre code Python, importez la classe `PWMLED` :

    ```python
    from gpiozero import PWMLED
    ```

    La classe `PWMLED` vous permet de contrôler la luminosité de la LED en utilisant la PWM (pulse-width modulation). 

1. Créer un objet `PWMLED` sur l'entrée 21:

    ```python
    led = PWMLED(21)
    ```

1. Testez de contrôler manuellement la LED:

    ```python
    led.on()  # the led should be lit
    led.off()  # the led should go off
    led.value = 0.5  # the led should be lit at half brightness
    ```

1. Connectez la LED au potentiomètre :

    ```python
    led.source = pot.values
    ```

1. Tournez la molette du potentiomètre afin de faire changer la luminosité de la LED !

### "Source and Values"

La librairie GPIO Zero a une feature très puissante appelée : **source and values**. 

Every device has a `value` property (the current value) and a `values` property (a stream of the device's values at all times). Every output device has a `source` property which can be used to set what the device's value should be.

Chaque périphérique possède une propriété `value` (la valeur actuelle) et une propriété` values` (un flux de valeurs du périphérique en tout temps). Chaque périphérique de sortie possède une propriété `source` qui peut être utilisée pour définir la valeur d'entrée.

- `pot.value` donne la valeur actuelle du potentiomètre (il est seulement lu car il s'agit d'un périphérique d'entrée)
- `led.value` est la valeur actuelle de la LED (c'est en mode lecture / écriture: vous pouvez voir ce qu'elle est et vous pouvez la modifier)
- `pot.values` génère en continu la valeur actuelle du potentiomètre
- `led.source` est une manière de définir d'où la LED obtient ses valeurs

Plutôt que de régler en continu la valeur de la LED à la valeur du potentiomètre dans une boucle, vous pouvez simplement jumeler les deux périphériques. Par conséquent, la ligne `led.source = pot.values` est équivalente à la boucle suivante:

```python
while True:
    led.value = pot.value
```

## Plusieurs potentiomètres

1. Ajoutez un deuxième potentiomètre à votre carte à puce et connectez-le à la chaîne 1 de l'ADC: 
Add a second potentiometer to your breadboard and connect it to the ADC's channel 1:

    ![Second potentiometer](images/mcp3008-2pots-led.png)

1. Now create a second `MCP3008` object on channel 1:

    ```python
    pot2 = MCP3008(1)
    ```

1. Make the LED blink:

    ```python
    led.blink()
    ```

    The LED will blink continuously, one second on and one second off.

1. Change the `on_time` and `off_time` parameters to make it blink faster or slower:

    ```python
    led.blink(on_time=2, off_time=2)
    led.blink(on_time=0.5, off_time=0.1)
    ```

1. Now use a loop to change the blink times according to the potentiometer values:

    ```python
    while True:
        print(pot.value, pot2.value)
        led.blink(on_time=pot.value, off_time=pot2.value, n=1, background=False)
    ```

    Note you have to make it blink once in the foreground, so that each iteration gets time to finish before it updates the blink times.

1. Rotate the dials to make it blink at different speeds!

1. Also try changing `blink` to `pulse` and change `on_time` and `off_time` to `fade_in_time` and `fade_out_time` so that it fades in and out at different speeds, rather than just blinking on and off:

    ```python
    while True:
        print(pot.value, pot2.value)
        led.pulse(fade_in_time=pot.value, fade_out_time=pot2.value, n=1, background=False)
    ```

1. Rotate the dials to change the effect.

## What next?

- Use potentiometers to control other GPIO Zero output devices
- Use potentiometers to control the speed of motors
- Use potentiometers to control the visual settings of a Camera Module in real time
- Find more analogue sensors that will work with the ADC
- Use potentiometers to control a player in a [PyGame Zero](http://pygame-zero.readthedocs.io) game, or in [Minecraft](https://www.raspberrypi.org/learning/getting-started-with-minecraft-pi/)
- Continue to the next worksheet on using [Motors](motors.md)
