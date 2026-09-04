# STM32G4 Industrial Data Acquisition & Control Terminal

# Board Bring-up Test Report

**Version:** V1.0\
**Date:** 2026-09

------------------------------------------------------------------------

# 1. Bring-up Overview

## 1.1 Purpose

This document records the hardware bring-up process after receiving the
first PCB prototype.

The purpose is to verify:

-   PCB manufacturing quality
-   Power system stability
-   MCU startup capability
-   Peripheral functionality
-   Communication interfaces

The bring-up process follows:

    PCB Received

          |

    Visual Inspection

          |

    Power Check Before Startup

          |

    First Power-On

          |

    Power Rail Verification

          |

    MCU Debug Verification

          |

    Peripheral Testing

          |

    System Validation

------------------------------------------------------------------------

# 2. Board Information

## 2.1 Hardware Version

PCB Version:

    V1.0

MCU:

    STM32G474RET6

Power Input:

    24V DC Industrial Input

------------------------------------------------------------------------

# 3. Pre-Power Inspection

Before applying power, all hardware should be checked manually.

------------------------------------------------------------------------

# 3.1 PCB Visual Inspection

Inspection items:

-   PCB appearance
-   Component placement
-   Soldering quality
-   IC orientation
-   Connector direction
-   Missing components

Insert photos:

    [PCB Front Photo]


    [PCB Back Photo]

------------------------------------------------------------------------

# 3.2 Power Resistance Check

Tool:

-   Digital Multimeter

Purpose:

Check possible short circuits before power-on.

Measurement points:

  Test Point   Measured Resistance   Result
  ------------ --------------------- --------
  VIN-GND                            
  5V-GND                             
  3.3V-GND                           

Expected:

No short circuit condition.

------------------------------------------------------------------------

# 3.3 Component Inspection

## Power Section

Check:

-   TVS diode direction
-   MOSFET orientation
-   Buck converter components
-   Inductor
-   Capacitor polarity

## MCU Section

Check:

-   STM32 orientation
-   Crystal oscillator
-   Reset circuit
-   Decoupling capacitors

## Communication Section

Check:

-   RS485 transceiver
-   CAN transceiver
-   Protection components

------------------------------------------------------------------------

# 4. First Power-On Test

## 4.1 Power Supply Setup

Equipment:

-   Adjustable DC power supply

Initial setting:

    Voltage:

    24V


    Current Limit:

    500mA

Insert photo:

    [Power Supply Setup Photo]

------------------------------------------------------------------------

# 4.2 Power-on Procedure

Step 1:

Connect power supply.

Step 2:

Observe current consumption.

Step 3:

Check smoke, abnormal heating or noise.

Step 4:

Measure power rails.

------------------------------------------------------------------------

# 5. Power Rail Verification

## 5.1 24V Input

Check:

-   Input voltage
-   Stability
-   Surge behavior

Measurement:

    [24V Oscilloscope Waveform]

------------------------------------------------------------------------

## 5.2 5V Buck Output

Expected:

    5V ±5%

Measurement:

  Parameter      Result
  -------------- --------
  Voltage        
  Ripple         
  Startup Time   

Insert:

    [5V Ripple Waveform]

------------------------------------------------------------------------

## 5.3 3.3V MCU Supply

Purpose:

Verify MCU power quality.

Check:

-   Voltage level
-   Ripple
-   Startup sequence

Insert:

    [3.3V Waveform]

------------------------------------------------------------------------

# 6. MCU Bring-up

## 6.1 SWD Debug Connection

Tool:

-   ST-Link

Verification:

-   Device detection
-   Flash programming
-   Debug connection

Insert:

    [STM32CubeProgrammer Screenshot]

------------------------------------------------------------------------

# 6.2 Clock Verification

Check:

-   External crystal oscillator
-   PLL startup

Measurement:

    [Crystal Oscilloscope Waveform]

------------------------------------------------------------------------

# 7. Peripheral Verification

# 7.1 UART Debug

Purpose:

Provide firmware log output.

Test:

    STM32 UART

          |

    USB-UART Adapter

          |

    PC Terminal

Insert:

    [UART Log Screenshot]

------------------------------------------------------------------------

# 7.2 ADC Verification

Input signals:

-   0-10V
-   4-20mA

Test:

  Input Signal   ADC Value   Error
  -------------- ----------- -------
  0V                         
  5V                         
  10V                        
  4mA                        
  20mA                       

------------------------------------------------------------------------

# 7.3 RS485 Verification

Equipment:

-   USB-RS485 Adapter

Test:

-   Differential waveform
-   Data transmission
-   Modbus communication

Insert:

    [RS485 Test Screenshot]

------------------------------------------------------------------------

# 7.4 CAN Verification

Equipment:

-   CAN Analyzer

Test:

-   CAN_H/CAN_L waveform
-   Message transmission
-   Error status

Insert:

    [CAN Test Screenshot]

------------------------------------------------------------------------

# 8. Problem Tracking

Hardware debugging requires recording every issue.

  Issue ID   Problem   Root Cause   Solution   Status
  ---------- --------- ------------ ---------- --------
  001                                          

Example:

    Problem:

    MCU cannot start


    Investigation:

    1. Check 3.3V supply
    2. Check reset signal
    3. Check crystal waveform


    Root Cause:

    Power supply issue


    Solution:

    Modify power circuit

------------------------------------------------------------------------

# 9. Final Verification

  Function          Result
  ----------------- --------
  24V Input         
  5V Power Rail     
  3.3V Power Rail   
  MCU Boot          
  UART              
  ADC               
  RS485             
  CAN               

------------------------------------------------------------------------

# 10. Engineering Notes

## Debug Principle

Hardware debugging follows:

    Power

    ↓

    Clock

    ↓

    Reset

    ↓

    Communication

    ↓

    Application

Do not debug software before confirming hardware fundamentals.

------------------------------------------------------------------------

# Version History

  Version   Date      Description
  --------- --------- ------------------------------
  V1.0      2026-09   Initial bring-up test report
