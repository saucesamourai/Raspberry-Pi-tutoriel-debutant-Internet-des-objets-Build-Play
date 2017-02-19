# Tutorial : premiers pas dans l'univers de l'Internet des Objets

## Les broches GPIO 

La broche GPIO (General-Purpose Input/Output, ou en français Entrées / Sorties) constitue l’un des principaux éléments du Raspberry Pi. Interface entre le Raspberry Pi et le monde extérieur, cette broche permet de prendre contrôler les actionneurs connectés au Raspberry Pi et de collecter les informations que vous souhaitez récupérer. A titre d’exemple, le Raspberry Pi peut allumer des LEDs, les éteindre, détecter si un interrupteur a été pressé, de mesurer la température, et la liste est longue! Cela s’appelle du Physical Computing!
Le Raspberry compte 40 entrées/sorties delivrant de nombreuses fonctionnalités.
Si vous disposez d’un <a rel="nofollow" href="https://www.amazon.fr/gp/product/B01DRO5QHC/ref=as_li_tl?ie=UTF8&camp=1642&creative=6746&creativeASIN=B01DRO5QHC&linkCode=as2&tag=curioschron01-21">"RaspIO Label"</a><img src="http://ir-fr.amazon-adsystem.com/e/ir?t=curioschron01-21&l=as2&o=8&a=B01DRO5QHC" width="1" height="1" border="0" alt="" style="border:none !important; margin:0px !important;" />
 (étiquette avec le nom de chaque entrées/sorties), cela peut vous permettre d’identifier plus rapidement les différents entrées/sorties et d’éviter de les confondre dans le futur. Lorsque vous installez votre étiquette, veillez à bien placer le trou jaune face aux ports USB, pointant vers l'extérieur.

![](images/raspio-ports.jpg)

Si vous n’avez pas d’étiquette, ne vous inquiétez pas. Ce schéma peut vous aider à vous y retrouver et à les identifier correctement.

Vous pouvez remarquer différents types de label: 3V3, 5V, GND, GP2, etc. Vous trouvez dans la table ci-dessous leur description respectives:

![](images/pinout.png)

You'll see pins labelled as 3V3, 5V, GND and GP2, GP3, etc:

|   |   |   |
|---|---|---|
| 3V3 | 3.3 volts | Tout élément connecté à cette broche recevra une tension de 3,3V|
| 5V | 5 volts | Tout élément connecté à cette broche recevra une tension de 5V |
| GND | ground | Tension nulle permettant de boucler un circuit |
| GP2 | GPIO pin 2 | Ces broches sont les entrées/sorties classiques, configurables et contrôlable par le Raspberry Pi |
| ID_SC/ID_SD/DNC | Broches spéciales ||

**ATTENTION**: en suivant les instructions de ce tutorial, vous limiterez les risques d’endommagement de votre Raspberry. Veillez à ne pas brancher au hasard des éléments sur des broches sans avoir vérifié au préalable leur compatibilité. En particulier, les broches 5V sont dangereuses pour vos composants. Inversement, il faut prendre des précautions lorsque vous connectez des composants de forte puissance sur le Raspberry Pi. Les LED ne posent pas de problèmes, mais les moteurs peuvent endommager votre carte.


## Allumer une LED

