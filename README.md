# MPU6050 STM32 HAL Driver

Lightweight C driver for the MPU6050 (GY-521) 6-axis IMU with built-in Kalman filter for angle estimation. Designed for STM32 microcontrollers using the HAL library.

---

## Features

- MPU6050 initialization and configuration
- Read raw and scaled accelerometer + gyroscope values
- Read temperature from sensor
- Built-in Kalman filter for angle fusion (roll, pitch)
- Simple one-call data update (`MPU6050_Read_All`)
- Designed for STM32 HAL (`I2C_HandleTypeDef`)

---

## Hardware Requirements

- STM32 microcontroller (tested on STM32F4/F1 series)
- MPU6050 module (GY-521)
- I2C communication

**Default I2C Address:** `0xD0` (8-bit format, AD0 = GND)

---

## Connections (Example STM32 → MPU6050)

| MPU6050 Pin | STM32 Pin Example |
|--------------|------------------|
| VCC          | 3.3V            |
| GND          | GND             |
| SCL          | PB6 (I2C1_SCL)  |
| SDA          | PB7 (I2C1_SDA)  |
| AD0          | GND (addr 0x68) |

---

## Add to Project

Import these two files into your STM32 project. 
Add include:
#include "mpu6050.h"
Add inside main() before the while loop:

extern I2C_HandleTypeDef hi2c1;  // replace with your I2C
MPU6050_t MPU6050;

if (MPU6050_Init(&hi2c1, 0xD0) != 0) {
    // Initialization failed
    Error_Handler();
}

Read Data in Loop

while (1)
{
    MPU6050_Read_All(&hi2c1, &MPU6050, 0xD0);
    HAL_Delay(10); // 100 Hz refresh
}

Print Data via UART

printf("Ax=%.2f Ay=%.2f Az=%.2f | AngleX=%.2f AngleY=%.2f\r\n",
       MPU6050.Ax, MPU6050.Ay, MPU6050.Az,
       MPU6050.KalmanAngleX, MPU6050.KalmanAngleY);

