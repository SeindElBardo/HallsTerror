#Lenguaje

Bueno, lo primero es saber si sabeís programar PREGUNTAS SI ALGUIEN SABER PROGRAMAR, bueno, la charla esta pensada como que noteneis ni idea. Como curiosidad, Arduino usa un lenguaje muy parecido a C, C++, lo bueno es que aunque no tengais soltura programando, hacer pequeñas cosas es relativamente sencillo y además hay mucho código al alcance, por lo que con unas nociones básicas podeis hacer bastante.

##Entorno

Para explicaros esto, lo mejor es que lo vayamos viendo juntos ABRES EL IDE, esto es el ide de arduino, EXPLICA QUE ES IDE, En mi opinión, como editor de texto deja bastante que desear, pero nos permitirá compilar y cargar el programa rápidamente. Podemos abrir el proyecto y tambien unos cuantos ejemplos que nos servirán tanto para aprender como para copiar vilmente.

##Funciones

Bueno, llegados a este punto quiero preguntaros: ¿Que es programar? ¿Y un programa?

Podemos resumir que un programa es una serie de instrucciones que el programador deja indicadas al procesador. En ellas le dice que debe hacer y como.

Vamos a usar la metafora de los gremlins, en el interior de la placa hay un montón de criaturitas que hacen que todo funcione, y yo, como programador, les doi instrucciones.

Vamos a usar los ejemplos para empezar, INSTRUCCION UP/DOWN Los pines del arduino, les digo enciende o no enciende. pin analogico, 
serial y lecturas de sensores.


#Software coche

## cosas que poner
~~~
#include <SoftwareSerial.h>

SoftwareSerial BT1(4,2); // RX, TX recorder que se cruzan

char str[2],i; // Necesitaremos este par de variables para leer la señal bluetooht
~~~

## Definir pines

![](../img/4.png)


~~~
#define leftMotorF 12
#define leftMotorB 13
#define leftMotorSpeed 11
#define rightMotorF 5
#define rightMotorB 6
#define rightMotorSpeed 10
~~~


## Las variables

~~~
int velocidadR; // Las velocidades de cada rueda
int velocidadI;
int estado;	// El estado define si el coche esta detenido (0), en marcha (1) o marcha atras (-1)
int marcha; // La marcha es un modificador (multiplicador) de la velocidad.
~~~

~~~
void setValues(int sv_velocidadI, int sv_velocidadR, int sv_estado, int sv_marcha){
  velocidadI = sv_velocidadI;
  velocidadR = sv_velocidadR;
  estado = sv_estado;
  marcha = sv_marcha;
}
~~~

## setup

~~~
void setup() {                
  // initialize the digital pin as an output.
  pinMode(leftMotorF, OUTPUT);
  pinMode(leftMotorB, OUTPUT);
  pinMode(leftMotorSpeed, OUTPUT);
  pinMode(rightMotorF, OUTPUT);
  pinMode(rightMotorB, OUTPUT);
  pinMode(rightMotorSpeed, OUTPUT);
  Serial.begin(9600);      // open the serial port at 9600 bps:
  Serial.println("Enter AT commands:");
  BT1.begin(9600);
}
~~~

## Arrancar

~~~
void arrancar(){ //Quiza deberia solo permitirse con el coche detenido
  if(estado == 0){
    Serial.println("Arrancando!");
    setValues(102, 102, 1, 2);
  }
  else if(estado == 1){    
    Serial.println(velocidadR);
    Serial.println(velocidadI);
    setValues(51*marcha, 51*marcha, estado, marcha);
  }
  else if(estado == -1){    
    Serial.println(velocidadR);
    Serial.println(velocidadI);
    setValues(0, 0, 0, 0);
  }  
  Serial.println(estado);
  Serial.println(marcha);
}
~~~

## Detener

~~~
/*
  El coche se detiene, poniendo las variables a 0
 */
void detener(){
  Serial.println("Parando!");
  setValues(0, 0, 0, 0);
}

 ~~~

## Girar a la Izquierda

~~~

/*
  Los giros se realizan disminuyendo la velocidad de una de las ruedas, la cantidad se deberia determinar
 en la fase de pruebas (la mitad actualmente).
 */
void girarI(){
  if(estado == -1){
    Serial.println("Girando a la izquierda marcha atras!");
    setValues(70*marcha, 140*marcha, estado, marcha);
  }
  else{    
    Serial.println("Girando a la izquierda!");
    setValues(25*marcha, 51*marcha, estado, marcha);
  }  
}

~~~

## Girar a la Derecha

