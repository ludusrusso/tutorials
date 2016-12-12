# Introduzione alla libreria gpiozero

Ho da pochissimo (ieri nel momento in cui scrivo questo post) scoperto un'interessantissima libreria chiama [gpiozero](https://gpiozero.readthedocs.io/en/v1.3.1/) che permette di utilizzare i GPIO di un raspberry pi in modo semplice ed intuitivo. Dopo una giornata di utilizzo sono riuscito ad integrare il mio *dotbot* con un sensore di distanza, a controllare i motori con pochissime linee di codice ed ad accedere agli input di bottoni tramite interrupt.

Qui aggiungerò piano piano delle guide passo passo per controllare vari tipi di sensori e attuatori direttamente in questa libreria. Ovviamente seguiranno post simili per l'utilizzo della libreria in ROS.

## Installazione
Due modi alternativi per installare *gpiozero*. Ovviamente io utilizzerò la versione Python 2.7.

### Metodo 1: apt-get
~~~
$ sudo apt-get install python-gpiozero
~~~

### Metodo 2: pip dentro un Virtualenv
Per chi non sa cosa è un virtualenv, a breve scriverò un post. Per le altre persone, si può installare con il seguente comando:

~~~
$ pip install RPi.GPIO gpiozero
~~~

PS: ricordatevi di installare, da pip, anche la libreria *RPi.GPIO*. Altrimenti alcune funzionalità (legate all'utilizzo dei PWM) non funzioneranno.


## Controllo di un Led
Partiamo da cose semplici e controlliamo un Led connesso ad un Pin del raspberry Pi. 

Colleghiamo un led e una resistenza di valore consigliato 270Ω (ohm) come in figura.

![alt text][led-img]

Notare che l'anodo (+) del led è collegato al pin GPIO17 tramite la resistenza, mentre il catodo (-) è collegato a GND.

Siamo quindi pronti a scrivere del codice. Per gestire un led, *gpiozero* mette a disposizione l'oggetto `LED`, che deve essere importato nel programma

```python
from gpiozero import LED
```
e inizializzato passandogli come argomento il pin a cui è collegato.
```python
led = LED(17)
```

A questo punto, possiamo utilizzare la variabile `led` per accendere o spegnere il led. Con `led.on()` accenderemo il led, mentre con `led.off()` spegneremo il led.

Possiamo ora scrivere un programma per fare il classico _blink_ di un led. Utilizzaerò la liberia `time`, ed in particolare la funzione `sleep`, per regolare la frequenza di accensione e spegnimento.

```python
from gpiozero import LED 	#importo il LED
from time import sleep		#importo sleep

led = LED(17) 				#inizializzo LED al GPIO17

while True:					#ciclo infinito
    led.on()				#accendi il led
    sleep(1)				#aspetta 1 secondo
    led.off()				#spegni il led
    sleep(1)				#aspetta 1 secondo
```

Vorrei anche segnalare, che la libreria mette a disposizione un interessante metodo di led, chiamato `toggle`, che cambia lo stato del led. In altre parole, chiamando la funzione, se il led è acceso si spegnerà, se è spento si accenderà.

Possiamo riscrivere il programma quindi nel seguente modo

```python
from gpiozero import LED 	#importo il LED
from time import sleep		#importo sleep

led = LED(17) 				#inizializzo LED al GPIO17

while True:					#ciclo infinito
    led.toggle()			#cambia stato
    sleep(1)				#aspetta 1 secondo
```




[led-img]: ./led.png "Logo Title Text 2"
