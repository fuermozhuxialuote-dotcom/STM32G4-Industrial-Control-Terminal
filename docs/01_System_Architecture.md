# STM32G4 Industrial Data Acquisition & Control Terminal

## System Architecture Design Specification

Version: V1.0\
Date: 2026-09

------------------------------------------------------------------------

# 1. Project Overview

## 1.1 Introduction

This project designs an industrial-grade embedded control terminal based
on STM32G4 MCU.

The system integrates:

-   24V industrial power input
-   Power protection circuit
-   DC/DC power conversion
-   Analog signal acquisition
-   RS485 communication
-   CAN communication
-   Real-time embedded firmware
-   PC monitoring software

The goal is to develop a complete industrial embedded product prototype
following a real engineering development process.

------------------------------------------------------------------------

# 2. Product Objectives

The terminal is designed for:

-   Industrial sensor data acquisition
-   Equipment monitoring
-   Industrial communication
-   Local control

System workflow:

    Industrial Sensor

            |

    STM32G4 Control Terminal

            |

    RS485 / CAN Communication

            |

    PC Monitoring Software

------------------------------------------------------------------------

# 3. Hardware Architecture

## 3.1 System Block Diagram

    24V DC Input

            |

    Input Protection

    (Fuse + TVS + Reverse Protection)

            |

    Buck Converter

    24V → 5V

            |

    LDO

    5V → 3.3V

            |

    STM32G4 MCU

            |

    ----------------------------

    |            |             |

    ADC        RS485          CAN

    |

    Sensor

------------------------------------------------------------------------

# 4. MCU Selection

Selected MCU:

STM32G474RET6

Reasons:

-   Cortex-M4 architecture
-   170MHz operating frequency
-   High performance ADC
-   Multiple UART interfaces
-   Integrated FDCAN controller
-   DMA support

The MCU meets requirements of:

-   Industrial control
-   Sensor processing
-   Communication applications

------------------------------------------------------------------------

# 5. Power Architecture

Input:

24V Industrial DC

Power conversion:

    24V

     |

    Protection

     |

    Buck

     |

    5V

     |

    LDO

     |

    3.3V

     |

    MCU

Design considerations:

-   Surge protection
-   Reverse polarity protection
-   EMI filtering
-   Power integrity

------------------------------------------------------------------------

# 6. Analog Acquisition

Supported signals:

  Signal          Range
  --------------- --------
  Voltage Input   0-10V
  Current Input   4-20mA

ADC processing:

    Industrial Signal

            |

    Signal Conditioning

            |

    ADC Sampling

            |

    Digital Processing

------------------------------------------------------------------------

# 7. Communication

## RS485

Purpose:

Industrial long-distance communication

Features:

-   Differential transmission
-   High noise immunity
-   Modbus RTU support

------------------------------------------------------------------------

## CAN

Purpose:

Real-time distributed communication

Features:

-   Multi-node communication
-   High reliability

------------------------------------------------------------------------

# 8. PCB Design Requirement

PCB:

4-layer board

Stack-up:

    Layer 1 : Signal

    Layer 2 : GND Plane

    Layer 3 : Power Plane

    Layer 4 : Signal

Design focus:

-   EMC
-   Signal integrity
-   Grounding
-   Power integrity

------------------------------------------------------------------------

# 9. Firmware Architecture

Software structure:

    Application

          |

    FreeRTOS

          |

    HAL Driver

          |

    STM32 Hardware

Main tasks:

-   ADC acquisition task
-   Communication task
-   Parameter management task
-   System monitoring task

------------------------------------------------------------------------

# 10. Development Roadmap

## Phase 1

System Architecture

Status:

Completed

## Phase 2

Hardware Design

Tasks:

-   Component selection
-   Schematic design
-   PCB layout

## Phase 3

Firmware Development

Tasks:

-   HAL development
-   FreeRTOS
-   Communication protocol

## Phase 4

Hardware Bring-up

Tasks:

-   Power test
-   Signal measurement
-   Communication verification

------------------------------------------------------------------------

# Version History

  Version   Date      Description
  --------- --------- -------------------------------
  V1.0      2026-09   Initial architecture document
