![Build & Play, les meilleurs tutoriels DIY pour débuter et progresser dans l'univers de l'Internet des Objets](BuildnPlay_small.png)

# Feu de signalisation 

Pour ce tutoriel, vous aurez besoin des éléments suivants :
 - une plaquette de câblage (breadboard)
 - trois LED
 - un bouton
 - un buzzer
 - des câbles
 - des résistances 330 Ohms

## Câblage

Pour commencer, installez tous les composants sur la plaquette breadboard et connectez les aux pins GPIO du Raspberry Pi.

1. Tout d'abord, vous devez comprendre comment chaque composant est connecté :
    - Un bouton poussoir nécessite une broche de masse et une broche GPIO
    - Une LED nécessite une une broche de masse et une broche GPIO et une résistance pour limiter le courant
    - Un buzzer nécessite une broche de masse et une broche GPIO
    
    Chaque composant a besoin de sa propre broche PGIO, mais peuvent partager la même broche de masse. Nous allons utiliser la plaquette pour permettre ces connexions.
    
1. Placez les composants sur la plaquette et connectez les au broches GPIO du Raspberry Pi, selon le schéma suivant:

    ![GPIO diagram](images/camjam1wiring.png)

    Remarquez que la rangée sur le bord long de la breadboard est connectée à une broche de masse du Raspberry Pi. Donc, tous les composants reliés à cette rangée sont reliés à la masse.
    
1. Observez le tableau suivant, indiquant à quel port GPIO raccorder chaque composant:

| Composant | broche GPIO |
| --------- | :------: |
| Button    | 21       |
| Red LED   | 25       |
| Amber LED | 8        |
| Green LED | 7        |
| Buzzer    | 15       |

## Plongez dans le code Python

Ouvrez l'application Python, et commençons en testant le bouton.

1. Ouvrez **Python 3** depuis le menu principal:
    ![Python 3](images/python3-app-menu.png)

1. Créez un nouveau fichier en cliquant sur **File** > **New File**. Ceci ouvrira une seconde fenêtre.

1. Enregistrez ce nouveau fichier en cliquant sur **File** > **Save**; nommez le fichier `trafficlights.py` et enregistrez le dans votre dossier d'accueil.

1. Tapez le code suivant:

    ```python
    from gpiozero import Button

    button = Button(21)

    while True:
        print(button.is_pressed)
    ```

   Dans GPIO Zero, nous créons un objet pour chaque composant utilisé. Chaque interface de composant doit être importée de la librairie 'gpiozero' et une instance doit être crée au niveau de la broche GPIO à laquelle chaque composant est connecté.
   

1. Enregistrez et éxecutez le code en pressant les touches  `Ctrl + S` and `F5`.

1. La fenêtre Python va passer en mode focalisation, et va afficher 'False' en permancence. Quand vous pressez le bouton, l'affichage va passer à 'True', et quand vous relâchez le bouton, l'affichage repassera à 'False'.


    `button.is_pressed` is a property of the `button` object, which provides the state of the button (pressed or not) at any given time.

1. Now return to the code window and modify your `while` loop to show the following:

    ```python
    while True:
        if button.is_pressed:
            print("Hello")
        else:
            print("Goodbye")
    ```

1. Run the code again and you'll see "Hello" printed when the button is pressed, and "Goodbye" when the button is not pressed.

1. Modify the loop again:

    ```python
    while True:
        button.wait_for_press()
        print("Pressed")
        button.wait_for_release()
        print("Released")
    ```

1. When you run the code this time, nothing will happen until you press the button, when you'll see "Pressed", then when you let go you'll see "Released". This will occur each time the button is pressed, but rather than continuously printing one or the other, it only does it once per press.

## Add an LED

Now you'll add an LED into the code and use GPIO Zero to allow the button to determine when the LED is lit.

1. In your code, add to the `from gpiozero import...` line at the top to also bring in `LED`:

    ```python
    from gpiozero import Button, LED
    ```

1. Add a line below `button = Button(21)` to create an instance of an `LED` object:

    ```python
    led = LED(25)
    ```

1. Now modify your `while` loop to turn the LED on when the button is pressed:

    ```python
    while True:
        button.wait_for_press()
        led.on()
        button.wait_for_release()
        led.off()
    ```

1. Run your code and the LED will come on when you press the button. Hold the button down to keep the LED lit.

1. Now swap the `on` and `off` lines to reverse the logic:

    ```python
    while True:
        led.on()
        button.wait_for_press()
        led.off()
        button.wait_for_release()
    ```

