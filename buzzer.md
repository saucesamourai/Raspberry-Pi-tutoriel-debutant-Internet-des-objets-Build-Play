# Les Buzzers

Il existe deux principales catégories de buzzers: *actifs* et *passifs*.

Un buzzer *passif* émet un son lorsqu'une tension est appliquée à ses bornes. Il nécessite un signal spécifique pour générer une variété de sons. Le buzzer *actif* est beaucoup plus simple à utiliser, c'est donc de lui que nous allons traiter dans ce tutoriel.

## Connecter un buzzer

Un buzzer *actif* se branche comme une LED, mais ils sont plus costauds et vous n'aurez pas besoin d'une résistance pour le protéger.

Préparez votre circuit comme ci-dessous:

![buzzer](images/buzzer-circuit.png)

1. Pour importer la fonction Buzzer dans Python, ajoutez `Buzzer` à la ligne: `from gpiozero import...` :

    ```python
    from gpiozero import Buzzer
	from time import sleep
    ```

1. Ajoutez la ligne ci-dessous pour ajouter un objet `Buzzer` :

    ```python
    buzzer = Buzzer(17)
    ```

1. Dans GPIO Zero, un `Buzzer` fonctionne exactement comme une 'LED', donc essayez d'ajouter une boucle contenant `buzzer.on()` et `buzzer.off()` :

    ```python
    while True:
        buzzer.on()
	    sleep(1)
        buzzer.off()
		sleep(1)

    ```

1. Le `Buzzer` dispose aussi d'une méthode `beep()` qui fonctionne comme la méthode `blink` d'une 'LED, c'est à dire qui permet de la faire clignoter. Essayez:

    ```python
    while True:
        buzzer.beep()
    ```

## Et ensuite?

- Continuez au prochain tutoriel pour fabriquer votre propre [feu rouge](trafficlights.md) en utilisant GPIO Zero.
