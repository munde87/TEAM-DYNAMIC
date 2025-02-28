# TEAM-DYNAMIC
#define BLYNK_TEMPLATE_ID "TMPL3dZVNeIHP"
#define BLYNK_TEMPLATE_NAME "Digital Farming"
#define BLYNK_AUTH_TOKEN "LCEUyg6nH_hGCrJaS3E-aakh5pxHCkGY"

#include <Wire.h>
#include <LiquidCrystal_I2C.h>
#include <DHT.h>
#include <WiFi.h>
#include <WiFiClient.h>
#include <BlynkSimpleEsp32.h>

// Replace with your WiFi credentials
char auth[] = BLYNK_AUTH_TOKEN ;
char ssid[] = "Wokwi-GUEST";
char pass[] = "";

// Blynk virtual pins
#define BLYNK_VIRTUAL_PIN V0
#define BLYNK_TEMPERATURE_PIN V1
#define BLYNK_HUMIDITY_PIN V2
// normal pins
#define POTENTIOMETER_PIN 34
#define LED1 12
#define RELAY 13
#define RELAY2 14
#define RELAY3 15

// DHT22 Configuration
#define DHT_PIN 12    // DHT22 data pin
#define DHT_TYPE DHT22
DHT dht(DHT_PIN, DHT_TYPE);

// LCD I2C Configuration
LiquidCrystal_I2C lcd(0x27, 16, 2);  // Set LCD I2C address (usually 0x27)

// Variables
int potentiometerValue;
float voltage, temperature, humidity;

// Timer to send data periodically
BlynkTimer timer;

int SW_State_M = 0;
BLYNK_WRITE(V3) {
  SW_State_M = param.asInt();
  if (SW_State_M == 1) {
    digitalWrite(LED1,LOW);
    digitalWrite(RELAY, LOW);
    Serial.println("RED ON");
    Blynk.virtualWrite(V3, LOW);
  } else {
    digitalWrite(LED1, HIGH);
    digitalWrite(RELAY, HIGH);
    Serial.println("RED OFF");
    Blynk.virtualWrite(V3, HIGH);
  }
}

int Relay2 = 0;
BLYNK_WRITE(V4) {
  Relay2 = param.asInt();
  if (Relay2 == 1) {
    digitalWrite(LED1,LOW);
    digitalWrite(RELAY2, LOW);
    Serial.println("RED ON");
    Blynk.virtualWrite(V4, LOW);
  } else {
    digitalWrite(LED1, HIGH);
    digitalWrite(RELAY2, HIGH);
    Serial.println("RED OFF");
    Blynk.virtualWrite(V4, HIGH);
  }
}

int Relay3 = 0;
BLYNK_WRITE(V5) {
  Relay3 = param.asInt();
  if (Relay3 == 1) {
    digitalWrite(LED1,LOW);
    digitalWrite(RELAY3, LOW);
    Serial.println("RED ON");
    Blynk.virtualWrite(V5, LOW);
  } else {
    digitalWrite(LED1, HIGH);
    digitalWrite(RELAY3, HIGH);
    Serial.println("RED OFF");
    Blynk.virtualWrite(V5, HIGH);
  }
}

// Function to read and send potentiometer data
void sendPotentiometerData() {
  potentiometerValue = analogRead(POTENTIOMETER_PIN);   // Read potentiometer value
  voltage = potentiometerValue * (3.3 / 4095.0);       // Convert to voltage for ESP32
  Blynk.virtualWrite(BLYNK_VIRTUAL_PIN, potentiometerValue);  // Send data to Blynk App
  Serial.print("Potentiometer Value: ");
  Serial.println(potentiometerValue);  // Print to Serial Monitor
}

// Function to read and send DHT22 data
void sendDHTData() {
  temperature = dht.readTemperature();  // Read temperature
  humidity = dht.readHumidity();        // Read humidity



  Blynk.virtualWrite(BLYNK_TEMPERATURE_PIN, temperature);  // Send temperature to Blynk
  Blynk.virtualWrite(BLYNK_HUMIDITY_PIN, humidity);        // Send humidity to Blynk

  Serial.print("Temperature: ");
  Serial.print(temperature);
  Serial.print(" °C, Humidity: ");
  Serial.print(humidity);
  Serial.println(" %");

  // Display on LCD
  lcd.setCursor(0, 0);
  lcd.print("Temp: ");
  lcd.print(temperature);
  lcd.print(" C");
  lcd.setCursor(0, 1);
  lcd.print("Hum: ");
  lcd.print(humidity);
  lcd.print(" %");
}

void setup() {
  // Start serial communication
  Serial.begin(9600);

  // Initialize pins
  pinMode(DHT_PIN, INPUT);
  
  pinMode(POTENTIOMETER_PIN, INPUT);
  pinMode(LED1, OUTPUT);
  pinMode(RELAY, OUTPUT);

  // Start DHT sensor
  dht.begin();

  // Initialize LCD
  lcd.init();
  lcd.backlight();

  // Start Blynk
  Blynk.begin(BLYNK_AUTH_TOKEN, ssid, pass);

  // Setup a function to be called every second
  timer.setInterval(1000L, sendPotentiometerData);
  timer.setInterval(2000L, sendDHTData);  // Call DHT22 function every 2 seconds
}

void loop() {
  Blynk.run();  // Run Blynk connection
  timer.run();  // Run timer to send data periodically
}
