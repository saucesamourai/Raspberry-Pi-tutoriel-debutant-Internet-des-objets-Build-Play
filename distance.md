# Mesurer des distances avec le module à ultrasons HC-SR04 

Dans l'air, le son voyage à une vitesse de 343 mètres par seconde. Un capteur de distance envoie des impulsions d'ultrasons inaudibles par l'oreille humaine, et chronomètre le temps écoulé avant de recevoir l'écho généré par le rebond de l'onde sonore sur un objet proche. Il utilise ensuite la vitesse du son pour en déduire la distance de l'objet.

![Ultrasonic distance sensor](images/ultrasonic-distance-sensor.png)

## Câblage

Le module HC-SR04 dispose de 4 broches. 
* Deux broches servent à son alimentation électrique.
 * La première broche, Vcc va être connecté au 5V.
 * La seconde broche, GND va être connectée à la masse (ground).
* Les deux autres broches servent à transmettre de l'information entre le Raspberry Pi et le module.
 * La broche Trigger va permettre d'ordonner au module d'envoyer une impulsion à la demande du Raspberry Pi.
 * La broche Echo va renvoyer l'information au Raspberry Pi que l'écho a été entendu par le module.

**Attention**, le module produit en sortie de sa broche Echo une tension de 5V. Cette tension est trop élevée pour les entrées GPIO du Raspberry Pi et pourrait l'endommager. Il est donc nécessaire d'abaisser cette tension à environ 3.3V au moyen d'un pont diviseur de tension. 

Pour ce faire, il existe plein de possibilités, nous vous en proposons deux :
* Utiliser une paire de résitances de 330Ω et 470Ω. Le schéma de câblage est représenté ci-dessous.
* Utiliser trois résistances de 100Ω. C'est celle que vous préferez si vous utilisez le kit Build & Play. Dans ce cas, vous adapterez le schéma représenté ci-dessous. Pour obtenir la tension désirée de 3.3V, il faut deux résistances de 100Ω connectées en série entre le port GPIO et la masse, et une résistance de 100Ω connectée entre la borne Echo et le GPIO.


![wiring](images/wiring-uds.png)

## Code

Pour utiliser le module à ultrasons dans Python, vous avez besoin de savoir sur quelles broches du GPIO les connecteurs Echo et Trigger sont branchés.

1. Ouvrez Python 3. 

![Naviguez dans le menu pour démarrer Python 3](images/python3-app-menu.png)

1. Dans la console, tapez la ligne suivante pour importer la fonction `DistanceSensor` de la librairie GPIO Zero:

    ```python
    from gpiozero import DistanceSensor
    ```

    Après chaque ligne de code, tapez **Entrée** et la ligne de commande sera exécutée.

1. Lancez une première impulsion d'ultrasons en créant une instance de `DistanceSensor` avec des valeurs pour echo et trigger:

    ```python
    ultrasonic = DistanceSensor(echo=17, trigger=4)
    ```

1. Observez la distance mesurée par le capteur:

    ```python
    ultrasonic.distance
    ```

    Vous devriez voir un nombre. C'est la distance en mètres de l'objet le plus proche du capteur.
    
1. Nous allons maintenant utiliser une boucle infinie pour relancer des impulsions régulièrement. Passez votre main devant le capteur pour voir la distance mesurée évoluer:

    ```python
    while True:
        print(ultrasonic.distance)
    ```

    La valeur devrait diminuer quand votre main s'approche. Pressez les touche **Ctrl + C** pour sortir de la boucle.

## Portée et intervalles

En plus des mesures de distance, il est possible de programmer votre capteur pour réagir lorsqu'un objet entre à portée.

1. Utilisons une boucle While qui va afficher "Objet à portée" lorsqu'un objet passe suffisamment près, et "Objet hors de portée" lorsqu'il s'éloigne au delà de la zone détectable:
    
    ```python
    while True:
        ultrasonic.wait_for_in_range()
        print("Objet a portee")
        ultrasonic.wait_for_out_of_range()
        print("Objet hors de portee")
    ```
    Déplacez votre main devant le capteur. Il devrait alterner entre les messages "Objet à portée" et "Objet hors de portée" lorsque vous approchez et éloignez votre main. Amusez vous à trouver le point limite entre les deux zones.
    
1. La limite de portée par défaut est de 30cm, ou 0.3m. Celle valeur peut être modifiée au démarrage du capteur:

    Ici par exemple, on va indiquer une portée modifiée de 0.5m.

    ```python
    ultrasonic = DistanceSensor(echo=17, trigger=4, threshold_distance=0.5)
    ```
    
    Autrement, cette valeur peut être changée après la création du capteur, en utilisant l'attribut `threshold_distance`, de l'objet 'ultrasonic':
   
    ```python
    ultrasonic.threshold_distance = 0.5
    ```

1. Essayez la boucle précédente et observez le nouveau seuil de portée défini.

1. The `wait_for` functions are **blocking**, which means they halt the program until they are triggered. Another way of doing something when the sensor goes in and out of range is to use `when` properties, which can be used to trigger actions in the background while other things are happening in the code.

    First, you need to create a function for what you want to happen when the sensor is in range:

    ```python
    def hello():
        print("Hello")
    ```

    Then set `ultrasonic.when_in_range` to the name of this function:

    ```python
    ultrasonic.when_in_range = hello
    ```

1. Add another function for when the sensor goes out of range:

    ```python
    def bye():
        print("Bye")

    ultrasonic.when_out_of_range = bye
    ```

    Now these triggers are set up, you'll see "hello" printed when your hand is in range, and "bye" when it's out of range.

1. You may have noticed that the sensor distance stopped at 1 metre. This is the default maximum and can also be configured on setup:

    ```python
    ultrasonic = DistanceSensor(echo=17, trigger=4, max_distance=2)
    ```

    Or after setup:

    ```python
    ultrasonic.max_distance = 2
    ```

1. Try different values of `max_distance` and `threshold_distance`.

## What next?

Now you've learned to use an ultrasonic distance sensor, you could:

- Build a proximity sensor alert using a buzzer or a sound file
- Build a Pi camera photo booth which is activated when a person is close enough to the camera
- Build a robot with a distance sensor to stop it bumping into other objects
- Continue to the next worksheet on [analogue inputs](analogue.md)
