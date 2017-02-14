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

1. Les fonctions de type 'wait_for' que nous allons voir maintenant (qu'on pourrait traduire 'attendre_que', en français) sont **bloquantes**, ce qui signifie qu'elles arrêtent le programme tant qu'elles ne sont pas déclenchées. Une autre façon de déclencher une action quand l'objet entre et sort de la portée du capteur est d'utiliser l'attribut 'when'. Ceci permet de déclencher automatiquement des actions en arrière plan, même lorsque le programme exécute d'autres lignes de code.


    Par exemple, vous allez creér une une fonction qui définit ce qui se passe quand un objet est à portée du capteur:
    
    ```python
    def hello():
        print("Hello")
    ```
    Dans cette exemple, on affiche 'Hello'.
    
    Ensuite, on configure `ultrasonic.when_in_range` pour déclencher cette fonction.
    
    ```python
    ultrasonic.when_in_range = hello
    ```

1. On ajoute une autre fonction qui définit ce qui se passe quand l'objet passe hors de portée:

    ```python
    def bye():
        print("Bye")

    ultrasonic.when_out_of_range = bye
    ```
    Ici, on dit 'Bye'.
    
    Maintenant que ces déclenchements sont paramétrés, vous devriez voir "Hello" quand votre main est à portée, puis "Bye", quand votre main s'éloigne.
    
    
1. Vous avez peut être remarqué que la distance mesurée plafonne à 1 mètre. Il s'agit de la valeur maximum définie par défaut, mais celle-ci peut être modifiée...


    grâce au paramètre 'max_distance' de la fonction DistanceSensor:


    ```python
    ultrasonic = DistanceSensor(echo=17, trigger=4, max_distance=2)
    ```

    où après initialisation de cette manière:

    ```python
    ultrasonic.max_distance = 2
    ```

1. Amusez vous à essayer différentes valeurs de `max_distance` (distance maximum) et `threshold_distance` (seuil de portée).

## Que faire ensuite ?

Maintenant que vous savez utiliser un module de mesure de distance, vous pourriez :

- Fabriquer un radar de recul avec un buzzer physique ou un fichier son  
- Fabriquer un photomaton qui se déclenche automatiquement quand la personne est suffisamment proche de l'objectif
- Construire un robot équipé d'un capteur de distance qui l'emêche de buter contre les murs et les objets

