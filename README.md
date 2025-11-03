# INTERFAZII
### Ejercicio1: Hola mundo! 04/08
```js
void setup() {
  Serial.begin(9600); // Inicia la comunicación serie a 9600 bps
  Serial.println("Hola, quiero dormir"); // Envía "Hola, Mundo!" al monitor serie
}

void loop() {
  // No es necesario poner nada en el loop para este ejemplo
}
```
### Ej n2: LEDs intermitentes 11/08

```js
void setup() {  // Configuración inicial (ej: pines como entrada/salida)
  pinMode(13, OUTPUT);  // Pin 13 como salida
}

void loop() {   // Se repite infinitamente
  digitalWrite(13, HIGH);  // Encender LED
  delay(500);             // Esperar 1 segundo
  digitalWrite(13, LOW);   // Apagar LED
  delay(800);             // Esperar 1 segundo
  
  digitalWrite(8, HIGH);  // Encender LED
  delay(1000);             // Esperar 1 segundo
  digitalWrite(8, LOW);   // Apagar LED
  delay(90);  
}
```
<img src= "https://github.com/smanriquezj/INTERFAZII/blob/main/img/Leds%20parpadeantes.png"/>
<img src= "https://github.com/smanriquezj/INTERFAZII/blob/main/img/led%20parpadeante%20fis.jpeg">

### Ej n3: Control con pulsador
Objetivo: Encender un LED solo al presionar un botón. Circuito: Pulsador en pin 2 (con resistencia pull-down de 10k Ω). LED en pin 13.
```js
void setup() {
  pinMode(2, INPUT);  // Botón como entrada
  pinMode(13, OUTPUT);
}
void loop() {
  if (digitalRead(2) == HIGH) {  // Si se presiona el botón
    digitalWrite(13, HIGH);
  } else {
    digitalWrite(13, LOW);
  }
}
```
<img src= "https://github.com/smanriquezj/INTERFAZII/blob/main/img/led%20con%20boton.png">


### Ej n4: LED con potenciómetro
Objetivo: Regular brillo de un LED con un potenciómetro. Circuito: Potenciómetro: Patas extremas a +5V y GND, central a pin A0. LED en pin 9 (con resistencia 220 Ω).
```js
void setup() {
  pinMode(9, OUTPUT);  // Pin PWM (símbolo ~)
}
void loop() {
  int valor = analogRead(A0);           // Leer potenciómetro (0-1023)
  int brillo = map(valor, 0, 1023, 0, 255);  // Convertir a rango PWM
  analogWrite(9, brillo);               // Ajustar brillo
}
```
<img src= "https://github.com/smanriquezj/INTERFAZII/blob/main/img/led%20con%20potenciometro.png">

### Ej n5: Semáforo 25/08
```js
int LED_1 = 6;  // Luz roja autos
int LED_2 = 7;  // Luz amarilla autos
int LED_3 = 8;  // Luz verde autos
int LED_4 = 9;  // Luz verde peatones
int LED_5 = 10; // Luz roja peatones

void setup() {
  // Configuramos todos los pines como salida
  pinMode(LED_1, OUTPUT);
  pinMode(LED_2, OUTPUT);
  pinMode(LED_3, OUTPUT);
  pinMode(LED_4, OUTPUT);
  pinMode(LED_5, OUTPUT);
}

void loop() {
  // 🚦 Fase 1: Autos en verde, peatones en rojo
  digitalWrite(LED_1, LOW);   // Rojo autos apagado
  digitalWrite(LED_2, LOW);   // Amarillo autos apagado
  digitalWrite(LED_3, HIGH);  // Verde autos encendido
  digitalWrite(LED_4, LOW);   // Verde peatones apagado
  digitalWrite(LED_5, HIGH);  // Rojo peatones encendido
  delay(5000); // 5 segundos

  // 🚦 Fase 2: Amarillo autos, peatones siguen en rojo
  digitalWrite(LED_3, LOW);   // Verde autos apagado
  digitalWrite(LED_2, HIGH);  // Amarillo autos encendido
  delay(2000); // 2 segundos
  digitalWrite(LED_2, LOW);   // Amarillo autos apagado

  // 🚦 Fase 3: Rojo autos, verde peatones
  digitalWrite(LED_1, HIGH);  // Rojo autos encendido
  digitalWrite(LED_5, LOW);   // Rojo peatones apagado
  digitalWrite(LED_4, HIGH);  // Verde peatones encendido
  delay(5000); // 5 segundos

 // 🚦 Fase 4: Rojo autos, rojo peatones (tiempo intermedio)
 //digitalWrite(LED_4, LOW);   // Verde peatones apagado

 //  digitalWrite(LED_5, HIGH);  // Rojo peatones encendido
 // delay(2000); // 2 segundos}
```
<img src= "https://github.com/smanriquezj/INTERFAZII/blob/main/img/semaforo.png">
<img src= "https://github.com/smanriquezj/INTERFAZII/blob/main/img/semaforo%20fis.jpeg">

