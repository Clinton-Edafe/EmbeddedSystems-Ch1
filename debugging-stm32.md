# Debugging and Troubleshooting in STM32 Development

Debugging is an essential part of embedded system development. STM32 microcontrollers provide various debugging tools and techniques to diagnose and resolve issues efficiently.

## 1. Debugging Tools for STM32

### a. Integrated Debugger in STM32CubeIDE
- STM32CubeIDE includes a built-in debugger with breakpoints, watch variables, and memory inspection.
- It supports debugging via ST-Link, J-Link, and other compatible hardware debuggers.

### b. ST-Link Debugger
- The ST-Link hardware debugger provides real-time debugging capabilities.
- It supports serial wire debugging (SWD) and JTAG interfaces.

### c. Serial Wire Viewer (SWV)
- SWV allows real-time variable tracking and event logging.
- Useful for monitoring system behavior without halting execution.

### d. USART Debugging (printf-style Debugging)
- Sending debug messages over UART to a serial monitor.
- Example:
```c
#include <stdio.h>
#include "usart.h"

int _write(int file, char *ptr, int len) {
    HAL_UART_Transmit(&huart2, (uint8_t*)ptr, len, HAL_MAX_DELAY);
    return len;
}

printf("Debug message\n");
```

## 2. Common Debugging Techniques

### a. Breakpoints and Step Debugging
- Set breakpoints in STM32CubeIDE to pause execution at specific lines.
- Step through the code to identify logical errors.

### b. Watch Variables and Expressions
- Monitor variable values in real-time using the watch window.
- Useful for detecting unintended variable modifications.

### c. LED Debugging
- Blink an LED at key points in the code to confirm execution flow.
- Example:
```c
HAL_GPIO_TogglePin(GPIOB, GPIO_PIN_0);
HAL_Delay(500);
```

### d. Using Debug Registers
- Read system registers (e.g., status registers) to diagnose issues.
- Example: Checking the reset cause from RCC_CSR register.
```c
if (__HAL_RCC_GET_FLAG(RCC_FLAG_PINRST)) {
    printf("Reset due to pin reset\n");
}
```

## 3. Troubleshooting Common Issues

### a. Code Not Running After Flashing
- Ensure the correct boot mode is selected (BOOT0 pin settings).
- Check for incorrect linker script configurations.
- Verify power supply and connections.

### b. Unexpected System Resets
- Look for watchdog timer resets (IWDG/WDT).
- Check for stack overflow by monitoring the stack pointer.
- Use a memory analyzer to detect buffer overflows.

### c. Peripheral Communication Issues
- Verify clock configurations in STM32CubeMX.
- Use an oscilloscope or logic analyzer to inspect communication lines.
- Ensure proper pull-up resistors for I2C communication.

### d. HardFault or Exception Errors
- Enable HardFault handler for debugging:
```c
void HardFault_Handler(void) {
    printf("HardFault occurred!\n");
    while (1);
}
```
- Use Cortex-M fault status registers (CFSR, HFSR) to identify the cause.

### e. Debugging Over-the-Air (OTA) Issues
- Enable logging over a wireless interface (e.g., BLE, Wi-Fi, or LoRa).
- Use remote debugging tools like OpenOCD for connected devices.

## 4. Best Practices for Debugging STM32
- **Modular Code Design**: Keep functions small and test individual modules separately.
- **Use Assert Statements**: Add runtime assertions to catch errors early.
- **Enable Compiler Warnings**: Use `-Wall -Wextra` flags in GCC to catch potential issues.
- **Check Power and Ground Connections**: Ensure stable voltage levels for reliable operation.
- **Keep Logs and Documentation**: Maintain a log of issues and fixes for future reference.

## Conclusion
Effective debugging techniques and tools can significantly improve STM32 development efficiency. By leveraging hardware and software debugging features, developers can identify and resolve issues faster.

The next section will cover **Case Study: Implementing an Embedded System with STM32**.
