#include <LiquidCrystal_I2C.h>

// C++ code
// PROYECTO DE MANUEL REYES SERRANO Y ANTONIO CAÑETE LÓPEZ
// PROYECTO DESACTIVA LA BOMBA
// ARDUINO N.2

const int LEDS[] = {6, 5, 4, 3}; 
const int PASOS = 5; 

void setup()
{
  Serial.begin(9600);
  
  for (int i = 0; i < 4; i++)
  {
    pinMode(LEDS[i], OUTPUT); 
  }
  
  randomSeed(analogRead(A5)); 
}

void loop()
{
  if (Serial.available() > 0)
  {
    char comando = Serial.read();
    
    if (comando == 'y')
    {
      int secuencia[PASOS]; 
      
      for (int i = 0; i < PASOS; i++)
      {
        secuencia[i] = random(0, 4);
      }
      
      for (int i = 0; i < PASOS; i++)
      {
        delay(200);
        digitalWrite(LEDS[secuencia[i]], HIGH);
        delay(600);
        digitalWrite(LEDS[secuencia[i]], LOW);
      }
      
      for (int i = 0; i < PASOS; i++)
      {
        Serial.write(secuencia[i]);
      }
    }
  }
}

/*

VARIABLES QUE SE USAN: 

LEDS[] - Array con los pines de los 4 LEDs que muestran la secuencia.
PASOS - Longitud total de la secuencia del juego.
comando - Guarda el carácter recibido por Serial para activar el juego ('y').
secuencia[] - Array temporal que almacena los números aleatorios generados.

*/