Les LED sont des objets fragiles. Il faut faire attention et ne pas laisser un courant trop élevé les traverser - sous peine de voire la LED explosée sous vos yeux. Afin de limiter l’injection de courant, il faudra utiliser une résistance en série.
Connectez une patte de la LED à la sortie 3,3V via une résistance de plus de 50Ω et l’autre à la sortie GND. Si vous avez acheté le [kit d'apprentissage Build & Play](https://www.amazon.fr/dp/B01N0TKCJN)
, vous pouvez choisir n'importe laquelle des LED avec n'importe laquelle des résistances de 100Ω fournies.

![](images/led-3v3.png)

La LED devrait s’allumer et rester dans cet état, étant connectée à une tension constante de 3.3V


![](images/led-gpio17.png)

La LED devrait s’éteindre. Elle est maintenant connectée à une GPIO et peut donc être contrôlée depuis votre Raspberry Pi.

## Allumer et éteindre une LED depuis le Raspberry Pi

GPIO Zero est une nouvelle librairie Python permettant de dialoguer avec les GPIOs du Raspberry Pi. Cette librairie est installée par défaut avec Raspbian. 

1. Ouvrer votre IDLE du menu principal (Menu > Programming > Python 3 (IDLE)).


![Naviguez dans le menu pour démarrer Python 3](images/python3-app-menu.png)

1. Vous pouvez maintenant contrôler votre LED en tapant des commandes directement depuis votre interpréteur. Dans un premier temps, importons la librairie. Il va être aussi nécessaire de déclarer à votre Raspberry Pi quelle GPIO vous souhaitez piloter. En loccurence, c'est la broche 17 qui nous intéresse. À côté des chevrons `>>>`, tapez :

	```python
	from gpiozero import LED
	led = LED(17)

	```
	Appuyez sur **Entrée** sur votre clavier pour exécuter la ligne de commande.

1. Pour allumer la LED, taper le code suivant et appuyer sur **Entrée**:

	```python
	led.on()
	```

1. Afin de l'éteindre, vous pouvez taper :

	```python
	led.off()
	```

1. Votre LED devrait s'allumer puis s'étindre. Mais c'est loin d'être fini!

## Faire clignoter une LED

A l'aide de la librairie `time` et d'une petite boucle, vous allez faire scintiller votre LED.	

1. Créez un nouveau fichier en cliquant sur **Fichier > Nouveau**.

1. Enregistrez votre fichier en cliquant sur **Fichier > Enregistrer **. Enregistrer le fichier sous le nom `gpio_led.py`.

1. Entrez ce code pour commencer:

    ```python
    from gpiozero import LED
    from time import sleep

    led = LED(17)

    while True:
        led.on()
        sleep(1)
        led.off()
        sleep(1)
    ```

1. Enregistrer tout en faisant **Ctrl + S** et exécutez le code en pressant la touche **F5**.

1. La LED devrait maintenant clignoter. Pour quitter le programme, pressez **Ctrl + C** sur votre clavier.

## Utiliser un bouton poussoir en entrée

Maintenant que vous êtes capable de contrôler une LED, essayons de connecter et contrôler un bouton poussoir.
Créer un nouveau fichier en cliquant sur File > New file.
Sauvegarder le fichier en cliquant sur File > Save. Sauvegarder le fichier en tant que gpio_button.py.
Cette fois, nous aurons besoin de la classe Button et de déclarer que nous allons contrôler la broche 2.
Taper le code suivant dans votre fichier:
from gpiozero import Button
button = Button(2)
Maintenant vous pouvez déclencher une action lorsque le bouton est pressé:
button.wait_for_press()
print('You pushed me')
Sauvegarder en appuyant sur Ctrl + S et compiler votre code en tapant sur F5
Appuyer sur le bouton et votre texte apparaîtra.



1. Connectez un bouton à n'importe quelle autre broche GND et à la broche GPIO 2, comme ci-dessous: 

    ![](images/button.png)

1. Créez un nouveau fichier en cliquant sur **File > New file**.

1. Sauvergardez le fichier en cliquant sur **File > Save**. Enregistrer le fichier sous le nom `gpio_bouton.py`.

1. This time you'll need the `Button` class, and to tell it that the button is on pin 2. Write the following code in your new file:

	```python
	from gpiozero import Button
	button = Button(2)
	```

1. Now you can get your program to do something when the button is pushed. Add these lines:

	```python
	button.wait_for_press()
	print('You pushed me')
	```
1. Save with **Ctrl + S** and run the code with **F5**. 
1. Press the button and your text will appear. 

## Contrôler une LED manuellement avec un bouton poussoir

Vous pouvez maintenant combiner les deux programmes.

You can now combine your two programs written so far to control the LED using the button.

1. Create a new file by clicking **File > New file**.

1. Save the new file by clicking **File > Save**. Save the file as `gpio_control.py`.

1. Now write the following code:

    ```python
    from gpiozero import LED, Button
    from time import sleep
    
    led = LED(17)
    button = Button(2)
    
    button.wait_for_press()
    led.on()
    sleep(3)
    led.off()
    ```
	
1. Save and run your program. When you push the button the LED should come on for three seconds.

## Making a switch

With a switch, a single press and release on the button would turn the LED on, and another press and release would turn it off again.

1. Modify your code so that it looks like this:

	```python
	from gpiozero import LED, Button
	from time import sleep

	led = LED(17)
	button = Button(2)

	while True:
		button.wait_for_press()
		led.toggle()
	```

    `led.toggle()` switches the state of the LED from on to off, or off to on. Since this happens in a loop the LED with turn on and off each time the button is pressed.

1. It would be great if you could make the LED switch on only when the button is being held down. With GPIO Zero, that's easy. There are two methods of the `Button` class called `when_pressed` and `when_released`. These don't block the flow of the program, so if they are placed in a loop, the program will continue to cycle indefinitely.

1. Modify your code to look like this:

    ```python
    from gpiozero import LED, Button
    from signal import pause

    led = LED(17)
    button = Button(2)

    button.when_pressed = led.on
    button.when_released = led.off

    pause()
    ```

1. Save and run the program. Now when the button is pressed, the LED will light up. It will turn off again when the button is released.

## What next?

There are lots of other things you can control or monitor with your Raspberry Pi. Have a look at the worksheets below, to see how easily this can be done.

- [Using an active buzzer](buzzer.md)  
- [Making traffic lights](trafficlights.md)  
- [Using a light-dependent resistor](ldr.md)  
- [Using a PIR Sensor](pir.md)  
- [Using an ultrasonic distance sensor](distance.md)
- [Analogue inputs](analogue.md)
- [Using motors](motors.md)
