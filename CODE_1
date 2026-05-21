#include <Wire.h>
#include <LiquidCrystal_I2C.h>

// C++ code
// PROYECTO DE MANUEL REYES SERRANO Y ANTONIO CAÑETE LÓPEZ
// PROYECTO DESACTIVA LA BOMBA
// ARDUINO N.1

const int LED_AZUL = 2;
const int LED_AMARILLO = 3;
const int LED_ROJO = 4;
const int LED_VERDE = 5;
const int PIN_BUZZER = 6;
const int PIN_POTENCIOMETRO = A2;
const int BOTON_START = 12;
const int BOTONES[] = {8, 9, 10, 11};

const int PASOS = 5; 

LiquidCrystal_I2C lcd(0x20, 16, 2); 

int secuenciaObjetivo[PASOS]; 
int pasoActual = 0;
int tiempoBomba = 0;
int tiempoRestante = 0;
int contadorMuestrasCiclo = 0; 
int estadoJuego = 0; 

void setup()
{
  Serial.begin(9600);
  lcd.init();
  lcd.backlight();
  
  pinMode(LED_AZUL, OUTPUT);
  pinMode(LED_AMARILLO, OUTPUT);
  pinMode(LED_ROJO, OUTPUT);
  pinMode(LED_VERDE, OUTPUT);
  pinMode(PIN_BUZZER, OUTPUT);
  
  pinMode(BOTON_START, INPUT_PULLUP); 
  
  for (int i = 0; i < 4; i++)
  {
    pinMode(BOTONES[i], INPUT_PULLUP);
  }
}

void loop()
{
  // BOMBA ARMADA (LED AZUL) 
  if (estadoJuego == 0)
  {
    digitalWrite(LED_AZUL, HIGH);
    digitalWrite(LED_AMARILLO, LOW);
    digitalWrite(LED_ROJO, LOW);
    digitalWrite(LED_VERDE, LOW);
    
    int lecturaPot = analogRead(PIN_POTENCIOMETRO);
    tiempoBomba = map(lecturaPot, 0, 1023, 10, 60); 
    
    lcd.setCursor(0, 0);
    lcd.print("BOMBA ARMADA ");
    lcd.setCursor(0, 1);
    lcd.print("Tiempo: "); lcd.print(tiempoBomba); lcd.print("s    ");
    
    if (digitalRead(BOTON_START) == LOW)
    {
      Serial.print('y'); 
      
      int leidos = 0;
      while (leidos < PASOS)
      {
        if (Serial.available() > 0)
        {
          secuenciaObjetivo[leidos] = Serial.read();
          leidos++;
        }
      }
      
      tiempoRestante = tiempoBomba;
      pasoActual = 0;
      contadorMuestrasCiclo = 0;
      lcd.clear();
      estadoJuego = 1; 
    }
  }
  
  // DESARMANDO (LED AMARILLO)
  else if (estadoJuego == 1)
  {
    digitalWrite(LED_AZUL, LOW);
    digitalWrite(LED_AMARILLO, HIGH);
    
    lcd.setCursor(0, 0);
    lcd.print("DESACTIVANDO... ");
    lcd.setCursor(0, 1);
    lcd.print("Tiempo: "); lcd.print(tiempoRestante); lcd.print("s   ");
    
    contadorMuestrasCiclo++; 
    
    if (tiempoRestante <= 0)
    {
      estadoJuego = 2;
    }
    
    if (contadorMuestrasCiclo >= 10)
    {
      tiempoRestante--;
      tone(PIN_BUZZER, 1000, 100);
      contadorMuestrasCiclo = 0; 
    }
    
    for (int i = 0; i < 4; i++)
    {
      if (digitalRead(BOTONES[i]) == LOW)
      {
        if (i == secuenciaObjetivo[pasoActual])
        {
          pasoActual++;
          tone(PIN_BUZZER, 1500, 150);
          if (pasoActual >= PASOS)
          {
            estadoJuego = 3; 
          }
        }
        else
        {
          estadoJuego = 2; 
        }
        delay(200); 
      }
    }
  }
  
  // EXPLOSIÓN (LED ROJO)
  else if (estadoJuego == 2)
  {
    digitalWrite(LED_AMARILLO, LOW);
    digitalWrite(LED_ROJO, HIGH);
    
    lcd.setCursor(0, 0);
    lcd.print("KABOOOOOOOOM!!! ");
    lcd.setCursor(0, 1);
    lcd.print("PERDISTE!!      ");
    
    tone(PIN_BUZZER, 150, 2000); 
    while (true)
    {
    }
  }
  
  // VICTORIA (LED VERDE)
  else if (estadoJuego == 3)
  {
    digitalWrite(LED_AMARILLO, LOW);
    digitalWrite(LED_VERDE, HIGH);
    
    lcd.setCursor(0, 0);
    lcd.print("BOMBA DESACTIVADA");
    lcd.setCursor(0, 1);
    lcd.print("VICTORIA!!      ");
    
    tone(PIN_BUZZER, 700, 200); delay(200);
    tone(PIN_BUZZER, 1200, 400);
    while (true)
    {
    }
  }
  
  delay(100);
}

/*

VARIABLES

PASOS - cuantas partes tiene la combinación que desactiva la bomba
secuenciaObjetivo[PASOS] - almacena la clave correcta enviada por la Placa 2
pasoActual - contador del paso o botón actual que el jugador debe presionar
tiempoBomba - tiempo configurado desde el potenciómetro
tiempoRestante - segundos que quedan en la cuenta atrás
contadorMuestrasCiclo - mide 1 segundo que pasa y se lo resta al contador
estadoJuego - 0: Armada, 1: Desarmando, 2: Explosión, 3: Victoria, da el estado del juego
lecturaPot - almacena el valor del potenciómetro (0-1023)
leidos - cuenta los botones que metemos

*/

LiquidCrystal lcd(12, 11, 5, 4, 3, 2);
