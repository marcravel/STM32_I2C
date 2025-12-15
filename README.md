# 🚀 STM32F103 + ADS1115 I2C Practice Project (USB CDC Output)

This project is a **practical I2C learning implementation** using the STM32F103C8T6 “Blue Pill” as the master and the **ADS1115 16-bit ADC** as the slave.

The system reads **two analog sensors**, performs **on-device conversion to physical units**, and transmits **ready-to-use values** to the PC via **USB CDC (Virtual COM Port)**.

---

## 🌟 Project Status

✅ I2C communication stable
✅ ADS1115 configuration validated
✅ Correct channel selection per sensor
✅ **Temperature converted to °C on the STM32**
✅ **Potentiometer converted to resistance (Ω) on the STM32**
✅ USB CDC output working
✅ No UART required

All transmitted values are **physical quantities**, not raw ADC counts.

---

## 🛠️ Hardware

| Component                     | Description                |
| ----------------------------- | -------------------------- |
| **STM32F103C8T6 (Blue Pill)** | I2C master, USB CDC device |
| **ADS1115**                   | 16-bit I2C ADC             |
| **Temperature Sensor**        | Analog temperature sensor  |
| **Potentiometer**             | Variable resistor input    |
| **USB Cable**                 | Power + CDC communication  |

---

## 🔌 Wiring

| ADS1115 Pin | Blue Pill Pin | Notes                         |
| ----------- | ------------- | ----------------------------- |
| **VDD**     | 3.3V          | Powered from STM32            |
| **GND**     | GND           | Common ground                 |
| **SCL**     | PB6           | I2C clock                     |
| **SDA**     | PB7           | I2C data                      |
| **ADDR**    | GND           | I2C address set to `0x48`     |
| **A1**      | Temp sensor   | Temperature measurement input |
| **A3**      | Potentiometer | Resistance measurement input  |

---

## 💻 Tools Used

* **STM32CubeIDE**
* **STM32 HAL Drivers**
* **USB Device (CDC class)**
* **ST-Link v2**
* **PC Serial Monitor**

---

## ⚙️ How It Works

1. The STM32 initializes **I2C1** as the master interface.
2. The ADS1115 is configured for **single-shot measurements**.
3. Each sensor is selected using the **input multiplexer (MUX)**.
4. The ADC conversion result is read from the ADS1115.
5. **Raw ADC values are converted on the STM32**:

   * Voltage → **Temperature (°C)**
   * Voltage → **Resistance (Ω)**
6. The computed physical values are sent to the PC via **USB CDC**.

The PC receives **human-readable measurements**, requiring no further processing.

---

## 📤 Output

The serial output displays:

* **Temperature in degrees Celsius (°C)**
* **Potentiometer resistance in ohms (Ω)**

Values are accurate, stable, and correctly associated with their respective sensors.

---

## 🛣️ Future Improvements

* Continuous conversion mode
* Multi-channel scanning with timestamps
* Digital filtering (moving average / exponential)
* Calibration offsets and gain correction
* Data logging on the PC side

---

This project now serves as a **clean reference implementation** for:
**STM32 + ADS1115 + I2C + physical-unit conversion + USB CDC output**.