~~~

void girarD(){
  if(estado == -1){
    Serial.println("Girando a la derecha marcha atras!");
    setValues(140*marcha, 70*marcha, estado, marcha);
  }
  else{
    Serial.println("Girando a la derecha!");
    setValues(51*marcha, 25*marcha, estado, marcha);
  }  
}

~~~

## Marcha Atrás

~~~
/*
  La marcha atras solo tiene la peculariedad de cambiar el estado, el cual se tiene en cuenta a la hora de escribir
 en los pines la señal. Ademas se empieza la marcha atras en la marcha minima.
 Es posible que no deba haber marchas cuando se conduce hacia atras, y que tampoco se deba permitir la marcha
 atras sin detener previamente el coche.
 */
void marchaAtras(){
  if(estado == 1){
    detener();
    Serial.println("Parando para Marcha Atras!");
  }
  else{
    Serial.println("Marcha Atras!");
    setValues(120, 120, -1, 1);
  }  
}

~~~

## Disminuir Marcha

~~~

/*
  Los aumentos y disminuciones de marcha se limitan a modificar controladamente el valor de la variable "marcha".
 */
void disminuirMarcha(){
  if((marcha > 2) && (estado == 1)){
    Serial.println("Disminuyendo marcha!");
    marcha--;
    setValues(51*marcha, 51*marcha, 1, marcha);
  }
  else{
    Serial.println("Marcha minima");
  } 	
}

~~~

## Aumentar Marcha

~~~

void aumentarMarcha(){
  if((marcha < 5) && (estado == 1)){
    Serial.println("Aumentando marcha!");
    marcha++;
    setValues(51*marcha, 51*marcha, 1, marcha);
  }
  else{
    Serial.println("Marcha maxima");
  } 
}

~~~

## control (I)

![](../img/control1.png)

## control (II)

![](../img/control2.png)

## Lectura

Hazte un favor y ve a la aplicación...

## Escribir

~~~
  if (estado == -1){ // MarchaAtras
    digitalWrite(leftMotorF, LOW);
    digitalWrite(leftMotorB, HIGH);
    analogWrite(leftMotorSpeed, velocidadI);
    digitalWrite(rightMotorF, LOW);
    digitalWrite(rightMotorB, HIGH);
    analogWrite(rightMotorSpeed, velocidadR);
  }
  else{ // Avanzando
    digitalWrite(leftMotorF, HIGH);
    digitalWrite(leftMotorB, LOW);
    analogWrite(leftMotorSpeed, velocidadI);
    digitalWrite(rightMotorF, HIGH);
    digitalWrite(rightMotorB, LOW);
    analogWrite(rightMotorSpeed, velocidadR);
  }
~~~

# Agradecimientos y referencias

[circuitdigest.com "Esqueleto del programa"](http://circuitdigest.com/microcontroller-projects/bluetooth-controlled-robot-car-using-arduino)

[Prometec.net "bluetooth"](http://www.prometec.net/bt-hc06/)

[Electronilab.co "Motor Driver"](http://electronilab.co/tutoriales/tutorial-de-uso-driver-dual-l298n-para-motores-dc-y-paso-a-paso-con-arduino/)

[Adolfo Sanz De Diego "Reveal Presentation"](https://github.com/asanzdiego/markdownslides)

# Preguntas

# En producción. No todo fue tan feliz, pero nos reimos mucho.

## Gravedad que es eso
---

> Huele a quemado. —Luis Ángel García

---
---

> Adri: ¡Arranca!

> ...

> Adri: ¿Estás dándole?

> Luis: Sí.

---

---

> Adri: ¡GIRA QUE TE ESCOÑAS!
> Luis: ¡NO GIRA!
> Adri: ¡PUES PÁRALO!
> Luis: ¡HE PERDIDO LA CONEXIÓN!
> Mireya: Eso es porque quemasteis el bluetooth
> Luis: ¡QUE NO HEMOS QUEMADO EL BLUETOOTH!
> Coche: PLOGF-rggggggggggggg

---

---

> Adri: ¡GIRA QUE TE ESCOÑAS!
> Luis: ¡NO GIRA!
> Adri: ¿TIENE CONEXIÓN?
> Luis: ¡SÍ!
> Adri: ¡OTRA VEZ SE HA CORROMPIDO EL BUFFER, REINICIALO!
> Mireya: Eso es porque quemasteis el bluetooth
Adri: ¡QUE ESO NO TIENE NADA QUE VER!
> Coche: PLOGF-rgggggggggggggOsOdioggggggg

---