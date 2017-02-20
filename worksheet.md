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

1. Créez un nouveau fichier en cliquant sur **Fichier > Nouveau **.

1. Sauvergardez le fichier en cliquant sur **Fichier > Enregistrer **. Enregistrer le fichier sous le nom `gpio_bouton.py`.

1. Ici, nous allons avoir besoin de la classe d'objet 'Button' pour contrôler le bouton. Ensuite, nous devrons déclarer que le bouton est connecté à la broche GPIO 2. Voici le code à taper dans votre nouveau fichier:


	```python
	from gpiozero import Button
	button = Button(2)
	```

1. Maintenance, vous pouvez programmer votre Raspberry à réaliser une action quand le bouton est pressé. Ajotuez ces lignes:

	```python
	button.wait_for_press()
	print('Tu m'as pressé!')
	```
	On va afficher un message à chaque fois que le bouton est activé.
	
1. Enregistrez en pressant **Ctrl + S** et éxecutez le code en tapant **F5**. 
1. Pressez le bouton et observez le message s'afficher. 

## Contrôler une LED manuellement avec un bouton poussoir

Vous pouvez maintenant combiner les deux programmes précedents pour allumer la LED en pressant le bouton.

1. Créez un nouveau fichier en cliquant sur **Fichier > Nouveau **.

1. Sauvergardez le fichier en cliquant sur **Fichier > Enregistrer **. Enregistrer le fichier sous le nom `gpio_controle.py`.

1. Tapez le code suivant:

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
	Ce code signifique que lorsque le bouton est pressé, on allume la LED pendant 3 secondes avant de l'éteindre.
	
1. Enregistrez et executez le programme. Pressez le bouton et observez la LED s'allumer.
 

## Programmer le bouton en mode interrupteur

Maintenant, on veut que le programme réagisse différemment à la pression du bouton.
En pressant une fois, la LED va s'allumer, et en pressant une seconde fois, la LED va s'éteindre.

1. Modifiez votre code de la manière suivante:

	```python
	from gpiozero import LED, Button
	from time import sleep

	led = LED(17)
	button = Button(2)

	while True:
		button.wait_for_press()
		led.toggle()
	```

    La fonction `led.toggle()` bascule l'état de la LED de 'ON' à 'OFF' ou inversement. 
    
1. Maintenant, on veut que la LED ne s'allume que lorsqu'on maintient le bouton enfoncé. C'est très facile avec GPIO Zero. Il existe deux méthodes (fonctions spécifiques) de l'objet 'Button', `when_pressed` et `when_released'. Ces fonctions ne bloquent pas l'exécution du programme, donc si elles sont placées dans une boucle, le programme continuera à tourner à l'infini.

1. Modifiez votre code de la manière suivante:
    ```python
    from gpiozero import LED, Button
    from signal import pause

    led = LED(17)
    button = Button(2)

    button.when_pressed = led.on
    button.when_released = led.off

    pause()
    ```

1. Enregistrez et exécutez le programme. Appuyez sur le bouton et maintenez le enfoncé pour observer la LED s'éclairer.

## Que faire enuite ?

Vous savez maintenant connecter des LED et des boutons à votre Raspberry Pi, et les programmer en Python. 
L'univers des possibilités offertes par le Raspberry Pi est immense. Passez aux exercices suivants pour en découvrir comment utiliser différents types de détecteurs.


- [Utiliser un détecteur de mouvement infrarouge PIR](pir.md)  
- [Utiliser un module de mesure de distance à ultrasons HC-SR04](distance.md)