1. Run the code and you'll see the LED stays on until the button is pressed.

1. Now replace `led.on()` with `led.blink()`:

    ```python
    while True:
        led.blink()
        button.wait_for_press()
        led.off()
        button.wait_for_release()
    ```

1. Run the code and you'll see the LED blink on and off until the button is pressed, at which point it will turn off completely. When the button is released, it will start blinking again.

1. Try adding some parameters to `blink` to make it blink faster or slower:

    - `led.blink(2, 2)` - 2 seconds on, 2 seconds off
    - `led.blink(0.5, 0.5)` - half a second on, half a second off
    - `led.blink(0.1, 0.2)` - one tenth of a second on, one fifth of a second off

    `blink`'s first two (optional) parameters are `on_time` and `off_time`': they both default to 1 second.

## Traffic lights

You have three LEDs: red, amber, and green. Perfect for traffic lights! There's even a built-in interface for traffic lights in GPIO Zero.

1. Amend the `from gpiozero import...` line to replace `LED` with `TrafficLights`:

    ```python
    from gpiozero import Button, TrafficLights
    ```

1. Replace your `led = LED(25)` line with the following:

    ```python
    lights = TrafficLights(25, 8, 7)
    ```

    The `TrafficLights` interface takes three GPIO pin numbers, one for each pin: red, amber, and green (in that order).

1. Now amend your `while` loop to control the `TrafficLights` object:

    ```python
    while True:
        button.wait_for_press()
        lights.on()
        button.wait_for_release()
        lights.off()
    ```

    The `TrafficLights` interface is very similar to that of an individual LED: you can use `on`, `off`, and `blink`, all of which control all three lights at once.

1. Try the `blink` example:

    ```python
    while True:
        lights.blink()
        button.wait_for_press()
        lights.off()
        button.wait_for_release()
    ```

## Add a buzzer

Now you'll add your buzzer to make some noise.

1. Add `Buzzer` to the `from gpiozero import...` line:

    ```python
    from gpiozero import Button, TrafficLights, Buzzer
    ```

1. Add a line below your creation of `button` and `lights` to add a `Buzzer` object:

    ```python
    buzzer = Buzzer(15)
    ```

1. `Buzzer` works exactly like `LED`, so try adding a `buzzer.on()` and `buzzer.off()` into your loop:

    ```python
    while True:
        lights.on()
        buzzer.off()
        button.wait_for_press()
        lights.off()
        buzzer.on()
        button.wait_for_release()
    ```

1. `Buzzer` has a `beep()` method which works like `LED`'s `blink`. Try it out:

    ```python
    while True:
        lights.blink()
        buzzer.beep()
        button.wait_for_press()
        lights.off()
        buzzer.off()
        button.wait_for_release()
    ```

## Traffic lights sequence

As well as controlling the whole set of lights together, you can also control each LED individually. With traffic light LEDs, a button and a buzzer, you can create your own traffic lights sequence, complete with pedestrian crossing!

1. At the top of your file, below `from gpiozero import...`, add a line to import the `sleep` function:

    ```python
    from time import sleep
    ```

1. Modify your loop to perform an automated sequence of LEDs being lit:

    ```python
    while True:
        lights.green.on()
        sleep(1)
        lights.amber.on()
        sleep(1)
        lights.red.on()
        sleep(1)
        lights.off()
    ```

1. Add a `wait_for_press` so that pressing the button initiates the sequence:

    ```python
    while True:
        button.wait_for_press()
        lights.green.on()
        sleep(1)
        lights.amber.on()
        sleep(1)
        lights.red.on()
        sleep(1)
        lights.off()
    ```

    Try some more sequences of your own.

1. Now try creating the full traffic lights sequence:

    - Green on
    - Amber on
    - Red on
    - Red and amber on
    - Green on

    Be sure to turn the correct lights on and off at the right time, and make sure you use `sleep` to time the sequence perfectly.

1. Try adding the button for a pedestrian crossing. The button should move the lights to red (not immediately), and give the pedestrians time to cross before moving back to green until the button is pressed again.

1. Now try adding a buzzer to beep quickly to indicate that it is safe to cross, for the benefit of visually impaired pedestrians.

## What next?

- Try adding a second button for the other side of the road. You'll probably need to use GPIO Zero `button.when_pressed` rather than `wait_for_press`, which can only be used for one button at a time.
- Refer to the documentation at [gpiozero.readthedocs.org](https://gpiozero.readthedocs.org/) for more information on what can be done with GPIO Zero.
- Continue to the next worksheet on using a [Light Dependent Resitor](ldr.md)
