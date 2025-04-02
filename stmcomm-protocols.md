# Communication Protocols in STM32 Microcontrollers

Communication is a fundamental aspect of embedded systems, allowing microcontrollers to interact with other devices such as sensors, displays, and computers. STM32 microcontrollers support various communication protocols, including **UART, SPI, and I2C**.

## 1. Universal Asynchronous Receiver Transmitter (UART)

UART is a widely used serial communication protocol that facilitates full-duplex data transmission using two lines: **TX (transmit)** and **RX (receive)**.

### Configuring UART in STM32CubeMX
1. Open **STM32CubeMX** and create a new project.
2. Enable **USART/UART** in the **Pinout & Configuration** tab.
3. Set the desired baud rate (e.g., 115200 bps), word length, stop bits, and parity.
4. Generate the code and initialize UART in STM32CubeIDE.

### Example: Sending and Receiving Data via UART
```c
#include "stm32f1xx_hal.h"

UART_HandleTypeDef huart2;

void SystemClock_Config(void);
void MX_USART2_UART_Init(void);

int main(void) {
    HAL_Init();
    SystemClock_Config();
    MX_USART2_UART_Init();

    char msg[] = "Hello, STM32 UART!\r\n";
    HAL_UART_Transmit(&huart2, (uint8_t*)msg, sizeof(msg)-1, HAL_MAX_DELAY);

    while (1) {
        uint8_t rx_data;
        if (HAL_UART_Receive(&huart2, &rx_data, 1, HAL_MAX_DELAY) == HAL_OK) {
            HAL_UART_Transmit(&huart2, &rx_data, 1, HAL_MAX_DELAY); // Echo received character
        }
    }
}

void MX_USART2_UART_Init(void) {
    huart2.Instance = USART2;
    huart2.Init.BaudRate = 115200;
    huart2.Init.WordLength = UART_WORDLENGTH_8B;
    huart2.Init.StopBits = UART_STOPBITS_1;
    huart2.Init.Parity = UART_PARITY_NONE;
    huart2.Init.Mode = UART_MODE_TX_RX;
    huart2.Init.HwFlowCtl = UART_HWCONTROL_NONE;
    HAL_UART_Init(&huart2);
}
```

## 2. Serial Peripheral Interface (SPI)

SPI is a high-speed, full-duplex communication protocol used for connecting microcontrollers to sensors, displays, and memory chips. It uses four main lines:
- **MOSI (Master Out Slave In)**
- **MISO (Master In Slave Out)**
- **SCK (Serial Clock)**
- **SS (Slave Select)**

### Configuring SPI in STM32CubeMX
1. Enable **SPI** in the **Pinout & Configuration** tab.
2. Set the SPI mode (Master/Slave), clock polarity, and phase.
3. Generate the initialization code.

### Example: SPI Communication with an External Device
```c
SPI_HandleTypeDef hspi1;

void MX_SPI1_Init(void) {
    hspi1.Instance = SPI1;
    hspi1.Init.Mode = SPI_MODE_MASTER;
    hspi1.Init.Direction = SPI_DIRECTION_2LINES;
    hspi1.Init.DataSize = SPI_DATASIZE_8BIT;
    hspi1.Init.CLKPolarity = SPI_POLARITY_LOW;
    hspi1.Init.CLKPhase = SPI_PHASE_1EDGE;
    hspi1.Init.NSS = SPI_NSS_SOFT;
    hspi1.Init.BaudRatePrescaler = SPI_BAUDRATEPRESCALER_16;
    HAL_SPI_Init(&hspi1);
}

uint8_t SPI_Transmit(uint8_t data) {
    uint8_t received;
    HAL_SPI_TransmitReceive(&hspi1, &data, &received, 1, HAL_MAX_DELAY);
    return received;
}
```

## 3. Inter-Integrated Circuit (I2C)

I2C is a multi-master, multi-slave protocol used for communication between microcontrollers and peripherals like EEPROMs, sensors, and displays. It uses two lines:
- **SDA (Serial Data Line)**
- **SCL (Serial Clock Line)**

### Configuring I2C in STM32CubeMX
1. Enable **I2C** in the **Pinout & Configuration** tab.
2. Set the I2C mode (Master/Slave) and speed.
3. Generate the initialization code.

### Example: I2C Communication with a Sensor
```c
I2C_HandleTypeDef hi2c1;

void MX_I2C1_Init(void) {
    hi2c1.Instance = I2C1;
    hi2c1.Init.ClockSpeed = 100000;
    hi2c1.Init.DutyCycle = I2C_DUTYCYCLE_2;
    hi2c1.Init.OwnAddress1 = 0;
    hi2c1.Init.AddressingMode = I2C_ADDRESSINGMODE_7BIT;
    HAL_I2C_Init(&hi2c1);
}

void I2C_Write(uint16_t devAddr, uint8_t regAddr, uint8_t data) {
    uint8_t buffer[2] = {regAddr, data};
    HAL_I2C_Master_Transmit(&hi2c1, devAddr, buffer, 2, HAL_MAX_DELAY);
}

uint8_t I2C_Read(uint16_t devAddr, uint8_t regAddr) {
    uint8_t data;
    HAL_I2C_Master_Transmit(&hi2c1, devAddr, &regAddr, 1, HAL_MAX_DELAY);
    HAL_I2C_Master_Receive(&hi2c1, devAddr, &data, 1, HAL_MAX_DELAY);
    return data;
}
```

## Conclusion
Understanding and implementing communication protocols like UART, SPI, and I2C is crucial for interfacing STM32 microcontrollers with external peripherals. The next section will cover **Interrupts and Low-Power Modes** for efficient power management.
