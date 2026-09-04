# STM32G4 Industrial Data Acquisition & Control Terminal

# Hardware Design Specification

**Version:** V1.0\
**Date:** 2026-09

------------------------------------------------------------------------

# 1. Hardware Design Overview

## 1.1 Design Objective

The hardware system is designed as an industrial embedded control
terminal.

The design focuses on:

-   Reliable 24V industrial power input
-   Protected electrical interface
-   Accurate analog signal acquisition
-   Industrial communication capability
-   Stable embedded controller operation
-   EMC and signal integrity consideration

The hardware follows industrial product development principles:

    Requirement

    ↓

    System Architecture

    ↓

    Hardware Design

    ↓

    PCB Layout

    ↓

    Prototype Bring-up

    ↓

    Verification

------------------------------------------------------------------------

# 2. Hardware System Architecture

## 2.1 Functional Block Diagram

                     24V DC INPUT


                           |

                           |

            +-----------------------------+

            | Power Input Protection     |

            |                             |

            | Fuse                        |

            | TVS Surge Protection        |

            | Reverse Polarity Protection|

            | EMI Filter                 |

            +-----------------------------+


                           |

                           |


            +-----------------------------+

            | Power Conversion            |

            |                             |

            | Buck Converter              |

            | 24V -> 5V                   |

            |                             |

            | LDO                         |

            | 5V -> 3.3V                  |

            +-----------------------------+


                           |

                           |


            +-----------------------------+

            | STM32G4 Core System         |

            |                             |

            | MCU                         |

            | Clock                       |

            | Reset                       |

            | SWD Debug                   |

            +-----------------------------+


                  |          |          |

                  |          |          |


                ADC       RS485       CAN


                  |

                  |

            Industrial Sensors

------------------------------------------------------------------------

# 3. Hardware Module Definition

# 3.1 Power Input Protection Module

## Function

Protect the system from abnormal industrial power conditions.

## Input

    24V DC Industrial Power

## Protection Functions

### Fuse Protection

Purpose:

-   Prevent excessive current
-   Protect PCB from short circuit

Typical fault:

    Component failure

            |

            |

    Large current

            |

            |

    Fuse disconnects

------------------------------------------------------------------------

### TVS Surge Protection

Industrial environment may contain transient voltage caused by:

-   Motor switching
-   Relay operation
-   Solenoid valve switching

TVS provides:

-   Fast transient suppression
-   Voltage clamping
-   Protection for downstream circuits

------------------------------------------------------------------------

### Reverse Polarity Protection

Purpose:

Prevent damage caused by incorrect wiring.

Implementation:

-   MOSFET reverse protection

Reason:

Industrial equipment may require field maintenance where wiring mistakes
can occur.

------------------------------------------------------------------------

# 3.2 Power Conversion Module

## Power Architecture

    24V Input

        |

    Protection

        |

    Buck Converter

        |

    5V Rail

        |

    LDO

        |

    3.3V Rail

        |

    MCU System

------------------------------------------------------------------------

## Design Reason

A direct 24V to 3.3V linear regulator solution is not suitable.

Reason:

Power loss:

    P = (Vin - Vout) × I

Example:

    (24V - 3.3V) × 0.2A

    = 4.14W

The regulator would generate excessive heat.

Therefore:

Selected architecture:

    24V

    ↓

    Buck Converter

    ↓

    5V

    ↓

    LDO

    ↓

    3.3V

Advantages:

-   Higher efficiency
-   Lower heat generation
-   Better power stability

------------------------------------------------------------------------

# 3.3 STM32G4 Minimum System

Main Controller:

    STM32G474RET6

Required circuits:

## Clock Circuit

Function:

Provide stable MCU clock source.

Components:

-   External crystal oscillator
-   Load capacitors

------------------------------------------------------------------------

## Reset Circuit

Function:

Ensure reliable startup.

Includes:

-   Reset pull-up
-   Reset button
-   Debug reset support

------------------------------------------------------------------------

## SWD Debug Interface

Purpose:

-   Firmware programming
-   Debugging
-   Hardware verification

Interface:

    SWDIO

    SWCLK

    GND

    3.3V

    RESET

------------------------------------------------------------------------

# 3.4 Analog Front-End Module

Supported signals:

  Input Type       Range
  ---------------- --------
  Voltage Signal   0-10V
  Current Signal   4-20mA

------------------------------------------------------------------------

# 3.4.1 0-10V Input

Architecture:

    0-10V Sensor

          |

    Input Protection

          |

    Voltage Divider

          |

    RC Filter

          |

    STM32 ADC

Design objectives:

-   Protect ADC input
-   Reduce noise
-   Match ADC voltage range

------------------------------------------------------------------------

# 3.4.2 4-20mA Input

Architecture:

    Current Sensor

          |

    Sampling Resistor

          |

    Voltage Signal

          |

    Signal Conditioning

          |

    ADC

Conversion principle:

    V = I × R

Advantages:

-   Long transmission distance
-   Strong noise immunity
-   Industrial standard

------------------------------------------------------------------------

# 3.5 Communication Module

# RS485 Interface

Structure:

    STM32 UART

          |

    RS485 Transceiver

          |

    Protection Circuit

          |

    A/B Differential Bus

Design considerations:

-   Differential routing
-   TVS protection
-   Termination resistor
-   Bias resistor

------------------------------------------------------------------------

# CAN Interface

Structure:

    STM32 FDCAN

          |

    CAN Transceiver

          |

    CAN_H

    CAN_L

Design considerations:

-   Differential impedance
-   Bus termination
-   Noise protection

------------------------------------------------------------------------

# 3.6 Debug Interface

Interfaces:

## UART Debug

Purpose:

-   Log output
-   Firmware debugging

## SWD

Purpose:

-   Program MCU
-   Debug firmware

------------------------------------------------------------------------

# 4. PCB Partition Strategy

## 4.1 Functional Area Separation

PCB layout should be divided into:


    +--------------------------------+

    | Power Area                     |

    |                                |

    | Buck Converter                 |

    +--------------------------------+


    +----------------+---------------+

    | Analog Area    | Digital Area  |

    | ADC            | MCU           |

    +----------------+---------------+


    | Communication Area             |

    | RS485 / CAN                    |

    +--------------------------------+

------------------------------------------------------------------------

# 4.2 Layout Principles

## Power Section

Requirements:

-   Short current loop
-   Wide traces
-   Minimize switching noise

------------------------------------------------------------------------

## Analog Section

Requirements:

-   Keep away from switching power
-   Clean ground reference
-   Short ADC traces

------------------------------------------------------------------------

## Communication Section

Requirements:

-   Protection close to connector
-   Differential routing
-   Controlled return path

------------------------------------------------------------------------

# 5. Hardware Design Verification Plan

## Power Verification

Measurements:

-   24V input stability
-   5V ripple
-   3.3V ripple

Tools:

-   Oscilloscope
-   Electronic load

------------------------------------------------------------------------

## Signal Verification

Measurements:

-   ADC input voltage
-   Communication waveform
-   Noise level

Tools:

-   Oscilloscope
-   Logic analyzer

------------------------------------------------------------------------

# 6. Hardware Development Status

  Module                  Status
  ----------------------- -----------
  System Architecture     Completed
  Power Design            Pending
  MCU Minimum System      Pending
  Analog Front-End        Pending
  Communication Circuit   Pending
  PCB Layout              Pending

------------------------------------------------------------------------

# Version History

  Version   Date      Description
  --------- --------- ---------------------------------------
  V1.0      2026-09   Initial hardware design specification
