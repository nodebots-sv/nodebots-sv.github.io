title: Arduino y Node.js en Archlinux
date: 2015-08-18 15:17:36
tags:
	- Arduino
	- Archlinux
	- node.js
category:
  - tutoriales
author: Giovanni Hurtado
author_url: 'https://twitter.com/xiorone'
---

![Arduino + Archlinux](http://i.imgur.com/MC3DRVF.png)

En este tutorial aprenderemos a trabajar con Arduino y Node.js en Archlinux.

Para comenzar vamos a seguir los siguientes pasos:

1. Abrimos una terminal y escribimos.

	```
	$ yaourt -S arduino
	```

2. Ahora agregamos al usuario a los grupos uucp y lock:

	```
	$ sudo gpasswd -a <user> uucp
	$ sudo gpasswd -a <user> lock
	```

	<!-- more -->

3. Cerramos la sesión del usuario.

4. Conectamos el Arduino según la siguiente imagen.

	![Conectar arduino](http://i.imgur.com/THXhucN.png?1)

5. Ahora ingresamos al IDE del arduino
	![Arduino IDE](http://i.imgur.com/ia8UATr.png?1)

6. Se realizarán los siguientes pasos:
	
	a. Connect your Arduino-compatible microcontroller via USB
	b. Launch Arduino IDE and open the Firmata sketch via the menu: File > Examples > Firmata > StandardFirmata
	c. Select your Arduino board type (e.g. Arduino Uno) via Tools > Board
	d. Select the port for your board via Tools > Serial Port > (the comm port of your Arduino)
	e. Upload the program by selecting File > Upload

7. Ahora instalamos [NodeJS](https://nodejs.org/)

	```
	$ sudo pacman -S nodejs
	```

8. Luego instalamos [NPM](https://www.npmjs.com/)

	```
	$ sudo pacman -S npm
	```

10. Para nuestro primer ejemplo vamos a conectar un LED en el arduino como se muestra a continuación: pata corta en GND y pata larga en pin 13.

	![ejemplo](http://i.imgur.com/VyJh3Ih.gif)


## Primer ejemplo de programación con NodeJS

###Pasos:

1. Crear un directorio llamado Led
	
	```
	$ mkdir led
	```

2. Ingresamos al directorio

	```
	$ cd led
	```
3. Instalamos el modulo johnny-five
	```
	$ npm install johnny-five
	-
	> serialport@1.7.4 install /home/NodeBots/led/node_modules/johnny-five/node_modules/serialport
	> node-pre-gyp install --fallback-to-build

	[serialport] Success: "/home/NodeBots/led/node_modules/johnny-five/node_modules/serialport/build/serialport/v1.7.4/Release/node-v14-linux-x64/serialport.node" is installed v
	ia remote
	johnny-five@0.8.86 node_modules/johnny-five
	├── ease-component@1.0.0
	├── descriptor@0.1.0
	├── color-convert@0.5.3
	├── es6-shim@0.33.0
	├── nanotimer@0.3.1
	├── lodash@3.10.1
	├── temporal@0.4.0
	├── chalk@1.1.0 (supports-color@2.0.0, ansi-styles@2.1.0, escape-string-regexp@1.0.3, strip-ansi@3.0.0, has-ansi@2.0.0)
	├── array-includes@2.0.0 (define-properties@1.1.1, es-abstract@1.2.2)
	├── firmata@0.5.5 (object-assign@1.0.0, browser-serialport@2.0.3, es6-map@0.1.1)
	└── serialport@1.7.4 (bindings@1.2.1, sf@0.1.7, async@0.9.0, nan@1.8.4, debug@2.2.0, optimist@0.6.1)	
	```

4. Ahora escribimos el primer programa NodeJS usando johnny-five llamado led.js con su editor preferido.

	```
	$ vim led.js
	var five = require("johnny-five"),
	 board = new five.Board();
	// usar este código en caso de que johnny-five no reconoce ningun dispositivo
	//board = new five.Board({port: "/dev/ttyACM0"});

	board.on("ready", function(){
	 //crear un led en el pin 13
	 var led = new five.Led(13);

	 //parpadear cada segundo
	 led.blink(1000);
	});
	```
5. Ejecutamos el programa

	```
	$ node led.js  
	1439911736840 Device(s) /dev/ttyACM0   
	1439911736855 Connected /dev/ttyACM0   
	1439911740299 Repl Initialized   
	>>
	```

6. Veremos nuestro led parpadeando cada segundo.
7. Para cancelar el programa hay que digitar .exit o presionar 2 veces control+c.

Listo, ya podemos comenzar a inventar con Arduino y Node.js en Archlinux.

¿Te gusto este tutorial? si tu respuesta es si, comparte este post y deja tus comentarios abajo, cualquier duda escríbenos a nuestro correo <a href="mailto:nodebots.sv@gmail.com">nodebots.sv@gmail.com</a> y siguenos en twitter [@NodebotsSV](https://twitter.com/NodebotsSV).