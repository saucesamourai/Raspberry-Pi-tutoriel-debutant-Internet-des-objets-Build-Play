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

    `button.is_pressed` est une propriété de l'objet 'button', qui donne l'état du bouton (enfoncé ou non) à chaque instant.

1. Maintenant retournez dans la fenêtre de code et modifiez votre boucle 'while' comme suit:

    ```python
    while True:
        if button.is_pressed:
            print("Bonjour")
        else:
            print("Au revoir")
    ```

1. Executez le code à nouveau et vous devriez voir apparaître "Bonjour" lorsque vous pressez le bouton et "Au revoir" lorsque le bouton est relâché.

1. Modifiez à nouveau la boucle:

    ```python
    while True:
        button.wait_for_press()
        print("Enfoncé")
        button.wait_for_release()
        print("Relâché")
    ```

1. Lorsque vous éxecutez le code cette fois-ci, rien ne se passe jusqu'à ce que vous enfonciez le bouton et alors vous verrez s'afficher "Enfoncé". Au moment où vous le relâchez, vous verrez "Relâché". Ceci ce reproduira à chaque fois que vous presserez le bouton, plutôt que de ré-afficher continuellement l'un ou l'autre des message.

## Ajoutons une LED

Maintentant vous allez ajouter une LED dans le code et utiliser GPIO Zero pour permettre au bouton de contrôler l'allumage de la LED. 


1. Dans votre code, modifiez la ligne `from gpiozero import...` tout en haut,  pour aussi importer l'objet `LED`:

    ```python
    from gpiozero import Button, LED
    ```

1. Ajoutez une ligne en dessous de `button = Button(21)` pour créer une instance de l'objet `LED`:

    ```python
    led = LED(25)
    ```

1. Maintenant modifiez votre boucle 'while' pour allumer la LED lorsque le bouton est enfoncé:

    ```python
    while True:
        button.wait_for_press()
        led.on()
        button.wait_for_release()
        led.off()
    ```

1. Executez le code et testez le montage. La LED s'allume quand vous pressez le bouton. Maintenez le enfoncé pour maintenir la LED allumée.

1. Maintenant inversez le lignes `on` et le `off` dans lcode pour inverser la logique:

    ```python
    while True:
        led.on()
        button.wait_for_press()
        led.off()
        button.wait_for_release()
    ```

1. Executez le code et vous devriez voir la LED allumée tant que le bouton n'est pas enfoncé.

1. Maintenant remplacez `led.on()` par `led.blink()`:

    ```python
    while True:
        led.blink()
        button.wait_for_press()
        led.off()
        button.wait_for_release()
    ```

1. Executez le code et vous verrez la LED clignoter tant que le bouton n'est pas enfoncé. Maintenez enfoncé le bouton pour mainteanir la LED éteinte.

1. Essayez d'ajouter les paramètres suivants à 'blink' pour accélerer ou ralentir le clignotement:

    - `led.blink(2, 2)` - 2 secondes on, 2 secondes off
    - `led.blink(0.5, 0.5)` - Une demi seconde on, une demi seconde off
    - `led.blink(0.1, 0.2)` - Un dixème de seconde one, un cinquière de seconde off
    
    Les deux premiers paramètres de `blink` sont `on_time` et `off_time`': ils valent chaqun 1 seconde par défaut.

## Feu rouge

Choisissez trois LED de couleur différentes pour construire votre feu de signalisation.
Il y a une interface déjà toute faite pour les feux rouges dans GPIO Zero, c'est dingue, non ?

1. Modifiez la ligne  `from gpiozero import...` pour remplacer `LED` par `TrafficLights`, l'objet "feu rouge":

    ```python
    from gpiozero import Button, TrafficLights
    ```

1. Remplacez votre ligne `led = LED(25)` comme suit:

    ```python
    lights = TrafficLights(25, 8, 7)
    ```

    L'interface `TrafficLights` utilise trois numéros de broche GPIO, un pour chaque LED: rouge, orange, vert (dans cette ordre)
    
1. Maintenant, modifiez votre boucle  `while` pour contrôler l'objet `TrafficLights`:

    ```python
    while True:
        button.wait_for_press()
        lights.on()
        button.wait_for_release()
        lights.off()
    ```

    L'interface `TrafficLights` est très similaire à celle d'une LED individuelle : vous pouvez utiliser `on`, `off`, et `blink`. En revanche ici, ces fonctions contrôlent toutes les LED en même temps. 

1. Prenez l'exemple de `blink`:

    ```python
    while True:
        lights.blink()
        button.wait_for_press()
        lights.off()
        button.wait_for_release()
    ```

## Ajoutez un buzzer à votre feu rouge

Maintenant, nous allons ajouter du son à l'image avec le buzzer.

1. Ajoutez `Buzzer` à la ligne `from gpiozero import...` :

    ```python
    from gpiozero import Button, TrafficLights, Buzzer
    ```

1. Ajoutez une ligne en dessous de la création des objets `button` et `lights` pour créer un objet `Buzzer`:

    ```python
    buzzer = Buzzer(15)
    ```

1. `Buzzer` fonctionne exactement comme  `LED`, donc essayez d'ajouter  `buzzer.on()` et `buzzer.off()` dans la boucle:

    ```python
    while True:
        lights.on()
        buzzer.off()
        button.wait_for_press()
        lights.off()
        buzzer.on()
        button.wait_for_release()
    ```

1. `Buzzer` a une méthode `beep()` qui est l'équivalent de la méthode 'blink' pour les `LED`. Essayez-ça:

    ```python
    while True:
        lights.blink()
        buzzer.beep()
        button.wait_for_press()
        lights.off()
        buzzer.off()
        button.wait_for_release()
    ```

## La séquence du feu rouge

Maintenant, créons la vraie séquence de fonctionnement du feu rouge, complète, avec la traversée des piétons!

1. En haut du fichier, juste en dessous de la ligne `from gpiozero import...`, ajoutez une ligne pour importer la fonction `sleep` qui permet de temporiser:

    ```python
    from time import sleep
    ```

1. Modifiez la boucle pour jouer la séquence automatique d'allumage des LED une à une:

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

1. Ajoutez un `wait_for_press` qui initiera la séquence:

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


1. Maintenant, essayez de créer d'autres séquences pour vous amuser.

1. Essayez d'ajouter un bouton pour les piétons qui souhaitent traverser. Le bouton devrait faire passer le feu au rouge après une courte attente, et laisser le temps aux piétons de traverser, puis renvoyer le feu au vert.

1. Ensuite, essayez d'ajouter un bruit du buzzer pour indiquer qu'il est possible de traverser en sécurité pour les personnes malvoyantes.


## Que faire ensuite?

- Amusez-vous à ajouter un second bouton, de l'autre côté de la rue. Vous aurez probablement besoin d'utiliser `button.when_pressed` plutôt que `wait_for_press`, qui ne peut être utilisée que pour un seul bouton à la fois.
- Reportez vous à la documentation pour plus d'informations sur GPIO Zerio [gpiozero.readthedocs.org](https://gpiozero.readthedocs.org/).
- Continuez au prochain tutorial sur les [Photo-résistances](ldr.md)
