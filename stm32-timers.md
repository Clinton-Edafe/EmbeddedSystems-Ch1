# Timers and PWM in STM32 Microcontrollers

Timers are essential peripherals in STM32 microcontrollers used for time-based operations, periodic interrupts, and pulse-width modulation (PWM). This section explains how to configure and use timers effectively.

## Types of Timers in STM32
STM32 microcontrollers feature different types of timers:
- **Basic Timers (TIM6, TIM7)**: Used for simple time-based tasks.
- **General-Purpose Timers (TIM2-TIM5, TIM9-TIM14)**: Used for various timing applications, including PWM.
- **Advanced Timers (TIM1, TIM8)**: Feature-rich timers with advanced PWM and synchronization capabilities.

## Configuring Timers in STM32CubeMX
To configure a timer:
1. Open **STM32CubeMX** and create a new project.
2. Select the STM32 microcontroller you are using.
3. In the **Pinout & Configuration** tab, enable a timer under **Timers**.
4. Set the timer mode to **Output Compare, Input Capture, or PWM**.
5. Configure the timer parameters (Prescaler, Period, Clock Division, etc.).
6. Generate the initialization code and open it in STM32CubeIDE.

## Basic Timer Example (Using TIM2 for Delay)
This example configures TIM2 to generate an interrupt every second.

```c
#include "stm32f1xx_hal.h"

TIM_HandleTypeDef htim2;

void SystemClock_Config(void);
void MX_TIM2_Init(void);

int main(void) {
    HAL_Init();
    SystemClock_Config();
    MX_TIM2_Init();
    HAL_TIM_Base_Start_IT(&htim2);

    while (1) {
        // Main loop
    }
}

void MX_TIM2_Init(void) {
    __HAL_RCC_TIM2_CLK_ENABLE();
    htim2.Instance = TIM2;
    htim2.Init.Prescaler = 8000 - 1;
    htim2.Init.CounterMode = TIM_COUNTERMODE_UP;
    htim2.Init.Period = 1000 - 1;
    htim2.Init.ClockDivision = TIM_CLOCKDIVISION_DIV1;
    HAL_TIM_Base_Init(&htim2);
    HAL_NVIC_SetPriority(TIM2_IRQn, 0, 0);
    HAL_NVIC_EnableIRQ(TIM2_IRQn);
}

void TIM2_IRQHandler(void) {
    HAL_TIM_IRQHandler(&htim2);
}

void HAL_TIM_PeriodElapsedCallback(TIM_HandleTypeDef *htim) {
    if (htim->Instance == TIM2) {
        HAL_GPIO_TogglePin(GPIOC, GPIO_PIN_13); // Toggle LED every second
    }
}
```

## Generating PWM Signal
Pulse-width modulation (PWM) is used to control LED brightness, motor speed, and more.

1. Enable a timer channel in **PWM Mode** using STM32CubeMX.
2. Configure the duty cycle and frequency.
3. Use the HAL library to start PWM output.

### Example: PWM Signal Generation with TIM3
```c
TIM_HandleTypeDef htim3;

void MX_TIM3_Init(void) {
    __HAL_RCC_TIM3_CLK_ENABLE();
    htim3.Instance = TIM3;
    htim3.Init.Prescaler = 8000 - 1;
    htim3.Init.CounterMode = TIM_COUNTERMODE_UP;
    htim3.Init.Period = 1000 - 1;
    htim3.Init.ClockDivision = TIM_CLOCKDIVISION_DIV1;
    HAL_TIM_PWM_Init(&htim3);

    TIM_OC_InitTypeDef sConfigOC = {0};
    sConfigOC.OCMode = TIM_OCMODE_PWM1;
    sConfigOC.Pulse = 500;
    sConfigOC.OCPolarity = TIM_OCPOLARITY_HIGH;
    sConfigOC.OCFastMode = TIM_OCFAST_DISABLE;
    HAL_TIM_PWM_ConfigChannel(&htim3, &sConfigOC, TIM_CHANNEL_1);
}

int main(void) {
    HAL_Init();
    SystemClock_Config();
    MX_TIM3_Init();
    HAL_TIM_PWM_Start(&htim3, TIM_CHANNEL_1);
    while (1) {}
}
```

## Conclusion
Timers and PWM are powerful tools in embedded systems, enabling precise timing and control. The next section will discuss communication protocols like UART, SPI, and I2C.
