# STM32G4 Industrial Data Acquisition & Control Terminal

# Power Supply Design Specification

**Version:** V1.0\
**Date:** 2026-09

------------------------------------------------------------------------

# 1. Power Design Overview

## 1.1 Design Objective

The system is designed to operate in a 24V industrial power environment.

Unlike consumer electronics, industrial power input may experience:

-   Voltage fluctuation
-   Surge voltage
-   Reverse connection
-   Switching noise from motors and relays

Therefore, the power system must provide:

-   Input protection
-   Stable voltage conversion
-   Low noise 3.3V supply
-   Good EMC performance

The power architecture is:

    24V DC Input

          |

    Input Protection

          |

    Buck Converter

    24V → 5V

          |

    LDO

    5V → 3.3V

          |

    MCU + Analog Circuit + Communication

------------------------------------------------------------------------

# 2. Power Tree Design

## 2.1 System Power Rails

  Power Rail   Voltage   Purpose
  ------------ --------- ---------------------------------------
  VIN          24V       Industrial input
  5V           5V        Communication and intermediate supply
  3V3          3.3V      MCU and analog circuits

------------------------------------------------------------------------

# 3. 24V Input Protection Design

## 3.1 Protection Structure

The input protection stage:

    24V INPUT

        |

        |

    Fuse

        |

        |

    TVS Diode

        |

        |

    Reverse Polarity Protection

        |

        |

    EMI Filter

        |

        |

    Buck Converter

The order of protection components is important.

Protection devices should be placed close to the power connector to
prevent transient energy from entering the PCB.

------------------------------------------------------------------------

# 3.2 Fuse Selection

## Purpose

The fuse provides protection during:

-   Short circuit
-   Component failure
-   Excessive current condition

The fuse is not designed for normal current regulation.

Its purpose is to disconnect the circuit under abnormal conditions.

------------------------------------------------------------------------

## Selection Consideration

Important parameters:

-   Rated voltage
-   Rated current
-   Breaking capacity
-   Response time

The rated current should consider:

-   Normal operating current
-   Startup current
-   Load margin

Example:

Assume:

Normal system current:

    500mA

Select fuse:

    1A slow-blow fuse

Reason:

Allow normal startup current while protecting against faults.

------------------------------------------------------------------------

# 3.3 TVS Surge Protection

## Why TVS is Required

Industrial equipment may generate transient voltage from:

-   Motor switching
-   Relay contacts
-   Solenoid valves

These transients are usually short duration but high voltage.

TVS provides:

-   Fast response
-   Voltage clamping
-   Surge energy absorption

------------------------------------------------------------------------

## Selection Principle

The TVS voltage should satisfy:

    Normal operating voltage

            <

    TVS Stand-off Voltage

            <

    Protected device maximum voltage

For a 24V system:

Consider:

-   Maximum input voltage
-   Surge condition
-   Buck converter input rating

------------------------------------------------------------------------

# 3.4 Reverse Polarity Protection

## Problem

Field wiring errors can happen during:

-   Maintenance
-   Equipment installation
-   Cable replacement

A reverse connection may destroy:

-   Buck converter
-   MCU
-   Communication IC

------------------------------------------------------------------------

## Design Solution

Use MOSFET reverse protection.

Basic principle:

    Correct Connection

    MOSFET ON

    24V passes


    Reverse Connection

    MOSFET OFF

    Circuit protected

Advantages compared with diode:

-   Lower voltage drop
-   Lower power loss

------------------------------------------------------------------------

# 4. Buck Converter Design

## 4.1 Why Buck Converter is Required

A direct LDO conversion:

    24V → 3.3V

would generate:

    P = (Vin - Vout) × I

Assuming:

    I = 200mA

Power loss:

    P=(24-3.3)×0.2

    ≈4.14W

This causes:

-   High temperature
-   Low efficiency
-   Reliability problems

Therefore:

Switching regulator is required.

------------------------------------------------------------------------

# 4.2 Buck Architecture

Selected architecture:

    24V

     |

    Buck Converter

     |

    5V

Advantages:

-   High efficiency
-   Reduced thermal stress
-   Suitable for industrial input voltage

------------------------------------------------------------------------

# 4.3 Buck Converter Selection

Target requirements:

  Parameter       Requirement
  --------------- -----------------------
  Input Voltage   24V industrial supply
  Input Range     Above 30V margin
  Output          5V
  Current         \>1A capability
  Efficiency      High efficiency

Candidate device:

TPS54360

Reasons:

-   Wide input voltage range
-   Industrial application support
-   High current capability
-   Good thermal performance

------------------------------------------------------------------------

# 4.4 Buck Layout Consideration

The switching current loop should be minimized.

Critical components:

-   Input capacitor
-   MOSFET switching node
-   Inductor
-   Output capacitor

Layout principle:

    High Current Loop

    Keep:

    Short

    Wide

    Compact

Avoid routing sensitive signals near the switching node.

------------------------------------------------------------------------

# 5. 3.3V LDO Design

## Purpose

The LDO is not used for large voltage conversion.

Its purpose:

-   Reduce switching noise
-   Provide clean MCU supply
-   Improve ADC accuracy

Architecture:

    5V

     |

    LDO

     |

    3.3V

------------------------------------------------------------------------

# 6. Decoupling and Filtering Design

## 6.1 MCU Decoupling

Each power pin requires:

-   Ceramic capacitor
-   Short connection
-   Close placement

Typical:

    100nF + 1uF

Purpose:

Provide local high-frequency current.

------------------------------------------------------------------------

## 6.2 Bulk Capacitor

Placed near power input:

Purpose:

-   Reduce voltage fluctuation
-   Support transient current

Typical:

    10uF ~ 100uF

------------------------------------------------------------------------

# 7. Power Integrity Verification

## 7.1 Test Items

Before firmware debugging:

Measure:

  Test               Purpose
  ------------------ ----------------------
  24V Input          Check stability
  5V Output          Check ripple
  3.3V Output        Check MCU supply
  Startup waveform   Check power sequence

------------------------------------------------------------------------

## 7.2 Equipment

Recommended tools:

-   Digital multimeter
-   Oscilloscope
-   Electronic load

------------------------------------------------------------------------

# 8. Bring-up Plan

First power-on sequence:

Step 1:

Check resistance before power:

-   VIN to GND
-   5V to GND
-   3.3V to GND

Step 2:

Apply limited current power supply

Step 3:

Measure:

-   5V rail
-   3.3V rail

Step 4:

Connect MCU

------------------------------------------------------------------------

# 9. Interview Notes

## Q1:

Why not use LDO directly from 24V to 3.3V?

Answer:

Because the power dissipation is too high.

A switching regulator is required to improve efficiency and reduce
thermal stress.

------------------------------------------------------------------------

## Q2:

Why place TVS close to the connector?

Answer:

Because surge energy should be suppressed before entering the PCB.

------------------------------------------------------------------------

## Q3:

Why use Buck + LDO instead of only Buck?

Answer:

Buck provides efficiency, while LDO provides cleaner low-noise power for
sensitive circuits such as MCU ADC.

------------------------------------------------------------------------

# Version History

  Version   Date      Description
  --------- --------- ------------------------------------
  V1.0      2026-09   Initial power design specification
