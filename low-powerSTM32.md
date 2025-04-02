# Power Management and Low-Power Modes in STM32

Power management is a critical aspect of embedded system design, especially for battery-powered applications. STM32 microcontrollers offer several power-saving modes to reduce power consumption while maintaining functionality.

## 1. Power Modes in STM32
STM32 microcontrollers support multiple power-saving modes:

### a. Run Mode
- The CPU and all peripherals are active.
- Power consumption is highest in this mode.

### b. Sleep Mode
- The CPU is stopped, but peripherals and memory remain active.
- Wake-up can be triggered by an interrupt.

### c. Low-Power Sleep Mode
- Similar to sleep mode but with reduced clock speed.
- Suitable for applications with intermittent processing needs.

### d. Stop Mode
- The CPU and most peripherals are stopped, but RAM and registers are retained.
- Wake-up can occur through an external interrupt (EXTI) or RTC alarm.

### e. Standby Mode
- The lowest power mode; only the RTC and wake-up logic remain active.
- The system state is lost (except backup registers and RTC), requiring a full restart upon wake-up.

### f. Shutdown Mode (for some STM32 series)
- Similar to standby but with even lower power consumption.
- All clocks and power domains are turned off.

## 2. Configuring Low-Power Modes in STM32

Low-power modes can be configured using the STM32CubeIDE and HAL library functions.

### a. Entering Sleep Mode
```c
HAL_SuspendTick(); // Suspend SysTick to reduce power consumption
HAL_PWR_EnterSLEEPMode(PWR_MAINREGULATOR_ON, PWR_SLEEPENTRY_WFI);
HAL_ResumeTick(); // Resume SysTick when waking up
```

### b. Entering Stop Mode
```c
HAL_PWR_EnterSTOPMode(PWR_LOWPOWERREGULATOR_ON, PWR_STOPENTRY_WFI);
SystemClock_Config(); // Reconfigure system clock after wake-up
```

### c. Entering Standby Mode
```c
HAL_PWR_EnableWakeUpPin(PWR_WAKEUP_PIN1);
HAL_PWR_EnterSTANDBYMode();
```

## 3. Wake-Up Sources
Since low-power modes disable most system functions, STM32 provides various wake-up sources:
- **External Interrupts (EXTI)**: GPIO pins configured as interrupts can wake the system.
- **Real-Time Clock (RTC) Alarm**: The RTC can trigger a wake-up event at a predefined time.
- **Watchdog Timer (IWDG/WDT)**: Ensures the system resets if it becomes unresponsive.
- **Low-Power Timer (LPTIM)**: Used for periodic wake-ups in deep sleep modes.

### Example: Wake-Up from Standby Mode Using RTC
```c
RTC_HandleTypeDef hrtc;

void Enter_Standby_With_RTC(void) {
    HAL_PWR_EnableWakeUpPin(PWR_WAKEUP_PIN1);
    __HAL_PWR_CLEAR_FLAG(PWR_FLAG_WU);
    HAL_PWR_EnterSTANDBYMode();
}

void SystemClock_Config(void) {
    // Reconfigure system clock after waking up
}
```

## 4. Power Consumption Optimization Tips
- **Use Low-Power Peripherals**: Choose peripherals that support low-power operation.
- **Reduce Clock Speed**: Lowering the system clock reduces dynamic power consumption.
- **Disable Unused Peripherals**: Turn off peripherals that are not in use.
- **Optimize Sleep Duration**: Keep the microcontroller in low-power mode as long as possible.
- **Use Efficient Code**: Optimize loops and avoid unnecessary wake-ups.

## Conclusion
Power management in STM32 microcontrollers is crucial for energy-efficient embedded designs. By selecting appropriate low-power modes and optimizing power consumption, developers can extend battery life and improve system performance.

The next section will cover **Debugging and Troubleshooting in STM32 Development**.
