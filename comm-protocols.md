# Communication Protocols in STM32

STM32 microcontrollers support a variety of communication protocols, allowing them to interface with sensors, displays, and other peripherals. This section covers some of the most commonly used communication interfaces:

## 1. UART (Universal Asynchronous Receiver-Transmitter)
UART is a simple serial communication protocol used for transmitting and receiving data between devices.

### Configuring UART in STM32CubeIDE
1. Open **STM32CubeMX** and enable **USART** in the **Peripherals** tab.
2. Set baud rate, word length, and stop bits.
3. Generate the initialization code and write UART transmit/receive functions.

### UART Code Example
```c
#include "stm32f4xx_hal.h"

UART_HandleTypeDef huart2;

void UART_Init(void) {
    huart2.Instance = USART2;
    huart2.Init.BaudRate = 9600;
    huart2.Init.WordLength = UART_WORDLENGTH_8B;
    huart2.Init.StopBits = UART_STOPBITS_1;
    huart2.Init.Parity = UART_PARITY_NONE;
    huart2.Init.Mode = UART_MODE_TX_RX;
    HAL_UART_Init(&huart2);
}

void UART_Transmit(char *data) {
    HAL_UART_Transmit(&huart2, (uint8_t *)data, strlen(data), HAL_MAX_DELAY);
}

int main(void) {
    HAL_Init();
    UART_Init();
    UART_Transmit("Hello, STM32!\r\n");
    while (1);
}
```

## 2. I2C (Inter-Integrated Circuit)
I2C is a two-wire protocol used for communication between microcontrollers and peripheral devices such as sensors and EEPROMs.

### Configuring I2C in STM32CubeIDE
1. Enable **I2C** in **STM32CubeMX** and set the clock speed.
2. Generate the initialization code and use HAL functions for communication.

### I2C Code Example (Reading a Sensor)
```c
#include "stm32f4xx_hal.h"

I2C_HandleTypeDef hi2c1;

void I2C_Init(void) {
    hi2c1.Instance = I2C1;
    hi2c1.Init.ClockSpeed = 100000;
    hi2c1.Init.DutyCycle = I2C_DUTYCYCLE_2;
    hi2c1.Init.OwnAddress1 = 0;
    hi2c1.Init.AddressingMode = I2C_ADDRESSINGMODE_7BIT;
    HAL_I2C_Init(&hi2c1);
}

void I2C_Read(uint8_t devAddr, uint8_t regAddr, uint8_t *data, uint16_t size) {
    HAL_I2C_Mem_Read(&hi2c1, devAddr, regAddr, I2C_MEMADD_SIZE_8BIT, data, size, HAL_MAX_DELAY);
}
```

## 3. SPI (Serial Peripheral Interface)
SPI is a high-speed, full-duplex communication protocol used for interfacing with displays, memory modules, and sensors.

### Configuring SPI in STM32CubeIDE
1. Enable **SPI** in **STM32CubeMX** and configure clock polarity and phase.
2. Generate initialization code and use HAL functions for communication.

### SPI Code Example (Transmitting Data)
```c
#include "stm32f4xx_hal.h"

SPI_HandleTypeDef hspi1;

void SPI_Init(void) {
    hspi1.Instance = SPI1;
    hspi1.Init.Mode = SPI_MODE_MASTER;
    hspi1.Init.Direction = SPI_DIRECTION_2LINES;
    hspi1.Init.DataSize = SPI_DATASIZE_8BIT;
    hspi1.Init.CLKPolarity = SPI_POLARITY_LOW;
    hspi1.Init.CLKPhase = SPI_PHASE_1EDGE;
    HAL_SPI_Init(&hspi1);
}

void SPI_Transmit(uint8_t *data, uint16_t size) {
    HAL_SPI_Transmit(&hspi1, data, size, HAL_MAX_DELAY);
}
```

## 4. CAN (Controller Area Network)
CAN is a robust protocol used in automotive and industrial applications for communication between multiple microcontrollers.

### Configuring CAN in STM32CubeIDE
1. Enable **CAN** in **STM32CubeMX** and configure baud rate.
2. Generate initialization code and set up message transmission/reception.

### CAN Code Example (Sending a Message)
```c
#include "stm32f4xx_hal.h"

CAN_HandleTypeDef hcan1;

void CAN_Init(void) {
    hcan1.Instance = CAN1;
    hcan1.Init.Prescaler = 16;
    hcan1.Init.Mode = CAN_MODE_NORMAL;
    hcan1.Init.SyncJumpWidth = CAN_SJW_1TQ;
    HAL_CAN_Init(&hcan1);
}

void CAN_Transmit(void) {
    CAN_TxHeaderTypeDef TxHeader;
    uint32_t TxMailbox;
    uint8_t message[8] = {0x01, 0x02, 0x03, 0x04, 0x05, 0x06, 0x07, 0x08};
    
    TxHeader.StdId = 0x321;
    TxHeader.DLC = 8;
    TxHeader.IDE = CAN_ID_STD;
    HAL_CAN_AddTxMessage(&hcan1, &TxHeader, message, &TxMailbox);
}
```

## 5. USB Communication
STM32 microcontrollers support USB communication, allowing them to function as USB devices or hosts.

### Configuring USB in STM32CubeIDE
1. Enable **USB Device (FS)** or **USB Host** in **STM32CubeMX**.
2. Select the **USB Class (CDC, HID, or MSC)**.
3. Generate the initialization code and implement communication functions.

### USB Code Example (CDC Virtual COM Port)
```c
#include "usbd_cdc_if.h"

void USB_SendData(uint8_t *data, uint16_t size) {
    CDC_Transmit_FS(data, size);
}
```

## Conclusion
STM32 microcontrollers support various communication protocols, each suited for different applications. The next section will cover **Power Management and Low-Power Modes in STM32**.
