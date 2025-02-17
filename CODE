#include <ESP8266WiFi.h>
#include <BlynkSimpleEsp8266.h>
#include <SimpleDHT.h>

#define BLYNK_TEMPLATE_ID "YourTemplateID"
#define BLYNK_DEVICE_NAME "SmartIrrigation"
#define BLYNK_AUTH_TOKEN "YourAuthToken"

SimpleDHT11 dht11(D2);   // Temperature and humidity sensor on pin D2
int moisturePin = A0;    // Soil moisture sensor on pin A0
int valvePin = D3;       // Solenoid valve control on pin D3

char ssid[] = "YourSSID";
char pass[] = "YourPassword";

BlynkTimer timer;

void setup() {
    Serial.begin(9600);
    Blynk.begin(BLYNK_AUTH_TOKEN, ssid, pass);
    pinMode(valvePin, OUTPUT);
    timer.setInterval(2000L, sendSensorData); // Send data every 2 seconds
}

void sendSensorData() {
    int moistureValue = analogRead(moisturePin);
    Blynk.virtualWrite(V1, moistureValue);
    byte temperature = 0;
    byte humidity = 0;
    dht11.read(&temperature, &humidity, NULL);
    Blynk.virtualWrite(V2, temperature);
    Blynk.virtualWrite(V3, humidity);

    if (moistureValue < 500) {
        digitalWrite(valvePin, HIGH); // Activate valve
    } else if (moistureValue > 800) {
        digitalWrite(valvePin, LOW);  // Deactivate valve
    }
}

void loop() {
    Blynk.run();
    timer.run();
}
