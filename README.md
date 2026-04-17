# Ultimate ESPHome Soil Moisture Node 🌿

A professional, high-accuracy, rust-free ESPHome configuration for capacitive soil moisture sensors.

## ✨ Features

- **No Sensor Degradation**: Uses dynamic power switching to power the sensor *only* exactly during measurement, extending its life to years.
- **Hardware-Free Calibration**: Calibrate your DRY and WET values entirely from Home Assistant using an interactive, automated timer sequence. No serial console required!
- **Temperature Compensation**: Incorporates an advanced compensation algorithm using a DS18B20 temperature sensor to adjust moisture readings based on soil temperature variations.
- **16-bit Precision**: Uses an ADS1115 external ADC for vastly superior measurement stability compared to the noisy internal ESP32 ADC.
- **Flash Memory Save**: Calibration values are stored in the ESP32 flash and persist across reboots.

## 🛠️ Hardware Requirements

- ESP32 Development Board
- Capacitive Soil Moisture Sensor (v1.2 or v2.0)
- DS18B20 Waterproof Temperature Sensor (Requires a 4.7kΩ pull-up resistor)
- ADS1115 16-Bit I2C ADC Module

## 🔌 Wiring Guide

* **I2C / ADS1115:** Connect SCL to `GPIO22`, SDA to `GPIO21`. Connect the Soil Sensor analog output (AOUT) to `A0` on the ADS1115.
* **DS18B20 Temp Sensor:** Connect Data pin to `GPIO5`. *(Requires a 4.7kΩ pull-up resistor between Data and 3.3V)*.
* **Calibration Button:** This configuration uses the built-in "BOOT" button on your ESP32 (`GPIO0`)!

### 🔋 Sensor Power Management (Crucial for 0% corrosion & noise reduction)
The code turns `GPIO4` on and off to supply power ONLY when measuring. Choose one of the following methods to safely switch power:
1. **Direct (Minimalist / Hardware-Free):** Connect Sensor VCC directly to `GPIO4` (Safe for standard capacitive sensors using ~5mA).
2. **Logic-Level MOSFET (Modern):** Use an N-Channel MOSFET (e.g., 2N7000, AO3400). Gate to `GPIO4`, Source to GND, Drain to Sensor GND.
3. **LDO Regulator (Industrial Pro):** Use a 3.3V LDO with an Enable (EN) pin. Connect `GPIO4` to EN for perfect, noise-free hardware-isolated power.

## 🚀 Installation

1. Create a new Node in ESPHome.
2. Copy the contents of `ultimate_soil_sensor.yaml` into your ESPHome editor.
3. Update your `ssid` and `password` or point to your `!secrets`.
4. Install/Flash the code to your ESP32.

## 🎯 Automatic Calibration Process

This project introduces a fully automated, interactive calibration process visible directly in your Home Assistant dashboard!

**From Home Assistant:**
1. Navigate to your Device dashboard.
2. Click the `Start Calibration Sequence` button.
3. The `Calibration Status` text sensor will dynamically guide you through the process in real-time.
4. Leave the sensor in the **AIR** for the first 20 seconds.
5. Place the sensor in a glass of **WATER** and wait another 30 seconds as instructed on screen.
6. Done! The calibration is automatically applied and saved into the ESP32's flash memory permanently!

**From the Node (Physical):**
Double click the `BOOT` button on your ESP32 board to trigger the exact same automatic timer calibration sequence!
