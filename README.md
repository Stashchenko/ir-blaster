# 🌡️ ESP32-C3 IR Blaster (Samsung AC + Sensors)

This project is an ESPHome-based smart IR blaster built on an **ESP32-C3**, designed to control a Samsung air conditioner and monitor room conditions using sensors.

It includes:
- 📡 IR transmitter (Samsung AC control via heatpumpir)
- 📥 IR receiver (signal learning / debugging)
- 🌡️ AHT20 temperature + humidity sensor
- 🌬️ BMP280 pressure sensor
- 📶 WiFi + Home Assistant integration

---

# 🧰 Hardware

## Controller
- ESP32-C3 DevKitM-1

## Sensors
- AHT20 (temperature + humidity)
- BMP280 (pressure)

## IR system
- IR LED (transmitter)
- NPN transistor (2N2222A)
- IR receiver (VS1838B)

---

# 🔌 Wiring

## ESP32-C3 GPIO map

| Component | Pin |
|----------|-----|
| AHT20 / BMP280 (I2C SDA) | GPIO4 |
| AHT20 / BMP280 (I2C SCL) | GPIO5 |
| IR Transmitter (signal) | GPIO7 |
| IR Receiver (OUT) | GPIO6 |
| GND | GND |
| VCC (sensors) | 3.3V |

---

## 📡 IR Transmitter wiring
- ESP32 GPIO7 → 1KΩ resistor → base (2N2222A)
- Emitter → GND
- Collector → IR LED cathode (-)
- IR LED anode (+) → 5V with resistor ~100Ω

---

## 📥 IR Receiver (VS1838B)

| Pin | Connection |
|----|------------|
| OUT | GPIO6 |
| VCC | 3.3V |
| GND | GND |

👉 If pins are not labeled:
- Longer leg = VCC
- Middle = OUT
- Short leg = GND

---

# 🧠 Firmware

Built using **ESPHome (ESP-IDF framework)**.

---

## Features

### 🌡️ Sensors
- AHT20 temperature + humidity
- BMP280 pressure sensor
- Software calibration offsets (via Home Assistant)

### 📡 IR Control
- Samsung AC control via:
  - `heatpumpir`
- Protocol tested:
  - `samsung_aqv`

### 📥 IR Learning
- Remote receiver enabled
- Logs raw IR signals in Pronto format

---

# ❄️ Climate Control

Samsung AC integration:

```yaml
climate:
  - platform: heatpumpir
    name: "Samsung AC"
    transmitter_id: ir_tx
    protocol: samsung_aqv
    sensor: room_temperature
    min_temperature: 16
    max_temperature: 30
    vertical_default: middle
    horizontal_default: middle
