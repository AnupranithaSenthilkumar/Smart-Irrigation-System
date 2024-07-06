# Smart Irrigation System

## Project Description

The Smart Irrigation System leverages IoT technology to optimize water usage in agriculture. By monitoring soil moisture and temperature in real-time, this system helps farmers make data-driven irrigation decisions, improving crop yields and promoting sustainable farming practices. The system is controlled via the Blynk mobile app, which provides graphical representations of soil conditions and automates water supply based on soil dryness.

## Features
- **Real-time Monitoring**: Continuous monitoring of soil moisture and temperature.
- **Blynk Mobile App**: Displays data graphically for easy interpretation.
- **Automation**: Controls water supply automatically based on soil moisture levels.
- **Customizable Thresholds**: Allows setting different moisture thresholds for various soil types.
- **Data Logging**: Logs motor on/off timings and soil data for analysis.
- **Remote Access**: Control and monitor the system from anywhere via the Blynk app.

## Components

### Hardware
- ESP32 Microcontroller
- Soil Moisture Sensor (FC-28)
- DHT-11 Temperature and Humidity Sensor
- Single Channel Relay Module
- Connecting Wires
- 9V Battery and Connector
- Breadboard

### Software
- Arduino IDE for coding and uploading programs to the ESP32
- Blynk platform for creating the mobile app interface and dashboards

## Circuit Diagram
#### Configuration circuit for controlling pump through Relay
![Configuration circuit for controlling pump through Relay](https://github.com/AnupranithaSenthilkumar/Smart-Irrigation-System/assets/162324840/af824552-e0fe-43c9-9fa6-1729a5692379)
#### Configuration circuit for ESP32 with soil moisture and DHT-11 Sensor
![Configuration circuit for ESP32 with soil moisture and DHT-11 Sensor](https://github.com/AnupranithaSenthilkumar/Smart-Irrigation-System/assets/162324840/e9ccb9a0-2166-4c09-b146-61cc0802ec3b)

## System Workflow
1. **Data Collection**: Sensors measure soil moisture and temperature.
2. **Data Transmission**: ESP32 sends data to the Blynk app via Wi-Fi.
3. **Visualization**: Real-time data is displayed graphically on the Blynk app.
4. **Automation**: The relay module activates/deactivates the water pump based on moisture thresholds.
5. **Analysis**: Logs motor activity and environmental data for irrigation strategy optimization.

## Installation

### Hardware Setup
1. **Connect the Soil Moisture Sensor**:
   - VCC to 3.3V on ESP32
   - GND to GND on ESP32
   - AO to A0 on ESP32

2. **Connect the DHT-11 Sensor**:
   - VCC to 3.3V on ESP32
   - GND to GND on ESP32
   - Data pin to D4 on ESP32

3. **Connect the Relay Module**:
   - VCC to 3.3V on ESP32
   - GND to GND on ESP32
   - IN to D5 on ESP32

4. **Connect the Water Pump**:
   - To the relay module output pins

5. **Power the ESP32**:
   - Using the 9V battery connected to the VIN pin

### Software Setup
1. **Install Arduino IDE**: Download and install the [Arduino IDE](https://www.arduino.cc/en/Main/Software).
2. **Install Blynk Library**: In the Arduino IDE, go to Sketch -> Include Library -> Manage Libraries and search for "Blynk". Install the Blynk library.
3. **Install DHT Sensor Library**: Similarly, install the "DHT sensor library" by Adafruit.
4. **Configure Blynk App**: 
   - Download the Blynk app from the App Store or Google Play.
   - Create a new project and get the Auth Token.
   - Add widgets for Value Display, Gauge, Graph, etc., and link them to virtual pins.
5. **Upload Code**: Use the following code template and upload it to the ESP32 using Arduino IDE.

### Sample Code
```cpp
#include <WiFi.h>
#include <BlynkSimpleEsp32.h>
#include <DHT.h>

#define DHTPIN 4
#define DHTTYPE DHT11
#define RELAY_PIN 5
#define SOIL_MOISTURE_PIN 34

char auth[] = "YourAuthToken";
char ssid[] = "YourSSID";
char pass[] = "YourPassword";

DHT dht(DHTPIN, DHTTYPE);
BlynkTimer timer;

void setup() {
  Serial.begin(115200);
  Blynk.begin(auth, ssid, pass);
  dht.begin();
  pinMode(RELAY_PIN, OUTPUT);
  digitalWrite(RELAY_PIN, LOW);
  timer.setInterval(2000L, sendSensorData);
}

void sendSensorData() {
  float temp = dht.readTemperature();
  float humidity = dht.readHumidity();
  int soilMoisture = analogRead(SOIL_MOISTURE_PIN);
  
  Blynk.virtualWrite(V1, temp);
  Blynk.virtualWrite(V2, humidity);
  Blynk.virtualWrite(V3, soilMoisture);

  if (soilMoisture < 500) { // Adjust threshold as needed
    digitalWrite(RELAY_PIN, HIGH);
  } else {
    digitalWrite(RELAY_PIN, LOW);
  }
}

void loop() {
  Blynk.run();
  timer.run();
}
```

## Usage
1. **Power On**: Connect the battery to power the ESP32.
2. **Open Blynk App**: Monitor real-time data and control irrigation from the Blynk app.
3. **Automation**: The system will automatically water the soil based on the moisture readings.
4. **Customization**: Adjust moisture thresholds in the code or app to suit different soil types.

## Benefits
- **Water Conservation**: Minimizes water wastage by ensuring precise irrigation.
- **Labor Efficiency**: Reduces manual monitoring and intervention.
- **Crop Health**: Maintains optimal soil conditions for better crop yields.
- **Sustainability**: Promotes sustainable farming practices.

## Future Enhancements
- Integration with weather forecasting services for predictive irrigation.
- Expansion to include multiple sensors for larger fields.
- Enhanced analytics and reporting features in the Blynk app.

## License
This project is licensed under the MIT License. See the LICENSE file for more details.

## Acknowledgements
- [Blynk](https://blynk.io/) for the IoT platform.
- [Adafruit](https://www.adafruit.com/) for sensor libraries.

---

