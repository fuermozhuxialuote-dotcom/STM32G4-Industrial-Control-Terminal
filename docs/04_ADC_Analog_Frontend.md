# STM32G4 Industrial Data Acquisition & Control Terminal

# ADC Analog Front-End Design Specification

**Version:** V1.0\
**Date:** 2026-09

------------------------------------------------------------------------

# 1. Analog Front-End Overview

## 1.1 Design Objective

Industrial sensors usually do not output signals that can be directly
connected to an MCU ADC.

Typical industrial signals:

-   0-10V voltage signal
-   4-20mA current loop signal

However, STM32 ADC input range is limited:

    0V ~ 3.3V

Therefore, signal conditioning is required:

    Industrial Sensor

            |

    Protection Circuit

            |

    Signal Conditioning

            |

    Filtering

            |

    STM32 ADC

            |

    Digital Processing

The analog front-end is responsible for:

-   Protecting ADC input
-   Scaling signal range
-   Reducing noise
-   Improving measurement accuracy

------------------------------------------------------------------------

# 2. Industrial Signal Selection

## 2.1 Why Industrial Uses 4-20mA

4-20mA is widely used in industrial environments because it provides:

## Long Distance Transmission

Voltage signals are affected by cable resistance:

    Sensor ---------------- PLC

              Cable Resistance

Voltage drop:

    V = I × R

Cable resistance changes can introduce measurement errors.

Current loop avoids this problem because:

    Series circuit current is identical everywhere

Therefore:

Sensor output:

    10mA

Receiving end:

    10mA

------------------------------------------------------------------------

## 2.2 Why Not Use 0-20mA?

0-20mA has a problem:

If the cable is disconnected:

    0mA

The controller cannot distinguish:

    Normal zero value

    or

    Sensor cable failure

Therefore:

4mA is used as the minimum signal.

Example:

    0 bar

    ↓

    4mA


    10 bar

    ↓

    20mA

If measured current is:

    0mA

It indicates a possible fault.

------------------------------------------------------------------------

# 3. 0-10V Voltage Input Design

## 3.1 Signal Path

Architecture:

    0-10V Sensor

          |

    Input Protection

          |

    Voltage Divider

          |

    RC Filter

          |

    ADC Input

------------------------------------------------------------------------

## 3.2 Voltage Scaling

STM32 ADC maximum input:

    3.3V

Therefore 10V input cannot directly enter ADC.

A resistor divider is used:

    Vin

     |

    R1

     |

    +-------- ADC

     |

    R2

     |

    GND

Formula:

    Vout = Vin × R2/(R1+R2)

Design target:

    10V input

    ↓

    approximately 3V ADC input

This leaves voltage margin to avoid ADC over-range.

------------------------------------------------------------------------

# 4. 4-20mA Current Input Design

## 4.1 Current To Voltage Conversion

The current signal is converted using a precision resistor.

Principle:

    V = I × R

Example:

Using:

    250Ω resistor

For 4mA:

    V = 0.004 × 250

    V = 1V

For 20mA:

    V = 0.02 × 250

    V = 5V

Because STM32 ADC accepts:

    0 ~ 3.3V

Additional voltage scaling is required.

------------------------------------------------------------------------

# 5. Signal Filtering Design

## 5.1 Why Filtering Is Required

Industrial environments contain:

-   Motor switching noise
-   Relay switching noise
-   PWM interference
-   Power supply ripple

Therefore ADC input requires filtering.

------------------------------------------------------------------------

## 5.2 RC Low Pass Filter

Typical structure:

    Signal

     |

    R

     |

    +------ ADC

     |

    C

     |

    GND

Cutoff frequency:

    fc = 1/(2πRC)

The filter should remove high-frequency noise while maintaining sensor
response speed.

------------------------------------------------------------------------

# 6. ADC Sampling Architecture

STM32G4 ADC acquisition:

    Timer

     |

    ADC Trigger

     |

    ADC Conversion

     |

    DMA Transfer

     |

    Memory Buffer

     |

    Digital Filtering

     |

    Application

Advantages:

-   CPU load reduction
-   Stable sampling interval
-   Higher data processing efficiency

------------------------------------------------------------------------

# 7. PCB Layout Consideration

Analog circuits are sensitive to digital switching noise.

Layout principles:

## Keep Away From

-   Buck converter switching node
-   High-speed communication lines
-   Clock signals

## Routing

Requirements:

-   Short ADC traces
-   Clean analog ground
-   Proper filtering capacitor placement

------------------------------------------------------------------------

# 8. Calibration Design

Industrial measurement requires calibration.

Possible methods:

## Hardware Calibration

Adjust resistor values.

## Software Calibration

Use:

    ADC Value

    ×

    Gain

    +

    Offset

Stored parameters can be saved in:

-   Flash
-   EEPROM

------------------------------------------------------------------------

# 9. Verification Plan

## Test Method

Input known signals:

  Input   Expected
  ------- ---------------
  0V      ADC minimum
  5V      Middle value
  10V     Maximum value

For current loop:

  Current   Expected
  --------- ----------
  4mA       Minimum
  12mA      Middle
  20mA      Maximum

------------------------------------------------------------------------

# 10. Interview Notes

## Q1:

Why does industrial equipment use 4-20mA instead of voltage signals?

Answer:

Current transmission has better immunity against cable resistance and
noise, making it suitable for long-distance industrial environments.

------------------------------------------------------------------------

## Q2:

Why cannot 10V directly connect to STM32 ADC?

Answer:

STM32 ADC input range is normally around 0-3.3V. Higher voltage may
damage the MCU and requires signal conditioning.

------------------------------------------------------------------------

## Q3:

Why add RC filtering before ADC?

Answer:

To reduce high-frequency noise and improve measurement stability.

------------------------------------------------------------------------

# Version History

  Version   Date      Description
  --------- --------- ---------------------------------------------------
  V1.0      2026-09   Initial ADC analog front-end design specification
