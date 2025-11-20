# Embedded Systems Workshop - GPIO Control

This project demonstrates core embedded systems concepts by implementing firmware to control an LED using an STM32 microcontroller's GPIO pins through direct register manipulation.

## Project Overview
- **Microcontroller:** STM32
- **Language:** C
- **Key Concepts:** Hardware-Software Integration, Register-Level GPIO Control

## Implementation
- Wrote firmware in C to directly manipulate microcontroller registers
- Configured GPIO pin as a digital output
- Implemented a precise software delay loop to create a blinking pattern

## Skills Demonstrated
- Embedded C Programming
- Microcontroller Architecture
- Hardware/Software Integration
- Debugging and Validation

## Code Example
```c
for (int i = 0; i < 1000000000; i++) {
    // Busy-wait delay
}
```
