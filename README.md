Automated Plant Watering System using
IoT and Robotics

1. Introduction
Watering plants regularly is essential for their healthy growth, but many times plants
either get over-watered or under-watered due to irregular human intervention. To solve this, an
automated plant watering system is developed using IoT (Internet of Things) and Robotics.

2. Components Required
* ESP32 (or NodeMCU ESP8266)
* Soil Moisture Sensor (capacitive recommended)
* Relay Module (5V/3.3V)
* DC Water Pump (12V mini pump)
* Servo Motor (SG90/MG995)
* Power Supply (12V & 5V)
* Jumper wires, tubing

3. System Design & Working
     * Soil moisture sensor detects soil water content.
     * ESP32 processes data and uploads it to Blynk IoT Cloud.
     * If soil moisture is below threshold, relay switches ON pump.
     * Servo motor rotates pipe towards selected pot.
     * User can monitor via Blynk app

4. Circuit Diagram
Soil Sensor: VCC→3.3V, GND→GND, A0→GPIO34
Relay: VCC→5V, GND→GND, IN→GPIO26
Pump: +→Relay COM, -→12V(-), Relay NO→12V(+)
Servo: VCC→5V, GND→GND, Signal→GPIO27

5. Program Code (ESP32 + Blynk IoT)
#define BLYNK_TEMPLATE_ID "YourTemplateID"
#define BLYNK_DEVICE_NAME "Plant Watering System"
#define BLYNK_AUTH_TOKEN "YourAuthToken"
#include <WiFi.h>
#include <WiFiClient.h>
#include <BlynkSimpleEsp32.h>
#include <Servo.h>
char auth[] = BLYNK_AUTH_TOKEN;
char ssid[] = "YourWiFiSSID";
char pass[] = "YourWiFiPassword";
#define SOIL_PIN 34
#define RELAY_PIN 26
#define SERVO_PIN 27
Servo waterServo;
BlynkTimer timer;
void sendSensor()
{
int sensorValue = analogRead(SOIL_PIN);int moisturePercent = map(sensorValue, 4095, 0, 0, 100);
Blynk.virtualWrite(V0, moisturePercent);
if (moisturePercent < 40)
{
digitalWrite(RELAY_PIN, LOW);
Blynk.virtualWrite(V1, 1);
waterServo.write(60);
delay(3000);
digitalWrite(RELAY_PIN, HIGH);
Blynk.virtualWrite(V1, 0);
waterServo.write(0);
} else
{
digitalWrite(RELAY_PIN, HIGH);
Blynk.virtualWrite(V1, 0);
}
}
void setup()
{
Serial.begin(115200);
pinMode(RELAY_PIN, OUTPUT);
digitalWrite(RELAY_PIN, HIGH);
waterServo.attach(SERVO_PIN);
waterServo.write(0);
Blynk.begin(auth, ssid, pass);timer.setInterval(2000L, sendSensor);
}
void loop()
{
Blynk.run();
timer.run();
}

6. IoT Dashboard
In Blynk app:
* Gauge (V0) → Soil Moisture (%)
* LED (V1) → Pump Status
* Button → Manual Pump Control

7. Advantages
-> Fully automatic watering
-> IoT remote monitoring
-> Robotics allows watering multiple pots
-> Prevents overwatering/underwatering
-> Expandable with DHT22 for climate monitoring

8. Applications
* Smart home gardening
* Greenhouses & nurseries
* Agricultural automation* Rooftop gardens

9. Conclusion
This Automated Plant Watering System using IoT and Robotics ensures efficient plant
growth through precision irrigation while reducing human effort and water wastage.
