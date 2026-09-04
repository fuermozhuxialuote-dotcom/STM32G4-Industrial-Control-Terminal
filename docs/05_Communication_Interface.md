# STM32G4 Industrial Data Acquisition & Control Terminal

# Communication Interface Design Specification

**Version:** V1.0\
**Date:** 2026-09

------------------------------------------------------------------------

# 1. Communication Architecture Overview

## 1.1 Design Objective

Industrial control systems require reliable communication between:

-   Sensors
-   Controllers
-   PLC systems
-   Upper computer software
-   Other control nodes

This project supports:

-   RS485 communication
-   CAN communication
-   UART debugging interface

Communication architecture:

                     STM32G4

                        |

            ----------------------------

            |                          |

           UART                      FDCAN

            |                          |

         RS485                       CAN

            |                          |

     Industrial Device          CAN Network

                        |

                  PC Monitoring Tool

------------------------------------------------------------------------

# 2. RS485 Interface Design

# 2.1 Why Use RS485

UART communication is simple but has limitations:

    MCU UART

    TX/RX

    Short distance communication

Problems:

-   Poor noise immunity
-   Limited transmission distance

Industrial environments usually contain:

-   Motors
-   Relays
-   Inverters
-   Switching power supplies

Therefore differential communication is preferred.

RS485 advantages:

-   Differential transmission
-   Strong noise immunity
-   Long-distance communication
-   Multi-node capability

------------------------------------------------------------------------

# 2.2 RS485 Hardware Structure

Architecture:

    STM32 UART

          |

    RS485 Transceiver

          |

    Protection Circuit

          |

    A/B Differential Bus

          |

    Industrial Equipment

Main components:

## RS485 Transceiver

Example:

MAX3485

Function:

Convert:

    UART Signal

    ↓

    Differential RS485 Signal

------------------------------------------------------------------------

# 2.3 Half Duplex Control

RS485 usually uses half duplex communication.

The transceiver includes:

-   Driver Enable (DE)
-   Receiver Enable (RE)

Communication process:

    Transmit:

    MCU

     |

    Enable Driver

     |

    Send Data


    Receive:

    Disable Driver

     |

    Enable Receiver

     |

    Read Data

The MCU controls DE/RE through GPIO.

------------------------------------------------------------------------

# 2.4 RS485 Protection Design

Industrial interfaces require protection.

Included:

## TVS Protection

Purpose:

Protect against:

-   ESD
-   Surge voltage
-   Cable transient

Placement:

    Connector

     |

    TVS

     |

    RS485 Transceiver

Protection should be close to the external connector.

------------------------------------------------------------------------

## Termination Resistor

RS485 uses differential transmission.

For long cables:

A 120Ω termination resistor is added:

    A --------120Ω-------- B

Purpose:

Reduce signal reflection.

------------------------------------------------------------------------

## Bias Resistor

When no device is transmitting:

The bus state may become undefined.

Bias resistors provide a default state.

------------------------------------------------------------------------

# 3. Modbus RTU Communication

## 3.1 Communication Model

Typical structure:

    Master

     |

    RS485 Bus

     |

    Slave 1

     |

    Slave 2

     |

    Slave 3

This project can act as:

-   Master controller
-   Slave sensor node

------------------------------------------------------------------------

# 3.2 Modbus RTU Frame

Frame structure:

    Address

    Function Code

    Data

    CRC Check

CRC is used to verify communication reliability.

------------------------------------------------------------------------

# 3.3 Software Flow

Transmit:

    Prepare Frame

    ↓

    Calculate CRC

    ↓

    Enable RS485 Driver

    ↓

    Send UART Data

    ↓

    Disable Driver

Receive:

    Receive UART Data

    ↓

    Check Address

    ↓

    Verify CRC

    ↓

    Process Command

------------------------------------------------------------------------

# 4. CAN Interface Design

# 4.1 Why Use CAN

CAN is widely used in:

-   Automotive systems
-   Robots
-   Industrial controllers

Advantages:

-   Multi-node network
-   High reliability
-   Error detection
-   Real-time communication

------------------------------------------------------------------------

# 4.2 CAN Hardware Structure

Architecture:

    STM32 FDCAN

          |

    CAN Transceiver

          |

    CAN_H

    CAN_L

          |

    CAN Network

------------------------------------------------------------------------

# 4.3 CAN Transceiver

STM32 provides CAN controller internally.

External transceiver is required:

Function:

    Digital CAN Signal

    ↓

    Differential CAN Bus

Example:

TJA1051

------------------------------------------------------------------------

# 4.4 CAN Bus Termination

CAN requires termination resistors:

    Node A

    120Ω

    |

    CAN_H/CAN_L

    |

    120Ω

    Node B

Purpose:

-   Match bus impedance
-   Reduce reflection

------------------------------------------------------------------------

# 4.5 CAN PCB Routing

Requirements:

-   CAN_H and CAN_L routed together
-   Similar trace length
-   Avoid switching power area
-   Add protection near connector

------------------------------------------------------------------------

# 5. UART Debug Interface

## Purpose

UART is used for:

-   Firmware log output
-   Debug information
-   Parameter configuration

Interface:

    STM32 UART

    TX

    RX

    GND

Recommended:

Add test pins for debugging.

------------------------------------------------------------------------

# 6. Communication Bring-up Plan

## Step 1: Power Verification

Check:

-   Transceiver supply voltage
-   MCU IO voltage

------------------------------------------------------------------------

## Step 2: Signal Measurement

Tools:

-   Oscilloscope
-   Logic analyzer

Check:

-   UART waveform
-   RS485 differential signal
-   CAN waveform

------------------------------------------------------------------------

## Step 3: Communication Test

RS485:

-   PC USB-RS485 adapter
-   Modbus test software

CAN:

-   CAN analyzer
-   CAN communication tool

------------------------------------------------------------------------

# 7. PCB Design Considerations

## RS485 Area

Place:

    Connector

    ↓

    Protection

    ↓

    Transceiver

    ↓

    MCU

Keep external noise away from MCU.

------------------------------------------------------------------------

## CAN Area

Requirements:

-   Differential routing
-   Short connection
-   Good grounding

------------------------------------------------------------------------

# 8. Interview Notes

## Q1:

Why cannot UART directly replace RS485?

Answer:

UART is suitable for short-distance communication, while RS485 uses
differential transmission and provides better noise immunity for
industrial environments.

------------------------------------------------------------------------

## Q2:

Why does RS485 require termination resistance?

Answer:

To reduce signal reflection caused by transmission line impedance
mismatch.

------------------------------------------------------------------------

## Q3:

Why does CAN need two 120Ω resistors?

Answer:

CAN is a differential bus. Termination resistors match the
characteristic impedance of the cable and improve signal integrity.

------------------------------------------------------------------------

# Version History

  Version   Date      Description
  --------- --------- ------------------------------------------------------
  V1.0      2026-09   Initial communication interface design specification
