# 📡 ESP32-C3 IR Proxy

An **ESPHome-based IR transmitter and receiver proxy** built around the **ESP32-C3 DevKitM-1**.

The device provides an IR transmitter and receiver to Home Assistant through ESPHome and `ir_rf_proxy`. It can be used
as a network-connected bridge for sending and receiving infrared remote-control signals.

The ESP32-C3 also provides **Bluetooth Proxy** functionality for Home Assistant.

---

It can be used with projects such as:

- 🔧 [HAIR](https://github.com/DAB-LABS/HAIR) — learn, manage and automate IR devices directly from Home Assistant
- 📚 [SmartIR](https://github.com/smartHomeHub/SmartIR) — use its database of device-specific IR command codes
- 🏠 Home Assistant automations and scripts
- 📡 Other software that can communicate with
  the [ESPHome IR proxy](https://github.com/esphome/infrared-proxies/blob/main/xiao-ir-mate/xiao-ir-mate.yaml)

## ✨ Features

* 📡 IR transmitter
* 📥 IR receiver
* 🔁 IR RF Proxy integration
* 🏠 Home Assistant integration
* 📶 Wi-Fi connectivity
* 🦋 Bluetooth Proxy
* 🔄 OTA firmware updates
* 🔐 Encrypted ESPHome API
* 📊 Wi-Fi signal diagnostics
* ⚡ ESP32-C3 based
* 🛜 Network-connected remote-control bridge

---

# 🧰 Hardware

## Controller

* **ESP32-C3 DevKitM-1**

## IR Transmitter

* IR LED
* 2N2222A NPN transistor
* 1 kΩ base resistor
* ~100 Ω current-limiting resistor
* 5 V supply

## IR Receiver

* VS1838B or compatible 38 kHz IR receiver

---

# 🔌 Wiring

## GPIO Map

| Component           | ESP32-C3 |
|---------------------|---------:|
| IR Transmitter      |    GPIO7 |
| IR Receiver OUT     |    GPIO6 |
| Sensor/Receiver VCC |    3.3 V |
| GND                 |      GND |

---

# 📡 IR Transmitter

The IR LED is driven using a **2N2222A transistor**.

```text
ESP32-C3 GPIO7
      │
     1kΩ
      │
      ▼
   2N2222A
   ┌───────┐
   │ Base  │
   │       │
   │Emitter├──────── GND
   │       │
   │Collector
   └───┬───┘
       │
       ▼
 IR LED cathode (-)

 IR LED anode (+)
       │
      ~100Ω
       │
       ▼
      +5V
```

### Connections

* **GPIO7 → 1 kΩ → transistor base**
* **Emitter → GND**
* **Collector → IR LED cathode (-)**
* **IR LED anode (+) → ~100 Ω → 5 V**

> ⚠️ Check the pinout of your specific 2N2222A transistor before wiring it.

---

# 📥 IR Receiver

The project uses a **VS1838B** IR receiver.

| VS1838B | ESP32-C3 |
|---------|----------|
| OUT     | GPIO6    |
| VCC     | 3.3 V    |
| GND     | GND      |

The receiver is configured as an inverted input with an internal pull-up.


---

# 🔁 IR Proxy

The main purpose of this project is to expose the IR transmitter and receiver as an **IR proxy**. The transmitter is
connected to the ESPHome remote transmitter. This allows Home Assistant to use the ESP32-C3 as a network-connected IR
proxy.

---

# 🏠 Home Assistant

Once connected to Home Assistant, the device exposes the IR proxy functionality together with its diagnostic entities.

The general architecture is:

```text
             Home Assistant
                    │
                    │ ESPHome API
                    ▼
             ┌──────────────┐
             │   ESP32-C3   │
             │  IR Proxy    │
             └──────┬───────┘
                    │
          ┌─────────┴─────────┐
          ▼                   ▼
    IR Transmitter        IR Receiver
       GPIO7                GPIO6
          │                   │
          ▼                   ▼
       IR LED              VS1838B
```

# 🛠️ Use Cases

Because this project exposes the IR hardware as a proxy rather than implementing a specific appliance protocol, it can
be used with different IR-controlled devices.

Examples include:

* TVs
* Set-top boxes
* Audio equipment
* Air conditioners
* Fans
* Media players
* Other consumer IR equipment

The ESP32-C3 itself does not need to know which device is being controlled. It provides the **IR transmit and receive
hardware**, while the higher-level control logic can be handled by Home Assistant or another compatible system.
The ESP32-C3 acts as the physical IR gateway. Hair, when used, provides the device-specific command definitions on the
Home Assistant side.
