---
title: I2C with STM32
date: 2024/01/09
description: Game engine Test 
tag: graphics, day project
author: Matia Choi
---

## Introduction

I wrote a driver for the BMP180 sensor which is a precise digital barometric pressure and temperature sensor designed by Bosch Sensortec. It communicates over I2C, offering accurate pressure measurements within a range of 300 to 1100 hPa and temperature readings from -40°C to +85°C. With low power consumption, factory-calibrated coefficients, and a compact form factor, it's widely used in applications like weather forecasting, altimeters, and drones, where precise environmental data is essential.

I wrote this driver code for my STM32 microcontroller which I intend to create a drive logger in the future to track/log where I drive. One of the main reasons why I wanted to write a driver from scratch and use an STM32 microcontroller was because I wanted to move away from Arduinos and move onto something that "felt more professional". I felt like using an Arduino was kind of cheating with so many easy libraries I could use. It didn't help me understand how everything worked behind the scenes. 

## How I2C works

**Two-Wire Serial Interface:** I2C uses only two wires, SDA (Serial Data Line) and SCL (Serial Clock Line), to communicate between devices. Each line has its own pull-up resistor to keep the line active high. One device acts as the master (initiates and controls the communication) and the other as the slave (responds to the master).

**Addressing:** Each slave device has a unique address. When the master wants to communicate with a slave, it sends out the address of that device along with a bit indicating whether the operation is a read or write.

**Start and Stop Conditions:** Communication is initiated by a start condition, where the SDA line transitions from high to low while SCL is high. It ends with a stop condition, where SDA transitions from low to high while SCL is high.

**Clock Signal:** The SCL line is used by the master to synchronize data transmission. Data can only change when SCL is low and is read or sampled when SCL is high.

**Data Transfer:** Data is transferred in 8-bit packets. After each packet, an acknowledgment (ACK) bit is used. If the receiver successfully receives the 8-bit data, it sends back an ACK bit (pulls SDA low). If not, a NACK (non-acknowledgment) is sent (SDA remains high).

## Looking at the BMP180 Datasheet

I started by reading through the BMP180 datasheet which consisted of electrical characteristics, absolute maximum rating, operation, global memory map, I2C interface, package, and legal disclaimer. The ones we're really interested in are the operation, global memory map, and I2C interface.

## Writing the header file


I began with defining the I2C address of the BMP180 sensor, and all the memory addresses. Going to the I2C section shows device address, and the global memory map shows us all the data registers and their addresses. Which I have defined in my header files. 
```c
#define BMP180_REG_OUT_XLSB 0xF8
#define BMP180_REG_OUT_LSB 0xF7
#define BMP180_REG_OUT_MSB 0xF6
#define BMP180_REG_CTRL_MEAS 0xF4
#define BMP180_REG_SOFT_RESET 0xE0
#define BMP180_REG_ID 0xD0
#define BMP180_REG_CALIB_21 0xBF
#define BMP180_REG_CALIB_0 0xAA
```

I also defined an enum to organize the control register values which are used to decide if you want to read a temperature value or a pressure value with various accuracy levels.

```c
typedef enum {
    CTRL_REG_TEMPERATURE = 0x2E,
    CTRL_REG_PRESSURE_OSS_0 = 0x34,  // ultra low power
    CTRL_REG_PRESSURE_OSS_1 = 0x74,  // standard
    CTRL_REG_PRESSURE_OSS_2 = 0xB4,  // high res
    CTRL_REG_PRESSURE_OSS_3 = 0xF4,  // ultra high res
} CONTROL_REGISTER;
```

Then, I started defining the functions.


## Writing the source file