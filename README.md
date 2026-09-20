# IoT Temperature & Humidity Monitoring System

A small end-to-end IoT project: an Arduino reads a DHT11 sensor, switches an LED and buzzer on when the temperature is too high, and a Python program logs every reading to a SQLite database and a CSV file. A Java program summarizes the data and a Python script plots it.

**Built by:** Piyush Choudhury (BCA student)
**Tech:** Arduino (C++), Python, SQL (SQLite), Java, Git/GitHub

<!-- Add your own photo of the circuit and the generated plot after you build it: -->
<!-- ![Circuit](docs/circuit.jpg) -->
<!-- ![Plot](docs/plot.png) -->

## What it does
1. **Sensing:** the DHT11 measures temperature and humidity every 2 seconds.
2. **Actuation:** an LED and buzzer turn on when temperature exceeds 35 °C.
3. **Data collection:** Python reads the serial data, validates each line and stores it in SQLite and CSV.
4. **Reporting:** Java prints min / max / average values, Python plots the trends, and SQL queries analyze the data.

```
DHT11 -> Arduino -> LED + Buzzer
            |
       USB serial
            v
   Python logger -> SQLite DB + CSV -> Java summary / Python plot / SQL queries
```

## Components
- Arduino Uno (or ESP32) and USB cable
- DHT11 temperature & humidity sensor
- LED, 220 Ω resistor, active buzzer
- Breadboard and jumper wires

## Wiring (Arduino Uno)
| Part | Arduino pin |
|---|---|
| DHT11 VCC / GND / DATA | 5V / GND / D2 |
| LED (through 220 Ω) | D8 |
| Buzzer (+) | D9 (– to GND) |

For an **ESP32**, change the pin numbers at the top of the `.ino` file (for example GPIO 4, 16, 17) and power the sensor from 3.3V.

## How to run
1. Install the **DHT sensor library** (Adafruit) in the Arduino IDE, then upload `arduino/temp_humidity_monitor/temp_humidity_monitor.ino`.
2. Close the Serial Monitor, then run the logger:
   ```
   cd python
   pip install -r requirements.txt
   python logger.py --port COM3
   ```
   No hardware yet? Test with `python logger.py --simulate --count 30`.
3. Plot the data: `python plot_data.py`
4. Summarize with Java:
   ```
   cd java
   javac ReadingSummary.java
   java ReadingSummary ../data/readings.csv
   ```
5. Try the queries in `sql/useful_queries.sql`.

## Troubleshooting
See [docs/TROUBLESHOOTING.md](docs/TROUBLESHOOTING.md) for common faults and how to check them.

## What I learned
<!-- Write 3 to 4 honest lines in your own words: wiring, reading sensor data, handling bad readings, a fault you faced and fixed. -->

## Future improvements
- Send data over Wi-Fi with ESP32 (HTTP or MQTT)
- Simple web dashboard
- Email or SMS alerts
