#include <Wire.h>
#include <LiquidCrystal_I2C.h>

LiquidCrystal_I2C lcd(0x27, 16, 2); // Cambia a 0x20 si tu LCD lo requiere

const int LED_ROJO = 13;
const int LED_AMARILLO = 12;
const int LED_VERDE = 11;

void mostrarEstado(String color, String accion, int tiempo)
{
  lcd.clear();
  lcd.setCursor(0, 0);
  lcd.print(color + " -> " + accion);

  for (int i = tiempo; i > 0; i--)
  {
    lcd.setCursor(0, 1);
    lcd.print("Tiempo: ");
    lcd.print(i);
    lcd.print(" seg   ");
    delay(1000);
  }
}

void setup()
{
  pinMode(LED_ROJO, OUTPUT);
  pinMode(LED_AMARILLO, OUTPUT);
  pinMode(LED_VERDE, OUTPUT);

  lcd.init();
  lcd.backlight();

  lcd.setCursor(2, 0);
  lcd.print("SEMAFORO");
  lcd.setCursor(1, 1);
  lcd.print("INTELIGENTE");

  delay(3000);
  lcd.clear();
}

void loop()
{
  // VERDE
  digitalWrite(LED_VERDE, HIGH);
  digitalWrite(LED_AMARILLO, LOW);
  digitalWrite(LED_ROJO, LOW);

  mostrarEstado("VERDE", "SIGA", 5);

  // AMARILLO
  digitalWrite(LED_VERDE, LOW);
  digitalWrite(LED_AMARILLO, HIGH);
  digitalWrite(LED_ROJO, LOW);

  mostrarEstado("AMARILLO", "CUIDADO", 3);

  // ROJO
  digitalWrite(LED_VERDE, LOW);
  digitalWrite(LED_AMARILLO, LOW);
  digitalWrite(LED_ROJO, HIGH);

  mostrarEstado("ROJO", "ALTO", 5);
}
