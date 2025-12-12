# 🚀 STM32F103 + ADS1115 I2C Practice Project (USB CDC Output)

This project is a **practice exercise for learning I2C** on the STM32F103C8T6 “Blue Pill”.
We use an **ADS1115 16-bit ADC** to read **any analog sensor** (temperature sensor, potentiometer, voltage source, etc.).

Measurements are sent to the PC using **USB CDC (Virtual COM Port)** instead of UART, making the setup simple and clean.

---

## 🌟 Overview

This firmware demonstrates how to:

* Initialize **I2C1** using HAL
* Configure the **ADS1115** (MUX, PGA, mode, data rate)
* Read 16-bit conversion values
* Convert raw ADC output into voltage
* Send results over **USB CDC**

The goal is to gain hands-on experience with low-level I2C communication.

---

## 🛠️ Hardware

| Component                     | Description                             |
| ----------------------------- | --------------------------------------- |
| **STM32F103C8T6 "Blue Pill"** | Microcontroller (I2C Master + USB CDC)  |
| **ADS1115 Module**            | 16-bit ADC                              |
| **Analog Sensor**             | Temperature sensor, potentiometer, etc. |
| **USB Cable**                 | For power + CDC output                  |

---

## 🔌 Wiring

| ADS1115 Pin | Blue Pill Pin | Notes                      |
| ----------- | ------------- | -------------------------- |
| **VCC**     | 3.3V          | Power                      |
| **GND**     | GND           | Ground                     |
| **SCL**     | PB6           | I2C Clock                  |
| **SDA**     | PB7           | I2C Data                   |
| **ADDR**    | GND           | Sets slave address to 0x48 |
| **A0–A3**   | Sensor Input  | Connect your analog sensor |

Most ADS1115 boards already include I2C pull-up resistors.

---

## 💻 Tools Used

* **STM32CubeIDE**
* **HAL Drivers**
* **USB Device (CDC Class)**
* **ST-Link**
* **Serial Monitor** (Windows, Linux, macOS)

---

## ⚙️ How It Works

1. **I2C1** runs at 400 kHz.
2. The ADS1115 **config register** is written to start a single conversion.
3. The **conversion register** is read via I2C.
4. The result is converted to a voltage.
5. The formatted string is transmitted over **USB CDC** to the PC.

---

## 📝 Example Code Snippet

```c
uint8_t txbuf[64];
uint8_t data[3];
uint8_t raw[2];
int16_t adc;
float voltage;

#define ADS1115_ADDR (0x48 << 1)

// Configure ADS1115
data[0] = 0x01;  // Config register
data[1] = 0xC3;  // Example MSB config
data[2] = 0x83;  // Example LSB config

HAL_I2C_Master_Transmit(&hi2c1, ADS1115_ADDR, data, 3, HAL_MAX_DELAY);

// Request conversion register
data[0] = 0x00;
HAL_I2C_Master_Transmit(&hi2c1, ADS1115_ADDR, data, 1, HAL_MAX_DELAY);

// Read conversion result
HAL_I2C_Master_Receive(&hi2c1, ADS1115_ADDR, raw, 2, HAL_MAX_DELAY);

adc = (raw[0] << 8) | raw[1];
voltage = adc * (4.096f / 32768.0f);

// Send measurement over USB CDC
int len = sprintf((char*)txbuf, "ADC: %d, Voltage: %.4f V\r\n", adc, voltage);
CDC_Transmit_FS(txbuf, len);
```

---

## 🛣️ Future Improvements

* Continuous sampling mode
* Read all ADS1115 input channels
* Sampling using timer interrupts
* Filtering (moving average, exponential smoothing)
