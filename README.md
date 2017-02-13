# HallsTerror

* [Español](#español)
* [English](#english)

## Español

* [Acerca de](#acerca-de)
  * [¿Qué es esto?](#qué-es-esto)
  * [Componentes](#Componentes)
  * [Referencias y Documentación](#referencias-y-documentación)
  * [Desarrolladores](#desarrolladores)
* [Montaje](#montaje)
* [Manejo](#manejo)
* [Releases notes](#releases-notes-1)
  * [Relese 1.0](#relese-10-1)
  
### Acerca de

#### ¿Qué es esto?

**HallsTerror** es un pequeño coche construido con Arduino y controlado por Bluetooth mediante un aplicación móvil. Es el primer proyecto que desarrollamos con Arduino y el principal objetivo es aprender a usar los distintos componentes.

![](./img/coche.png)

#### Componentes

Para la construcción del coche se ha utilizado:

- Placa Arduino UNO
- Motor Driver
- Servo Motor x 2
- Chasis y ruedas
- Módulo Bluetooth
- Pila 9V

#### Referencias y Documentación

circuitdigest.com <http://circuitdigest.com/microcontroller-projects/bluetooth-controlled-robot-car-using-arduino>
Prometec.net <http://www.prometec.net/bt-hc06/>

Electronilab.co <http://electronilab.co/tutoriales/tutorial-de-uso-driver-dual-l298n-para-motores-dc-y-paso-a-paso-con-arduino/>

#### Desarrolladores

- Adrián Borja Pimentel
- Luis Ángel Garcia Astudillo

### Montaje

Estamos trabajando en ello

### Manejo

El control del coche se realiza mediante la aplicación *Bluetooth Controller* de *FineApps*.

Se debe vincular la app con el módulo Bluetooth del coche y es necesario asignar desde la apps un botón que envié la señal apropiada para cada orden según de definió en el *loop*.

### Relases notes

#### Relese 1.0 Beta

- Funcionamiento básico (a veces se corrompe la señal Bluetooth y es necesario reiniciar la placa)




