# Turning On the Onboard LED on STM32 Using Direct Register Access

## Introduction

This document provides a step-by-step guide to turning on the onboard LED of an STM32 microcontroller (e.g., STM32F103C8T6, also known as the "Blue Pill") by writing directly to the hardware registers. We will avoid using any high-level libraries like HAL or CMSIS, focusing solely on direct memory-mapped I/O access. This approach helps embedded systems beginners understand how microcontrollers operate at a low level.

---
# Explanation of the Code Below

## Enabling GPIOC Clock

The **RCC (Reset and Clock Control)** peripheral manages the clocks for other peripherals in the STM32 microcontroller.

- The **RCC_APB2ENR** register is responsible for enabling the GPIOC clock. This is done by setting **bit 4** in the register.

---

## Configuring PC13 as an Output

The **GPIOC_CRH** register (GPIO Configuration Register High) controls the configuration of pins 8–15 of **Port C**.

- Since **PC13** falls within this range, we modify bits **23:20** of the **GPIOC_CRH** register.
- The value **0x2** (binary `0010`) is written to the register, which sets **PC13** as a **2 MHz push-pull output**.

---

## Turning the LED On

The LED on most STM32 boards (e.g., Blue Pill) is **active low**.

- To turn the LED on, we write **0** to **PC13** in the **GPIOC_ODR** (Output Data Register).
- Writing **0** to **PC13** effectively turns the LED on.

---

## Final Thoughts Before the Implementation

This example demonstrates how to interact with STM32 hardware directly using **memory-mapped registers**.

- By using direct register access, engineers can have full control over the microcontroller, allowing for optimized performance and reduced memory usage.
- This low-level approach is essential for embedded systems engineers aiming to build efficient and highly optimized embedded applications.

## Understanding STM32 GPIO Registers

### 1. Register Overview

In STM32 microcontrollers, GPIO (General-Purpose Input/Output) pins are controlled through several registers. The onboard LED is connected to **PC13**, which belongs to **GPIO Port C**. To control this pin, we interact with the following registers:

- **RCC_APB2ENR (Clock Enable Register):**  
  Enables the clock for GPIOC so that the microcontroller can access its registers.

- **GPIOC_CRH (Port Configuration Register High):**  
  Configures pins 8–15 of Port C, including PC13.

- **GPIOC_ODR (Output Data Register):**  
  Controls the output state of GPIO pins.

---

## Code Implementation

The following C program enables the clock for GPIOC, configures PC13 as an output, and then turns the LED on.

```c
// Define base addresses for peripheral memory
#define PERIPH_BASE     ((unsigned int)0x40000000) // Base address of peripheral memory
#define APB2PERIPH_BASE (PERIPH_BASE + 0x10000)    // Base address of APB2 peripherals
#define RCC_BASE        ((unsigned int)0x40021000) // Base address of RCC registers
#define GPIOC_BASE      ((unsigned int)0x40011000) // Base address of GPIOC registers

// Define register addresses
#define RCC_APB2ENR     (*(volatile unsigned int*)(RCC_BASE + 0x18)) // Clock enable register
#define GPIOC_CRH       (*(volatile unsigned int*)(GPIOC_BASE + 0x04)) // Configuration register high
#define GPIOC_ODR       (*(volatile unsigned int*)(GPIOC_BASE + 0x0C)) // Output data register

// Bit mask for enabling GPIOC clock in RCC_APB2ENR
#define RCC_APB2ENR_IOPCEN (1 << 4)  // Bit 4 enables GPIOC

int main(void) {
    // Step 1: Enable the clock for GPIO Port C
    RCC_APB2ENR |= RCC_APB2ENR_IOPCEN; // Set bit 4 to enable GPIOC

    // Step 2: Configure PC13 as General-Purpose Output (Push-Pull, 2 MHz)
    GPIOC_CRH &= ~(0xF << 20); // Clear configuration bits for PC13 (bits 23:20)
    GPIOC_CRH |= (0x2 << 20);  // Set MODE=10 (2 MHz output), CNF=00 (Push-Pull)

    // Step 3: Turn on the onboard LED (Active Low)
    GPIOC_ODR &= ~(1 << 13); // Set PC13 to LOW (0) to turn the LED on

    // Infinite loop to keep the LED on
    while (1) {
        // The LED remains ON indefinitely
    }

    return 0; // This line will never be reached
}