# Arduino/Processing

### Ej n6: Elipse Interactiva 
#### Código Arduino
```js
unsigned int ADCValue;
void setup() {
  Serial.begin(9600);
  pinMode(9, OUTPUT);  // Pin PWM (símbolo ~)
}
void loop() {
  int val = analogRead(A0);           // Leer potenciómetro (0-1023)
  int brillo = map(val, 0, 1023, 0, 255);  // Convertir a rango PWM
  analogWrite(9, brillo);               // Ajustar brillo
      Serial.println(val);
delay(50);
}
```
<img src= "https://github.com/smanriquezj/INTERFAZII/blob/main/img/procesing%20fis.jpeg">

#### Código Processing
```js
import processing.serial.*;

Serial myPort;  // Crear objeto de la clase Serial
static String val;    // Datos recibidos desde el puerto serial
int sensorVal = 0;

void setup()
{
  background(0); 
  //fullScreen(P3D);
   size(1080, 720);
   noStroke();
  noFill();
  String portName = "COM3";// Cambia el número (en este caso) para que coincida con el puerto correspondiente conectado a tu Arduino. 

  //myPort = new Serial(this, "/dev/cu.usbmodem1101", 9600);
  myPort = new Serial(this, Serial.list()[0], 9600);

}

void draw()
{
  if ( myPort.available() > 0) {  // Si hay datos disponibles,
  val = myPort.readStringUntil('\n'); 
  try {
   sensorVal = Integer.valueOf(val.trim());
  }
  catch(Exception e) {
  ;
  }
  println(sensorVal); // léelos y guárdalos en vals!
  }  
 //background(0);
  // Escala el valor de mouseX de 0 a 640 a un rango entre 0 y 175
  float c = map(sensorVal, 0, width, 0, 400);
  // Escala el valor de mouseX de 0 a 640 a un rango entre 40 y 300
  float d = map(sensorVal, 0, width, 40,500);
  fill(255, c, 0);
  ellipse(width/2, height/2, d, d);   
} 
```
<img src= "https://github.com/smanriquezj/INTERFAZII/blob/main/img/circulo.png">

### Ej n7: Arduino + Botón + Processing

#### Código Arduino
```js
int buttonPin = 2;  // Pin del botón
int buttonState = 0;

void setup() {
  pinMode(buttonPin, INPUT_PULLUP); // Botón con resistencia interna
  Serial.begin(9600);
}

void loop() {
  buttonState = digitalRead(buttonPin);

  if (buttonState == HIGH) {   // Botón presionado
    Serial.println(1);        // Enviar un "1" a Processing
    delay(200);               // Evitar rebotes
  }
}
```

#### Código Processing
```js
import processing.serial.*;

Serial myPort;
ArrayList<PVector> circles; 

void setup() {
  size(1920, 1080);
  background(0);
  
  // Ajusta el nombre del puerto según tu Arduino
  println(Serial.list());
  myPort = new Serial(this, "/dev/cu.usbmodem1101", 9600);
  //myPort = new Serial(this, Serial.list()[0], 9600);
  
  circles = new ArrayList<PVector>();
}

void draw() {
  //background(0);
  
  // Dibujar círculos almacenados
  fill(55, 105, 151);
  //noStroke();
  stroke(25, 200, 78);
  for (PVector c : circles) {
    ellipse(c.x, c.y, 100, 10);
  }
  
  // Revisar si llega algo de Arduino
  if (myPort.available() > 0) {
    String val = myPort.readStringUntil('\n');
    if (val != null) {
      val = trim(val);
      if (val.equals("1")) {
        // Cada vez que se aprieta el botón, agregar un círculo en posición aleatoria
        circles.add(new PVector(random(width), random(height)));
      }
    }
  }
}
```
### Ej n8: Arduino + Botón + Potenciómetro + Processing

