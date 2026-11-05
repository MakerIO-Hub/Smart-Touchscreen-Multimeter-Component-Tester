# 🚀 PicoMeter Touch — Smart Touchscreen Multimeter & Component Tester

**Developer:** MAKERIO  
**Platform:** Raspberry Pi Pico / Pico 2 (RP2040 / RP2350)  
**Programming Language:** MicroPython  

---

## 📌 Project Overview

**PicoMeter Touch** is a modern digital multimeter and component tester featuring a color touchscreen. Designed for both desktop lab use and portable field testing (powered by an internal 3S LiPo battery), it combines standard multimeter functions with smart features such as **automatic LiPo battery cell detection**, **auto-ranging component measurements**, and an **animated screen saver**.

```
       ┌────────────────────────────────────────────────────────┐
       │                PicoMeter Touch Dashboard               │
       │                                                        │
       │   [ DC VOLT ]        [ COMPONENT ]      [ CURRENT ]    │
       │   0–30V DC           Resistor / Diode   0–5A / Watt    │
       │   Min/Max Memory     LED / Buzzer       ADS1115 + ACS  │
       └────────────────────────────────────────────────────────┘
```

---

## 🔗 Links & Project Resources

* 📺 **YouTube Video:** [Watch the Full Build & Testing Video](Coming Soon)
* 💻 **Source Code & Schematics:** [GitHub Repository](https://github.com/MakerIO-Hub/Smart-Touchscreen-Multimeter-Component-Tester)
* 🛠️ **3D Enclosure Files (STL):** [Download via Google Drive](Coming Soon)

---

## 🌟 Key Features

1. **0–30V DC Voltmeter:**
   * High-precision voltage measurement using the 16-bit **ADS1115 ADC**.
   * Real-time **Min / Max** voltage recording memory.
2. **Smart LiPo Battery Analyzer:**
   * Automatic cell count (1S to 6S) and remaining capacity percentage ($0\% - 100\%$) calculation for Lithium-based batteries.
3. **Current & Power Meter:**
   * Instantaneous DC current ($A$) and power consumption ($W$) calculation using the **ACS712-05B** sensor.
4. **Component Test Menu (4 Main Modes):**
   * **Resistor Measurement:** Automatic unit scaling ($\Omega$, $k\Omega$, $M\Omega$).
   * **Diode Test:** Forward voltage drop ($V_f$) detection.
   * **LED Test:** Measures LED voltage drop and estimates color/type.
   * **Continuity Test:** Audible buzzer alert for short circuits below $40\Omega$.
5. **HOLD Function:** Freeze the current reading on screen with a single touch.
6. **Animated Screen Saver:** Enters a cute blinking eye animation mode after 60 seconds of inactivity.

---

## 🛠️ Bill of Materials (BOM)

| Category | Component / Model | Qty |
| :--- | :--- | :---: |
| **Microcontroller** | Raspberry Pi Pico or Pico 2 (RP2040 / RP2350) | 1 |
| **Display & Touch** | 2.4" or 2.8" ILI9341 SPI TFT LCD + XPT2046 Touch (320x240) | 1 |
| **ADC** | ADS1115 16-Bit 4-Channel I2C ADC Module | 1 |
| **Current Sensor** | ACS712-05B (5A DC Current Sensor Module) | 1 |
| **Buzzer** | 3-Pin Active Buzzer Module | 1 |
| **Status LEDs** | 5mm or 3mm LEDs (Red, Green, Blue) | 3 |
| **Voltage Divider** | 100k$\Omega$ (1% metal film) & 10k$\Omega$ (1% metal film) | 1 each |
| **Reference Resistor** | 1k$\Omega$ (1% metal film) | 1 |
| **LED Resistors** | 220$\Omega$ or 330$\Omega$ | 3 |
| **Power & Probes** | Test Probes, Breadboard, Jumper Wires | — |

---

## 📌 Pinout & GPIO Connection Tables

> ⚠️ **IMPORTANT NOTE (Pico 2 Compatibility):**  
> To prevent potential hardware pin conflicts on the Pico 2, I2C data lines in this project are assigned to **GP16 (SDA)** and **GP17 (SCL)** instead of GP4/GP5.

### 1. ILI9341 LCD & XPT2046 Touchscreen (Shared SPI1 Bus)

| LCD / Touch Pin | Pico GPIO | Physical Pin No | Description |
| :--- | :--- | :---: | :--- |
| **VCC** | 3.3V | Pin 36 | Display Power |
| **GND** | GND | Pin 18 | Ground |
| **CS (LCD)** | GP8 | Pin 11 | LCD Chip Select |
| **RESET** | GP13 | Pin 17 | Hardware Reset |
| **DC (D/C)** | GP9 | Pin 12 | Command / Data Select |
| **MOSI (SDI)** | GP11 | Pin 15 | SPI1 Data Out (Shared) |
| **SCK (CLK)** | GP10 | Pin 14 | SPI1 Clock Line (Shared) |
| **LED (BL)** | GP14 | Pin 19 | Backlight Control (PWM) |
| **MISO (SDO)** | GP12 | Pin 16 | SPI1 Data In (Shared) |
| **T_CS** | GP15 | Pin 20 | Touch Chip Select |
| **T_CLK** | GP10 | Pin 14 | Connected parallel to LCD SCK |
| **T_DIN** | GP11 | Pin 15 | Connected parallel to LCD MOSI |
| **T_DO** | GP12 | Pin 16 | Connected parallel to LCD MISO |
| **T_IRQ** | — | — | *Unused* |

### 2. ADS1115 16-Bit I2C ADC Module

| ADS1115 Pin | Pico GPIO / Line | Physical Pin No | Description |
| :--- | :--- | :---: | :--- |
| **VDD** | 3.3V | Pin 36 | 3.3V Power Supply |
| **GND** | GND | Pin 38 / Pin 23 | Ground |
| **SDA** | GP16 | Pin 21 | I2C0 Data Line |
| **SCL** | GP17 | Pin 22 | I2C0 Clock Line |
| **ADDR** | GND | Pin 38 / Pin 23 | Address select: `0x48` |
| **ALRT** | — | — | *Unused* |

### 3. Status Indicator LEDs

| LED Color | Mode / Function | Pico GPIO | Physical Pin | Series Resistor |
| :--- | :--- | :---: | :---: | :--- |
| **Red LED** | DC Voltmeter Mode Indicator | GP2 | Pin 4 | 220$\Omega$ / 330$\Omega$ $\rightarrow$ GND |
| **Green LED** | Component Tester Mode | GP3 | Pin 5 | 220$\Omega$ / 330$\Omega$ $\rightarrow$ GND |
| **Blue LED** | Current & Power Mode | GP1 | Pin 2 | 220$\Omega$ / 330$\Omega$ $\rightarrow$ GND |

### 4. Buzzer & ACS712 Current Sensor

| Module | Module Pin | Pico Connection | Description |
| :--- | :--- | :--- | :--- |
| **Active Buzzer** | **S (Signal)** | GP22 (Pin 29) | Audio Trigger Line |
| | **+ (VCC)** | VSYS / 5V (Pin 39) | 5V Power Input |
| | **- (GND)** | GND (Pin 28 / 38) | Ground |
| **ACS712 (5A)** | **VCC** | VSYS / 5V (Pin 39) | 5V Power Input |
| | **GND** | GND (Pin 38) | Ground |
| | **OUT** | ADS1115 (A1) | Analog Current Output |

---

## 📐 Measurement Circuits & Schematics

### 1. 0–30V DC Voltmeter Circuit (ADS1115 Channel A0)

An **11:1 voltage divider** ratio is used to scale higher input voltages down to safe levels for the ADC.

```
Red Probe (+) ───[ 100kΩ Resistor ]───┬───> ADS1115 A0 Pin
(Measured + Voltage)                   │
                                [ 10kΩ Resistor ]
                                       │
Black Probe (-) ───────────────────────┴───> Pico GND (Pin 38)
(Measured - Ground)
```

* **Wiring Details:** 
  * Red Probe (+) $\rightarrow$ Connected to the open terminal of the 100k$\Omega$ resistor.
  * Node between 100k$\Omega$ and 10k$\Omega$ resistors $\rightarrow$ Connected to ADS1115 **A0** input.
  * Free end of 10k$\Omega$ resistor and Black Probe (-) $\rightarrow$ Connected to common system ground (GND).

---

### 2. Component Test Circuit (ADS1115 Channel A2)

Uses a **1k$\Omega$ pull-up** reference resistor connected to the 3.3V rail for testing passive components and semiconductors.

```
Pico 3.3V (Pin 36) ───[ 1kΩ Reference ]───┬───> ADS1115 A2 Pin
                                           │
                                           └───> Red Test Probe (+)

Pico GND (Pin 38)  ────────────────────────────> Black Test Probe (-)
```

* **Working Principle:** Connecting a component between the probes creates a voltage drop read via channel **A2**. The firmware automatically calculates resistance ($\Omega$) or forward voltage drop ($V_f$). Direct probe contact triggers the continuity buzzer instantly.

---

### 3. Status LED Wiring Schematic

```
Pico GP2 (Pin 4) ───[ (+) Red LED   (-) ]───[ 220Ω Resistor ]───> GND
Pico GP3 (Pin 5) ───[ (+) Green LED (-) ]───[ 220Ω Resistor ]───> GND
Pico GP1 (Pin 2) ───[ (+) Blue LED  (-) ]───[ 220Ω Resistor ]───> GND
```

---

## 🔋 Portable Power & Battery Management System (3S LiPo)

PicoMeter Touch incorporates an onboard **3S Li-Ion/LiPo** battery management circuit for standalone handheld operation.

### 🔌 Power Hardware:
* **Battery Pack:** 3x 18650 Li-Ion Cells in series (~11.1V Nominal, 12.6V Fully Charged)
* **BMS Board:** 3S 40A Balanced Protection Board (Overcharge, overdischarge, short-circuit protection)
* **Charge Port:** Standard 5.5x2.1mm DC Female Jack
* **Charger:** 12.6V 1A/2A CC/CV Li-Ion Charger
* **Buck Converter:** LM2596 or Mini DC-DC Step-Down Module ($12.6V \rightarrow 5.0V$ Fixed Output)
* **Power Switch:** Rocker / Toggle Switch

### ⚡ Power Distribution & Charging Circuit Diagram

```
             [ 12.6V Charger Adapter ]
                        │
                        ▼
           [ 5.5x2.1mm DC Jack Input ]
                        │
                        ├──────────────────────────────────────────┐
                        │                                          │
                        ▼ (+)                                      ▼ (-)
                 ┌──────────────┐                           ┌──────────────┐
                 │   BMS (3S)   │                           │   BMS (3S)   │
                 │    B+ / P+   │                           │    B- / P-   │
                 └──────┬───────┘                           └──────┬───────┘
                        │                                          │
       [ 3S Battery Pack: 9.6V - 12.6V ]                          │
                        │                                          │
                  [ Power Switch ]                                 │
                        │                                          │
                        ▼ (+)                                      ▼ (-)
              ┌───────────────────────┐                            │
              │   DC-DC Step-Down     │                            │
              │   (12.6V -> 5.0V)     │                            │
              └─────────┬─────────────┘                            │
                        │                                          │
        +5.0V Regulated │                                          │ Common GND
        Clean Output    │                                          │
                        ├─────────────────┬────────────────────────┤
                        ▼                 ▼                        ▼
                   Pico VSYS         ACS712 VCC               Buzzer VCC
                   (Pin 39)          & Buzzer (+)             Common GND (Pin 38)
```

### ⚠️ Critical Safety & Hardware Rules:

1. **Never Feed 12.6V Directly to the Pico!**  
   The Raspberry Pi Pico `VSYS` pin accepts up to **5.5V** maximum. The ~12V voltage from the 3S battery **must** be regulated down to **5.0V** using a DC-DC Buck converter before connecting to `VSYS` (Pin 39).
2. **Sensor Supply (5V Rail):**  
   The ACS712 current sensor and Active Buzzer require a 5V supply for optimal performance. Connect their $VCC$ pins to the 5.0V Buck converter output.
3. **USB and Battery Dual-Power Safety:**  
   The Pico includes an onboard Schottky diode. Connecting a USB cable while powered by the battery is safe, though switching off battery power during firmware flashing is recommended.
4. **On-Screen Battery Monitoring:**  
   When measuring its own battery in DC VOLT mode, the firmware automatically identifies the battery configuration and displays real-time health metrics:  
   `Battery: 85% (3S LiPo)`

---

## 💻 Setup and Software Installation

### 1. MicroPython Firmware Installation
1. Press and hold the **BOOTSEL** button on your Raspberry Pi Pico while connecting it to your computer via USB.
2. Drag and drop the latest MicroPython `.uf2` firmware file onto the mounted drive.

### 2. Uploading Project Files
1. Open **Thonny IDE** and set the interpreter to `MicroPython (Raspberry Pi Pico)`.
2. Upload the project files directly to the Pico's root directory (`/`):
   * `main.py` — Main entry point and mode management loop
   * `ui.py` — Graphical user interface & touch event handling
   * `ads1115.py` — ADS1115 I2C ADC driver & mathematical conversions
   * `ili9341.py` — ILI9341 TFT display SPI driver
   * `xpt2046.py` — Touchscreen SPI controller driver
   * `notifier.py` — LED and audio feedback control layer

### 3. Running the Project
* Open `main.py` in Thonny and press **F5 (Run)**. The screen will display the splash sequence **"developed by MAKERIO"** and launch the measurement dashboard.

---

## 📖 Operating Instructions

* **Switching Modes:** Tap the touch buttons at the bottom of the screen:
  * **DC VOLT:** 0–30V Voltage measurement & automatic LiPo cell detection.
  * **COMPONENT:** Accesses Resistor, LED, Diode, and Continuity tests.
  * **CURRENT:** Displays real-time DC Amperes ($A$) and Power ($W$).
* **Data Hold (HOLD):** Tap the **HOLD** button in the top-right corner of the screen to lock the active reading.
* **Auto Sleep (Screensaver):** If inactive for 60 seconds, the device activates an animated blinking eye screen saver to conserve power. Tap anywhere on the screen to wake it up instantly.
