title: Introducción a NodeBots
date: 2015-07-23 09:04:19
tags:
	- Arduino	
	- node.js
	- johnny-five.js
  - NodeBots
category:
  - tutoriales
author: Carlos Cárcamo
author_url: 'http://carloscarcamo.me'
---

En esta guía aprenderemos a usar Node.js para comunicarnos con Arduino usando [johnny-five](http://johnny-five.io/) un Framework JavaScript para robótica. A continuación una lista de requerimientos para poder seguir este tutorial:

- Node.js y NPM instalados y corriendo en nuestro ordenador, [(Ver como instalar Node)](http://nodebots-sv.github.io/2015/07/08/Instalar-Node-js/)
- [Arduino IDE](https://www.arduino.cc/en/main/software)
- Placa [Arduino UNO](https://www.arduino.cc/en/Main/ArduinoBoardUno) (o compatibles)
- 1 o más Leds


## Preparando el Arduino

Node.js no puede comunicarse con arduino por si solo pues arduino por defecto se programa en lenguaje C, pero existe otra manera de controlar Arduino desde un software host en el ordenador sin necesidad de programar en el lenguaje nativo de la placa. Esto es posible a través de la librería [Firmata](https://www.arduino.cc/en/reference/firmata). Firmata es un firmware que establece un protocolo de comunicación entre un programa en el ordenador y el Arduino.</p>

<!-- more -->

Dicho lo anterior, necesitamos instalar el firmware Firmata en el Arduino, esto es muy simple, el Arduino IDE trae una serie de ejemplos listos para ser cargados en la placa Arduino, entre estos ejemplos tenemos el “StandardFirmata” el cual usaremos en este turorial. Para cargar firmata al arduino seguimos los siguientes pasos:

- En el menu seleccionar: **File > Examples > Firmata -> StandardFirmata**
	
	![Firmata Arduino IDE](http://i.imgur.com/adsOpsI.jpg?1)

- Cargar firmata a la placa arduino: <br>	
	Hacer click en el botón “Upload”, una vez cargado el firmware puedes cerrar el Arduino IDE.
	
	![Cargar firmata](http://i.imgur.com/mIR0MhW.png?1)

**Solucionando problemas**<br>
En GNU/Linux es posible que el Arduino IDE tenga deshabilitado la opción para seleccionar el puerto serial y por lo tanto no poder manipular la placa Arduino conectada al ordenador. La siguiente imagen ilustra este problema:

![No serialport](http://i.imgur.com/Ee5wwt4.png?1)

Para solucionar este problema ejecuta lo siguiente en una terminal:

```
$ sudo usermod -a -G dialout <usuario>
$ sudo usermod -a -G tty <usuario>
```

Cierra e inicia sesión con tu usuario, el problema deberá estar solucionado!

## ‘Hola Mundo!’ con Arduino + Node.js

Vamos a construir el siguiente ejemplo:

![Hola mundo, fuente: http://johnny-five.io"](http://i.imgur.com/VyJh3Ih.gif)

Para comenzar vamos crear una carpeta e instalar el modulo [johnny-five](https://github.com/rwaldron/johnny-five) para comunicarnos con Arduino. Sigue los pasos a continuación:

```
$ mkdir nodebots && cd $_
$ npm install johnny-five
```

Dentro de la carpeta nodebots, crea un archivo llamado **hello-world.js** y agrega el siguiente código:

```javascript
var five = require(“johnny-five”),
  board = new five.Board();
  // usar este codigo en caso de que johnny-five no reconozca ningun dispositivo
  //board = new five.Board({port: “/dev/ttyACM0”});
board.on(“ready”, function() {
  //crear un led en el pin 13
  var led = new five.Led(13);
  //parpadear cada segundo
  led.blink(1000);
});
```

Conecta tu arduino a una fuente de energía y ejecuta lo siguiente:

```
$ node hello-world.js
```

![Ese momento en que creas un circuito en menos de 5 minutos!](http://i.imgur.com/0erQFLu.gif)

El led comenzará a parpadear además en la terminal veras unos logs y la consola se volverá interactiva a la espera que teclees algun comando. Procedemos a detener el programa escribiendo **.exit**.

Felicidades!!! acabas de crear tu primer script con Node.js capaz de comunicarse con tu Arduino :)

### Recapitulemos lo que hemos hecho:

Primero, hemos incluido el modulo johnny-five, con la palabra reservada **require**, que nos permite interactuar con arduino:

```javascript
var five = require(‘johnny-five’)
```

Luego instanciamos la clase constructora **five.Board** que es parte del API de johnny-five, ver [Board API](http://johnny-five.io/api/board/)

```javascript
var board = new five.Board();
```

Luego, agregamos un [listener](https://nodejs.org/api/events.html#events_class_events_eventemitter) al evento ‘ready’, el listener es una función [callback](http://fernetjs.com/2011/12/creando-y-utilizando-callbacks/) que se ejecuta cuando el evento ready sea emitido, esto será luego del evento ‘connect’ y cuando la instancia del objecto Board haya completado la inicialización del hardware.

```javascript
board.on(“ready”, function() {
  …
});
```

Dentro del callback, creamos un objeto que representa un led usando la clase constructora **five.Led**, ver [Led API](http://johnny-five.io/api/led/)

```
var led = new five.Led(13);
```

Por último ejecutamos la función **blink** que hace parpadear nuestro led en el pin 13. A la función le pasamos como parámetro el intervalo de tiempo de parpadeo (milisegundos)

```
led.blink(1000);
```

Listo! hemos creado nuestro primer script usando Node.js y Arduino, ¿verdad que fue sencillo?<br>
Compartimos acá el código fuente de este ejemplo que esta en nuestra pagina de github + un par de ejemplos básicos para que puedas probar en tu casa:

[https://github.com/nodebots-sv/ejemplos-basicos](https://github.com/nodebots-sv/ejemplos-basicos)

Si deseas estudiar más ejemplos, ve a la pagina oficial del proyecto [johnny-five](http://johnny-five.io/) ahí encontraras mucha más información.

Si te gusto este sencillo tutorial, comparte este post y deja tus comentarios abajo, cualquier duda escríbenos.

**¿Quieres unirte a la comunidad?**<br>
Encantados, síguenos en twitter [@NodebotsSV](https://twitter.com/NodebotsSV), escríbenos a <a href="mailto:nodebots.sv@gmail.com" target="_blank" rel="external">nodebots.sv@gmail.com</a> y con gusto te responderemos.