#### Código Arduino
```js
int buttonPin = 2;       // Pin del botón
int potPin = A0;         // Pin del potenciómetro
int buttonState = 0;

void setup() {
  pinMode(buttonPin, INPUT_PULLUP); // Botón con resistencia interna
  Serial.begin(9600);
}

void loop() {
  buttonState = digitalRead(buttonPin);

  if (buttonState == HIGH) {   // Botón presionado
    int potValue = analogRead(potPin);   // 0 - 1023
    Serial.print("BTN,");     // etiqueta para Processing
    Serial.println(potValue); // mando el valor junto con el evento
    delay(200);               // debounce simple
  }
}
```
#### Código Processing
```js
import processing.serial.*;

Serial myPort;
ArrayList<CircleData> circles; 

void setup() {
  size(1200, 720);
  background(0);
  
  // Ajusta el puerto según tu Arduino
  println(Serial.list());
  myPort = new Serial(this, "/dev/cu.usbmodem1101", 9600);
  //myPort = new Serial(this, Serial.list()[0], 9600);
  
  circles = new ArrayList<CircleData>();
}

void draw() {
  //background(0);
  
  // Dibujar todos los círculos guardados
  //fill(0, 150, 255);
  //noStroke();
  fill(73, 187, 221);
  stroke(203, 40, 19);
  for (CircleData c : circles) {
    ellipse(c.x, c.y, c.size, c.size);
  }
  
  // Leer datos de Arduino
  if (myPort.available() > 0) {
    String val = myPort.readStringUntil('\n');
    if (val != null) {
      val = trim(val);
      if (val.startsWith("BTN")) {
        // Extraer el valor del potenciómetro
        String[] parts = split(val, ',');
        if (parts.length == 2) {
          float potVal = float(parts[1]);
          float circleSize = map(potVal, 0, 1023, 10, 100); // tamaño 10-100 px
          circles.add(new CircleData(random(width), random(height), circleSize));
        }
      }
    }
  }
}

// Clase para guardar datos de cada círculo
class CircleData {
  float x, y, size;
  CircleData(float x, float y, float size) {
    this.x = x;
    this.y = y;
    this.size = size;
  }
}
```
### Ej n9: If/else

#### Código Arduino
```js
int valor;  // aquí guardaremos la lectura del sensor

void setup() {
  Serial.begin(9600);   // Inicia la comunicación serial
}

void loop() {
  valor = analogRead(A0);   // lee el pin analógico A0

  if (valor < 300) {
    Serial.println("Muy bajo");
  } else if (valor < 600) {
    Serial.println("Medio");
  } else {
    Serial.println("Alto");
  }

  delay(500); // medio segundo entre lecturas
}
```
<img src= "https://github.com/smanriquezj/INTERFAZII/blob/main/img/If%20else.png">

### Ej n10: Botonera + Audio

