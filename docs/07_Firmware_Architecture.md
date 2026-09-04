# STM32G4 Industrial Data Acquisition & Control Terminal

# PCB Layout and EMC Design Specification

**Version:** V1.0\
**Date:** 2026-09

------------------------------------------------------------------------

# 1. PCB Design Overview

## 1.1 Design Objective

The PCB design must convert the electrical design into a reliable
industrial product.

The main design goals:

-   Stable power delivery
-   Low noise analog acquisition
-   Reliable communication
-   Good EMC performance
-   Easy debugging and maintenance

PCB design is not only about making connections work.

A good PCB should also consider:

-   Current paths
-   Return paths
-   Noise coupling
-   Thermal performance
-   Manufacturing reliability

------------------------------------------------------------------------

# 2. PCB Stack-up Design

## 2.1 Four-Layer PCB Structure

Selected stack-up:

    Layer 1:

    Signal + Components


    Layer 2:

    Continuous GND Plane


    Layer 3:

    Power Plane


    Layer 4:

    Signal

------------------------------------------------------------------------

## 2.2 Why Use Four Layers?

Compared with two-layer PCB:

Advantages:

## Better Ground Reference

A dedicated GND plane provides:

-   Lower impedance return path
-   Reduced EMI radiation
-   Better signal integrity

## Easier Power Distribution

Power rails can be distributed using dedicated copper areas.

## Better Analog Performance

Sensitive ADC signals can reference a clean ground plane.

------------------------------------------------------------------------

# 3. PCB Functional Partition

## 3.1 Area Division

The PCB should be divided into:


    +--------------------------------+

    | Power Input Area               |

    | Protection + Buck Converter    |

    +--------------------------------+


    +----------------+---------------+

    | Analog Area    | MCU Area      |

    | ADC Frontend   | STM32G4       |

    +----------------+---------------+


    +--------------------------------+

    | Communication Area             |

    | RS485 / CAN                   |

    +--------------------------------+

------------------------------------------------------------------------

# 4. Power Layout Design

## 4.1 Buck Converter Layout

The switching regulator is the biggest noise source.

Critical components:

-   Input capacitor
-   Switching node
-   Inductor
-   Output capacitor

Layout principle:

    Input Capacitor

          |

    Switching Element

          |

    Inductor

          |

    Output Capacitor

Keep this current loop:

-   Short
-   Compact
-   Wide

------------------------------------------------------------------------

## 4.2 Switching Node Consideration

The SW node contains high-frequency voltage transitions.

Avoid routing:

-   ADC signals
-   Clock lines
-   Communication traces

near this area.

------------------------------------------------------------------------

# 5. Analog Layout Design

## 5.1 ADC Routing

Requirements:

-   Short signal path
-   Clean ground reference
-   Avoid digital noise

Recommended:

    Sensor Connector

            |

    Protection

            |

    Filter

            |

    ADC Pin

------------------------------------------------------------------------

## 5.2 Analog and Digital Ground

The design should avoid uncontrolled current flowing through analog
ground.

Important principle:

Sensitive analog signals require a clean return path.

------------------------------------------------------------------------

# 6. Communication Layout

# 6.1 RS485 Routing

Requirements:

-   Place protection near connector
-   Keep A/B traces together
-   Avoid power switching area

Structure:

    Connector

     |

    TVS

     |

    RS485 Transceiver

     |

    MCU

------------------------------------------------------------------------

# 6.2 CAN Routing

Requirements:

-   CAN_H and CAN_L together
-   Similar length
-   Avoid sharp routing changes

Recommended:

-   Differential routing
-   Short path
-   Proper termination placement

------------------------------------------------------------------------

# 7. Decoupling Capacitor Placement

## 7.1 MCU Decoupling

Every MCU power pin requires local bypass capacitors.

Typical:

    100nF

    +

    1uF

Placement:

As close as possible to MCU power pins.

------------------------------------------------------------------------

# 8. Thermal Consideration

Important heat sources:

-   Buck converter
-   Power MOSFET
-   Linear regulator

Design methods:

-   Increase copper area
-   Use thermal vias
-   Keep heat sources away from sensitive circuits

------------------------------------------------------------------------

# 9. DRC and Manufacturing Check

Before Gerber output:

Check:

## Electrical

-   Unconnected nets
-   Short circuits
-   Clearance

## Mechanical

-   Board outline
-   Connector position
-   Mounting holes

## Manufacturing

-   Minimum trace width
-   Via size
-   Silkscreen overlap

------------------------------------------------------------------------

# 10. Bring-up Related Test Points

