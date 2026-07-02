# 🌀 Automatic Fan Speed Control Using Temperature Sensor

An embedded systems project that automatically regulates DC fan speed based on ambient temperature, using an **Arduino UNO**, a **TMP36 analog temperature sensor**, and a **transistor-driven PWM circuit**. Designed and validated through circuit simulation (Tinkercad) across low, moderate, and high temperature scenarios.

---

## 📖 Overview

Manual fan regulation is inefficient — fans are often left running at full speed regardless of actual cooling needs, or forgotten to be switched off, wasting power. This project builds a closed-loop, temperature-responsive fan control system that continuously senses the environment and adjusts fan speed accordingly, eliminating manual intervention and improving energy efficiency.

## 🎯 Objectives

- Design an embedded system that senses ambient temperature in real time.
- Automatically control DC fan speed proportional to sensed temperature using PWM.
- Eliminate manual fan speed adjustment and reduce power wastage at low temperatures.
- Simulate and validate circuit behaviour across multiple temperature test cases.

## 🧰 Components Required

| Designator | Component | Qty |
|---|---|---|
| U1 | Arduino UNO R3 | 1 |
| T1 | NPN Transistor (BJT) | 1 |
| D1 | Diode (flyback/freewheeling) | 1 |
| U2 | TMP36 Temperature Sensor | 1 |
| M1 | DC Motor (fan) | 1 |
| R1 | Resistor (base current limiting) | 1 |

## ⚙️ Working Principle

The **TMP36** sensor outputs an analog voltage linearly proportional to ambient temperature, read via Arduino analog pin **A0**. The reading is converted to °C (0.488 °C per ADC unit), and the following control logic sets the fan's PWM duty cycle on digital pin **9**:

| Temperature | Fan State | PWM Value | Duty Cycle |
|---|---|---|---|
| < 23 °C | OFF | 0 | 0% |
| 23 °C – 33 °C | Medium speed | 128 | ≈ 50% |
| ≥ 33 °C | Full speed | 255 | 100% |

Since Arduino I/O pins can't safely drive a motor's current directly, an **NPN transistor (T1)** acts as a current-controlled switch — the PWM signal drives its base (through base resistor R1), while its collector–emitter path switches power to the motor. A **flyback diode (D1)** is placed across the motor to safely dissipate the voltage spike generated when the inductive motor coil is switched off, protecting the transistor.

The loop repeats every 500 ms, with each reading logged to the Serial Monitor.

### Why an NPN Transistor?

An Arduino pin can source only ~20–40 mA, while a DC motor draws far more, especially at start-up. The NPN transistor amplifies a small base current into a much larger collector current from an external 5V supply, letting a low-power PWM signal switch a high-power load safely. NPN is chosen (over PNP) because the motor is wired high-side and the transistor switches the low side — matching the Arduino's natural idle-LOW / pulse-HIGH output.

### Why a Flyback(PN Junction) Diode?

The DC motor is an inductive load. When the transistor switches OFF, the collapsing magnetic field induces a large reverse voltage spike that can destroy the transistor. The diode, reverse-biased during normal operation, becomes forward-biased during this spike and gives the stored current a safe path to decay — standard protection for any switched inductive load (motor, relay, solenoid).

## 🔌 Circuit Diagram

Schematic built and simulated in **Tinkercad**, showing the Arduino UNO, TMP36 sensor, NPN transistor stage, flyback diode, base resistor, and DC motor (fan). See `/schematic` or the full report for the diagram.

## 🧪 Simulation & Testing

The circuit was tested across three representative conditions to validate each branch of the control logic:

| Condition | Sensed Temp | Fan State | PWM Value | Duty Cycle |
|---|---|---|---|---|
| Cool | 21 °C | OFF | 0 | 0% |
| Moderate | 26 °C | ON — Medium speed | 128 | ≈ 50% |
| Hot | 46 °C | ON — Full speed | 255 | 100% |

Results confirm the system correctly transitions between all three operating states in direct response to sensed temperature, validating the control logic.

## 🚀 Applications

- Automatic cooling for electronic enclosures and control panels
- Smart home climate control / energy-saving ventilation
- CPU or server cabinet cooling based on real-time temperature
- Industrial machine and motor cooling systems
- Greenhouse and storage room temperature regulation

## ✅ Advantages

- Fully automatic operation — no manual switching required
- Energy efficient — fan runs only when needed, at a speed proportional to demand
- Simple, low-cost hardware built around a widely available microcontroller and sensor
- Easily extendable with additional sensors, an LCD display, or IoT connectivity

## 🔮 Future Scope

- Add an LCD/OLED display for live temperature and fan status
- Integrate IoT connectivity (ESP8266/ESP32) for remote monitoring and logging
- Implement PID-based closed-loop control for smoother, continuous speed variation
- Add multiple sensor nodes for zone-wise cooling in larger environments

## 📝 Conclusion

This project demonstrates a practical application of embedded systems in real-time environmental monitoring and actuation. By combining a TMP36 sensor, an Arduino UNO, and a transistor-driven PWM stage, the system automatically adjusts fan speed across three temperature bands, validated through simulation at 21 °C, 26 °C, and 46 °C — providing a solid foundation for further enhancement with sensors, displays, or IoT integration.

---

## 👤 Author

**Mallampally Jayantha Siva Srinivas**
B.Tech, Electronics and Communication Engineering
July 2026