#### Código Arduino
```js
const int numButtons = 3;
const int buttonPins[numButtons] = {2, 4, 7};
const int ledButtonPins[numButtons] = {9, 10, 11}; // LEDs botones

// --- Configuración de potenciómetros ---
const int numPots = 2;
const int potPins[numPots] = {A0, A1};
const int ledPotPins[numPots] = {3, 5}; // LEDs PWM

// Variables de estados previos
int lastButtonState[numButtons];
int lastPotValue[numPots];

void setup() {
  Serial.begin(9600);

  // Configurar botones y LEDs
  for (int i = 0; i < numButtons; i++) {
    pinMode(buttonPins[i], INPUT_PULLUP);
    pinMode(ledButtonPins[i], OUTPUT);
    lastButtonState[i] = digitalRead(buttonPins[i]);
  }

  // Configurar LEDs de potenciómetros
  for (int i = 0; i < numPots; i++) {
    pinMode(ledPotPins[i], OUTPUT);
    lastPotValue[i] = analogRead(potPins[i]);
  }
}

void loop() {
  // Leer y enviar botones
  for (int i = 0; i < numButtons; i++) {
    int buttonState = digitalRead(buttonPins[i]);

    // LED se enciende cuando botón está presionado
    digitalWrite(ledButtonPins[i], buttonState == LOW ? HIGH : LOW);

    if (buttonState != lastButtonState[i]) {  // enviar cambios
      Serial.print("B");
      Serial.print(i); 
      Serial.print(":");
      Serial.println(buttonState);
      lastButtonState[i] = buttonState;
    }
  }

  // Leer y enviar potenciómetros
  for (int i = 0; i < numPots; i++) {
    int potValue = analogRead(potPins[i]); // 0–1023
    int pwmValue = potValue / 4;           // 0–255

    // Ajustar LED según valor
    analogWrite(ledPotPins[i], pwmValue);

    if (abs(pwmValue - lastPotValue[i]) > 2) { // evitar ruido
      Serial.print("P");
      Serial.print(i);
      Serial.print(":");
      Serial.println(pwmValue);
      lastPotValue[i] = pwmValue;
    }
  }

  delay(10);
}
```
<img src= "https://github.com/smanriquezj/INTERFAZII/blob/main/img/botonera%20con%20sonido.png">
<img src= "https://github.com/smanriquezj/INTERFAZII/blob/main/img/botonera%20fis.png">

#### Código Processing
```js
// Importamos librería para comunicación serial
import processing.serial.*;
// Importamos librería Minim para manejar audio
import ddf.minim.*;

// Declaramos el objeto serial para comunicarnos con Arduino
Serial myPort;
// Objeto principal de Minim
Minim minim;
// Array de reproductores de audio (3 pistas)
AudioPlayer[] players;
// Variable para guardar el índice de la pista que está sonando
int currentTrack = -1;  // -1 significa que no hay pista activa al inicio

void setup() {
  size(400, 200); // Ventana de 400x200 píxeles
  
  // --- Configuración del puerto serial ---
  printArray(Serial.list()); // Muestra en consola la lista de puertos disponibles
  myPort = new Serial(this, Serial.list()[0], 9600); // Abrimos el primer puerto de la lista a 9600 baudios
  
  // --- Configuración de audio ---
  minim = new Minim(this); // Inicializamos Minim
  players = new AudioPlayer[3]; // Creamos un array de 3 reproductores
  
  // Cargamos los 3 archivos de audio desde la carpeta "data"
  players[0] = minim.loadFile("audio1.mp3", 2048); 
  players[1] = minim.loadFile("audio2.mp3", 2048); 
  players[2] = minim.loadFile("audio3.mp3", 2048); 
}

void draw() {
  background(255); // Fondo negro
  fill(10);     // Color blanco para el texto
  textSize(20);  // Tamaño del texto
  
  // Mostramos en pantalla qué botón está activo
  text("Botón actual: " + (currentTrack == -1 ? "ninguno" : currentTrack), 20, 40);
}

void serialEvent(Serial myPort) {
  // Leemos la cadena que llega desde Arduino hasta el salto de línea
  String inString = trim(myPort.readStringUntil('\n'));
  
  // Si no llega nada, salimos
  if (inString == null) return;

  // --- Si el mensaje recibido empieza con "B" significa que es un botón ---
  if (inString.startsWith("B")) {
    // Quitamos la letra "B" y separamos el mensaje en partes (ejemplo "0:0")
    String[] parts = split(inString.substring(1), ':');
    
    // Si realmente recibimos dos partes (índice y estado)
    if (parts.length == 2) {
      int buttonIndex = int(parts[0]); // Número del botón (0,1,2)
      int state = int(parts[1]);       // Estado del botón (0 = presionado, 1 = suelto)
      
      // Si el botón fue presionado (LOW = 0 en Arduino)
      if (state == 0) { 
        playTrack(buttonIndex); // Llamamos a la función para reproducir la pista correspondiente
      }
    }
  }
}

// --- Función que reproduce una pista según el botón ---
void playTrack(int index) {
  // Si ya había una pista sonando, la pausamos y la rebobinamos al inicio
  if (currentTrack != -1 && players[currentTrack].isPlaying()) {
    players[currentTrack].pause();
    players[currentTrack].rewind();
  }
  
  // Reproducimos en bucle la pista seleccionada
  players[index].loop();
  
  // Actualizamos la variable para saber cuál es la pista activa
  currentTrack = index;
}
```
### Ejercicio nota 1: 4 leds intermitentes
```js
int leds[] = {2, 3, 4, 5}; // Creamos un arreglo con los pines donde van conectados los LEDs

void setup (){
  
}

void loop() {
  // Esta función corre en bucle infinito
  // for (int i = 0; i < 4; i++) {         // Recorre el arreglo desde i = 0 hasta i = 3
  // pinMode(leds[i], OUTPUT);           // Configura cada pin del arreglo como salida (para controlar LEDs)
  //}
  for (int j = 0; j < 4; j++) {   
         pinMode(leds[j], OUTPUT);

        // Recorre los 4 LEDs, uno por uno
    if (j % 1 == 0) {                   // Si el índice es par (0, 2)...
      digitalWrite(leds[j], HIGH);      // Enciende el LED correspondiente
    } else {                            // Si el índice es impar (1, 3)...
      digitalWrite(leds[j], LOW);       // Apaga el LED correspondiente
    }
      delay(500);
    pinMode(leds[j], LOW);

    // Espera 0,5 segundos antes de pasar al siguiente
  }



```
<img src= "https://github.com/smanriquezj/INTERFAZII/blob/main/img/4ledsparpadeantes.jpeg"> 
<img src= "https://github.com/smanriquezj/INTERFAZII/blob/main/img/4leds.png">