Recommended test points:

  Point       Purpose
  ----------- ---------------------------
  VIN         Input voltage measurement
  5V          Buck output
  3V3         MCU supply
  UART        Debug
  RS485 A/B   Communication testing
  CAN_H/L     CAN testing

------------------------------------------------------------------------

# 11. PCB Verification Plan

## Before Power On

Check:

-   Resistance between power and GND
-   Component polarity
-   Connector orientation

## First Power On

Use:

-   Current limited power supply

Measure:

-   5V
-   3.3V
-   MCU current consumption

------------------------------------------------------------------------

# 12. Interview Notes

## Q1:

Why use a four-layer PCB instead of two layers?

Answer:

A four-layer PCB provides dedicated power and ground planes, improving
signal integrity and reducing EMI.

------------------------------------------------------------------------

## Q2:

Why should the Buck converter be separated from ADC?

Answer:

The switching node generates high-frequency noise that can affect
sensitive analog measurements.

------------------------------------------------------------------------

## Q3:

Why place decoupling capacitors close to MCU pins?

Answer:

Because high-frequency transient current requires a low-impedance local
path.

------------------------------------------------------------------------

# Version History

  Version   Date      Description
  --------- --------- ------------------------------------------
  V1.0      2026-09   Initial PCB layout and EMC specification

CPU resources are occupied by continuous sampling.

With DMA:

    ADC

     |

    DMA

     |

    Memory Buffer

     |

    CPU Processing

Advantages:

-   Lower CPU load
-   Stable sampling frequency
-   Higher efficiency

------------------------------------------------------------------------

# 6. Data Filtering

Industrial sensors may contain noise.

Software filtering methods:

## Moving Average Filter

Example:

    Average = (Sample1 + Sample2 + ... + SampleN) / N

Advantages:

-   Simple implementation
-   Good noise reduction

------------------------------------------------------------------------

# 7. Communication Software Design

## 7.1 Modbus RTU

Frame:

    Address

    Function

    Data

    CRC

Software process:

    Receive

    ↓

    Parse

    ↓

    Verify CRC

    ↓

    Execute

    ↓

    Reply

------------------------------------------------------------------------

## 7.2 CAN Communication

Software flow:

    CAN Interrupt

            |

    Receive Buffer

            |

    Message Processing

            |

    Application

------------------------------------------------------------------------

# 8. Parameter Storage Design

Industrial equipment requires parameter retention after power off.

Stored parameters:

-   Calibration values
-   Device address
-   Communication settings
-   User configuration

Storage options:

-   Internal Flash
-   External EEPROM

Example:

    Parameter

    {

    Device_ID

    Baudrate

    Calibration_Value

    }

------------------------------------------------------------------------

# 9. Bootloader Upgrade Design

## Purpose

Allow firmware upgrade without physical programming tools.

Process:

    PC Software

          |

    UART / CAN

          |

    Bootloader

          |

    Application Firmware

Bootloader functions:

-   Firmware verification
-   Flash erase
-   Firmware writing
-   Jump to application

------------------------------------------------------------------------

# 10. Error Handling

Industrial systems require fault management.

Possible faults:

-   Sensor disconnected
-   Communication timeout
-   Power abnormality
-   ADC overflow

Software response:

    Detect Error

          |

    Record Error

          |

    Notify System

          |

    Recover

------------------------------------------------------------------------

# 11. Debugging Plan

Tools:

-   ST-Link
-   UART terminal
-   Logic analyzer
-   CAN analyzer

Debug sequence:

1.  Confirm MCU startup
2.  Verify clock
3.  Test UART
4.  Test ADC
5.  Test RS485
6.  Test CAN

------------------------------------------------------------------------

# 12. Firmware Verification Plan

  Function       Test Method
  -------------- ---------------------------
  ADC Sampling   Input known voltage
  RS485          Modbus communication test
  CAN            CAN analyzer
  Storage        Power cycle test
  Bootloader     Firmware upgrade test

------------------------------------------------------------------------

# 13. Interview Notes

## Q1:

Why use FreeRTOS in this project?

Answer:

FreeRTOS provides task scheduling and improves software modularity when
multiple functions such as ADC acquisition and communication need to run
simultaneously.

------------------------------------------------------------------------

## Q2:

Why use ADC DMA instead of polling?

Answer:

DMA reduces CPU load and provides stable high-frequency data
acquisition.

------------------------------------------------------------------------

## Q3:

Why design a bootloader?

Answer:

Industrial equipment often requires firmware updates after deployment.
Bootloader allows software maintenance without physical programming
access.

------------------------------------------------------------------------

# Version History

  Version   Date      Description
  --------- --------- ---------------------------------------------
  V1.0      2026-09   Initial firmware architecture specification
