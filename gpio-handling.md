# GPIO Handling in STM32 Microcontrollers

General-Purpose Input/Output (GPIO) is fundamental to microcontroller applications, allowing interaction with external components like LEDs, buttons, and sensors. This section explores GPIO configuration and control using the STM32 HAL (Hardware Abstraction Layer) library.

## GPIO Modes and Configuration
STM32 GPIO pins can operate in various modes:
- **Input Mode**: Reads digital signals from external components.
- **Output Mode**: Controls external devices like LEDs and relays.
- **Alternate Function Mode**: Connects GPIOs to peripherals (e.g., UART, SPI, I2C).
- **Analog Mode**: Used for ADC (Analog-to-Digital Conversion).
- **Interrupt Mode**: Triggers an event when signal changes.

## Configuring GPIOs in STM32CubeMX
To configure GPIOs:
1. Open **STM32CubeMX** and create a new project.
2. Select the **STM32 microcontroller** you are using.
3. Navigate to the **Pinout & Configuration** tab.
4. Click on the desired pin and choose its function (e.g., **GPIO Output** for an LED).
5. Configure GPIO settings in the **Configuration** tab (Mode, Speed, Pull-up/down).
6. Generate code and open it in STM32CubeIDE.

## GPIO Initialization Code Example
Below is an example of configuring a GPIO pin as an output using the HAL library.

```c
#include "stm32f1xx_hal.h"

void SystemClock_Config(void);
void MX_GPIO_Init(void);

int main(void) {
    HAL_Init();
    SystemClock_Config();
    MX_GPIO_Init();

    while (1) {
        HAL_GPIO_TogglePin(GPIOC, GPIO_PIN_13); // Toggle LED
        HAL_Delay(500);
    }
}

void MX_GPIO_Init(void) {
    __HAL_RCC_GPIOC_CLK_ENABLE(); // Enable clock for GPIOC
    GPIO_InitTypeDef GPIO_InitStruct = {0};
    GPIO_InitStruct.Pin = GPIO_PIN_13;
    GPIO_InitStruct.Mode = GPIO_MODE_OUTPUT_PP;
    GPIO_InitStruct.Pull = GPIO_NOPULL;
    GPIO_InitStruct.Speed = GPIO_SPEED_FREQ_LOW;
    HAL_GPIO_Init(GPIOC, &GPIO_InitStruct);
}
```

## Reading Input from a Push Button
To read a button press, configure a GPIO pin in **input mode** and enable a pull-up resistor.

```c
void MX_GPIO_Init(void) {
    __HAL_RCC_GPIOA_CLK_ENABLE(); // Enable clock for GPIOA
    GPIO_InitTypeDef GPIO_InitStruct = {0};
    GPIO_InitStruct.Pin = GPIO_PIN_0;
    GPIO_InitStruct.Mode = GPIO_MODE_INPUT;
    GPIO_InitStruct.Pull = GPIO_PULLUP;
    HAL_GPIO_Init(GPIOA, &GPIO_InitStruct);
}

int main(void) {
    HAL_Init();
    SystemClock_Config();
    MX_GPIO_Init();

    while (1) {
        if (HAL_GPIO_ReadPin(GPIOA, GPIO_PIN_0) == GPIO_PIN_RESET) {
            HAL_GPIO_TogglePin(GPIOC, GPIO_PIN_13);
            HAL_Delay(500);
        }
    }
}
```

## Using GPIO Interrupts
To handle GPIO interrupts, configure the EXTI (External Interrupt) line.

1. Set the GPIO pin to **External Interrupt Mode** in STM32CubeMX.
2. Enable **NVIC (Nested Vectored Interrupt Controller)** for the corresponding EXTI line.
3. Implement the **IRQ Handler** function.

```c
void HAL_GPIO_EXTI_Callback(uint16_t GPIO_Pin) {
    if (GPIO_Pin == GPIO_PIN_0) {
        HAL_GPIO_TogglePin(GPIOC, GPIO_PIN_13);
    }
}

void EXTI0_IRQHandler(void) {
    HAL_GPIO_EXTI_IRQHandler(GPIO_PIN_0);
}
```

## Conclusion
Understanding GPIO handling is crucial for embedded development. The next section will explore timers and PWM, enabling precise time-based operations and signal generation.