```js
import processing.serial.*;


Serial myPort;
PImage[] imgs;
int numImages = 3;
PImage avgImg;
float mixAmount = 0;

void setup() {
  size(770, 700);
  println(Serial.list());
  
  //Cambia el índice según tu puerto (0, 1, 2, etc.)
  myPort = new Serial(this, Serial.list()[0], 9600);
  //myPort = new Serial(this, "/dev/cu.usbmodem1101", 9600);
  myPort.bufferUntil('\n');

  // Cargar imágenes
  imgs = new PImage[numImages];
  imgs[0] = loadImage("img1.png");
  imgs[1] = loadImage("img2.png");
  imgs[2] = loadImage("img3.png");

  avgImg = createImage(imgs[0].width, imgs[0].height, RGB);
}

void draw() {
  // Dibujar la imagen promedio según el valor del potenciómetro
  background(0);
  calcAverage(mixAmount);
  image(avgImg, 0, 0, width, height);
  
  fill(255);
  textSize(20);
  text("Mezcla: " + nf(mixAmount, 1, 2), 20, height - 20);
}

void serialEvent(Serial p) {
  String val = p.readStringUntil('\n');
  if (val != null) {
    val = trim(val);
    float sensor = float(val);
    mixAmount = map(sensor, 0, 1023, 0, 1); // 0 a 1
  }
}

void calcAverage(float t) {
  avgImg.loadPixels();

  for (int i = 0; i < avgImg.pixels.length; i++) {
    color c1 = imgs[0].pixels[i];
    color c2 = imgs[1].pixels[i];
    color c3 = imgs[2].pixels[i];

    // Promedio ponderado según el potenciómetro
    float r = red(c1)*(1-t) + red(c2)*t*0.5 + red(c3)*t*0.5;
    float g = green(c1)*(1-t) + green(c2)*t*0.5 + green(c3)*t*0.5;
    float b = blue(c1)*(1-t) + blue(c2)*t*0.5 + blue(c3)*t*0.5;

    avgImg.pixels[i] = color(r, g, b);
  }
  avgImg.updatePixels();
}
}
```
### Sensor de proximidad
#### Arduino
```js
// Pines del sensor ultrasónico HC-SR04
const int trigPin = 9; // Pin Trig conectado al pin digital 9 de Arduino
const int echoPin = 10; // Pin Echo conectado al pin digital 10 de Arduino

// Variables para la medición de distancia
long duration; // Variable para almacenar la duración del pulso
int distance_cm; // Variable para almacenar la distancia en centímetros
const int maxDistance = 200; // Distancia máxima que quieres medir (en cm)
const int minDistance = 5;   // Distancia mínima que quieres medir (en cm)

void setup() {
  // Inicializa la comunicación serial a la misma velocidad que Processing
  Serial.begin(9600);
  
  // Configura los pines del sensor
  pinMode(trigPin, OUTPUT); // Pin de activación como salida
  pinMode(echoPin, INPUT);  // Pin de eco como entrada
}

void loop() {
  // 1. Limpia el pin Trig (asegura que está bajo)
  digitalWrite(trigPin, LOW);
  delayMicroseconds(2);
  
  // 2. Envía un pulso de 10 microsegundos para activar el sensor
  digitalWrite(trigPin, HIGH);
  delayMicroseconds(10);
  digitalWrite(trigPin, LOW);
  
  // 3. Lee el pin Echo, devuelve la duración del viaje del sonido
  duration = pulseIn(echoPin, HIGH);
  
  // 4. Calcula la distancia
  // Velocidad del sonido es 343 m/s o 0.0343 cm/µs. 
  // La distancia es (duración * 0.0343) / 2 (ida y vuelta)
  distance_cm = duration * 0.0343 / 2;
  
  // 5. Mapea la distancia al rango de valores que Processing espera
  // El código de Processing espera un valor entre 0 y 1023 (del potValue original).
  // Mapeamos un rango útil de distancia (ej: 5cm a 200cm) a 0 a 1023.
  // Notar que 'constrain' invierte el rango para que la cercanía (5cm) sea 1023 y la lejanía (200cm) sea 0, 
  // lo cual puede ser más intuitivo (más cerca -> siguiente imagen).
  int mappedValue = map(constrain(distance_cm, minDistance, maxDistance), minDistance, maxDistance, 1023, 0); 
  
  // 6. Envía el valor mapeado por Serial, seguido de un salto de línea
  Serial.println(mappedValue);
  
  // Pequeña pausa para evitar enviar datos demasiado rápido
  delay(50); 
}
```
### Processing
```js
int rectX, rectY;      // Position of square button
int circleX, circleY;  // Position of circle button
int rectSize = 30;     // Diameter of rect
int circleSize = 30;   // Diameter of circle
color rectColor, circleColor, baseColor;
color rectHighlight, circleHighlight;
color currentColor;
boolean rectOver = false;
boolean circleOver = false;
PImage img; // Declare variable 'img'.

void setup() {
  size(700, 525);
  img = loadImage("base.jpeg");
  rectColor = color(0); //color cuadrado boton
  rectHighlight = color(51); //Color de cuadrado apretado
  circleColor = color(255); //lo mismo circulo
  circleHighlight = color(204); // lo mismo circulo
  //baseColor = color(102);
  //currentColor = baseColor;
  circleX = width/2+circleSize/2+150;// posicion circulo X
  circleY = height/2;
  rectX = width/2-rectSize-180;
  rectY = height/2-rectSize/2+20;
  ellipseMode(CENTER);
}

void draw() {
  update(mouseX, mouseY);
  background(img);

  if (rectOver) {
    fill(rectHighlight);
  } else {
    fill(rectColor);
  }
  stroke(255);
  rect(rectX, rectY, rectSize, rectSize);

  if (circleOver) {
    fill(circleHighlight);
  } else {
    fill(circleColor);
  }
  stroke(0);
  ellipse(circleX, circleY, circleSize, circleSize);
}

void update(int x, int y) {
  if ( overCircle(circleX, circleY, circleSize) ) {
    circleOver = true;
    rectOver = false;
  } else if ( overRect(rectX, rectY, rectSize, rectSize) ) {
    rectOver = true;
    circleOver = false;
  } else {
    circleOver = rectOver = false;
  }
}

void mousePressed() {
  if (circleOver) {
    currentColor = circleColor;
  }
  if (rectOver) {
    currentColor = rectColor;
  }
}

boolean overRect(int x, int y, int width, int height) {
  if (mouseX >= x && mouseX <= x+width &&
    mouseY >= y && mouseY <= y+height) {
    return true;
  } else {
    return false;
  }
}

boolean overCircle(int x, int y, int diameter) {
  float disX = x - mouseX;
  float disY = y - mouseY;
  if (sqrt(sq(disX) + sq(disY)) < diameter/2 ) {
    return true;
  } else {
    return false;
  }
}
```
