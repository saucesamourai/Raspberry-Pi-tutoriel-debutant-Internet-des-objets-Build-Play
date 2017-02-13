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

To use the ultrasonic distance sensor in Python, you need to know which GPIO pins the echo and trigger are connected to.

1. Open Python 3.

1. In the shell, enter the following line to import `DistanceSensor` from the GPIO Zero library:

    ```python
    from gpiozero import DistanceSensor
    ```

    After each line, press **Enter** and the command will be executed immediately.

1. Create an instance of `DistanceSensor` using your echo and trigger pins:

    ```python
    ultrasonic = DistanceSensor(echo=17, trigger=4)
    ```

1. See what distance it shows:

    ```python
    ultrasonic.distance
    ```

    You should see a number: this is the distance to the nearest object, in metres.

1. Try using a loop to print the distance continuously, while waving your hand in front of the sensor to alter the distance reading:

    ```python
    while True:
        print(ultrasonic.distance)
    ```

    The value should get smaller the closer your hand is to the sensor. Press **Ctrl + C** to exit the loop.

## Ranges

As well as being able to see the distance value, you can also get the sensor to do things when the object is in or out of a certain range.

1. Use a loop to print different messages when the sensor is in range or out of range:

    ```python
    while True:
        ultrasonic.wait_for_in_range()
        print("In range")
        ultrasonic.wait_for_out_of_range()
        print("Out of range")
    ```

    Now wave your hand in front of the sensor; it should switch between showing the message "In range" and "Out of range" as your hand gets closer and further away from the sensor. See if you can work out the point at which it changes.

1. The default range threshold is 0.3m. This can be configured when the sensor is initiated:

    ```python
    ultrasonic = DistanceSensor(echo=17, trigger=4, threshold_distance=0.5)
    ```

    Alternatively, this can be changed after the sensor is created, by setting the `threshold_distance` property:

    ```python
    ultrasonic.threshold_distance = 0.5
    ```

1. Try the previous loop again and observe the new range threshold.

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
