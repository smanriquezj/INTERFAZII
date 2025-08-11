# INTERFAZII
### Ejercicio1: Hola mundo!
```js
void setup() {
  Serial.begin(9600); // Inicia la comunicación serie a 9600 bps
  Serial.println("Hola, quiero dormir"); // Envía "Hola, Mundo!" al monitor serie
}

void loop() {
  // No es necesario poner nada en el loop para este ejemplo
}
```
### Ej n2: LEDs intermitentes
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
