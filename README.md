# STM32G4 Industrial Data Acquisition & Control Terminal

![Project
Status](https://img.shields.io/badge/Status-Under%20Development-blue)
![MCU](https://img.shields.io/badge/MCU-STM32G4-green)
![PCB](https://img.shields.io/badge/PCB-4%20Layer-orange)

## Project Overview

The STM32G4 Industrial Data Acquisition & Control Terminal is an
embedded industrial controller designed for 24V field environments.

The project focuses on building a complete hardware and firmware system
following real product development processes, including:

-   Industrial power input protection
-   Multi-stage power conversion
-   Analog sensor acquisition
-   Industrial communication
-   Embedded real-time software
-   Hardware verification and bring-up

The final system provides a reliable interface between industrial
sensors, control equipment and upper-level monitoring software.

------------------------------------------------------------------------

# System Architecture

                     24V DC Industrial Input

                              |
                              |

                  +-----------------------+
                  | Input Protection      |
                  | Fuse                  |
                  | TVS Surge Protection  |
                  | Reverse Protection    |
                  +-----------------------+

                              |

                              |

                  +-----------------------+
                  | Buck Converter        |
                  | 24V -> 5V             |
                  +-----------------------+

                              |

                              |

                  +-----------------------+
                  | LDO                   |
                  | 5V -> 3.3V            |
                  +-----------------------+

                              |

                              |

                  +-----------------------+
                  | STM32G4 MCU           |
                  |                       |
                  | ADC                   |
                  | UART                  |
                  | RS485                 |
                  | CAN                   |
                  +-----------------------+

                      |              |

                      |              |

                 Industrial      PC Monitoring
                  Sensors          Software

------------------------------------------------------------------------

# Hardware Features

## MCU Controller

Selected MCU:

**STM32G474RET6**

Main features:

-   ARM Cortex-M4 core
-   170MHz maximum frequency
-   High performance ADC
-   DMA controller
-   Multiple UART interfaces
-   FDCAN controller

------------------------------------------------------------------------

## Industrial Power System

Input:

    24V DC

Power architecture:

    24V
     |
    Protection
     |
    Buck Converter
     |
    5V
     |
    LDO
     |
    3.3V
     |
    MCU System

Protection design:

-   Fuse protection
-   TVS surge suppression
-   Reverse polarity protection
-   EMI filtering

------------------------------------------------------------------------

## Analog Acquisition

Supported industrial signals:

  Signal          Range
  --------------- --------
  Voltage Input   0-10V
  Current Loop    4-20mA

Functions:

-   ADC sampling
-   Signal conditioning
-   Digital filtering
-   Calibration

------------------------------------------------------------------------

## Communication Interface

### RS485

Applications:

-   Industrial device communication
-   Long-distance sensor networking

Features:

-   Differential communication
-   High noise immunity
-   Modbus RTU support

------------------------------------------------------------------------

### CAN

Applications:

-   Distributed control systems
-   Real-time communication

Features:

-   Multi-node network
-   Reliable message transmission

------------------------------------------------------------------------

# Firmware Architecture

Software stack:

    Application Layer

            |

    FreeRTOS

            |

    STM32 HAL

            |

    Hardware Layer

Firmware functions:

-   ADC DMA acquisition
-   UART communication
-   RS485 protocol
-   CAN communication
-   Parameter management
-   Bootloader upgrade (planned)

------------------------------------------------------------------------

# PCB Design

PCB specification:

-   4-layer PCB
-   Dedicated GND plane
-   Power plane design
-   EMC consideration

Design objectives:

-   Reduce power noise
-   Improve signal integrity
-   Enhance industrial reliability

------------------------------------------------------------------------

# Development Process

## Phase 1: System Architecture

Status:

Completed

## Phase 2: Hardware Design

Tasks:

-   Component selection
-   Schematic design
-   PCB layout

## Phase 3: Firmware Development

Tasks:

-   Peripheral driver development
-   FreeRTOS integration
-   Communication protocol

## Phase 4: Hardware Bring-up

Tasks:

-   Power rail verification
-   Oscilloscope measurement
-   Communication testing

## Phase 5: System Optimization

Tasks:

-   EMC improvement
-   Reliability testing

------------------------------------------------------------------------

# Project Repository Structure

    STM32G4-Industrial-Control-Terminal

    ├── README.md

    ├── docs
    │   └── 01_System_Architecture.md

    ├── Hardware
    │   ├── Schematic
    │   ├── PCB
    │   └── BOM

    ├── Firmware

    ├── Software

    └── Test

------------------------------------------------------------------------

# Engineering Goals

This project aims to demonstrate capabilities in:

## Hardware Engineering

-   Industrial power design
-   Analog front-end design
-   PCB layout
-   EMC debugging
-   Hardware bring-up

## Embedded Software

-   STM32 HAL development
-   FreeRTOS
-   DMA
-   Communication protocols

## Product Development

-   Requirement analysis
-   Design documentation
-   Verification testing
-   Version management

------------------------------------------------------------------------

# Version History

  Version   Date      Description
  --------- --------- ----------------
  V1.0      2026-09   Initial README
