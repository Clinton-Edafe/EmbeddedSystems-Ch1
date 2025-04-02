# Case Study: Implementing an Embedded System with STM32

This case study explores the design and implementation of a practical embedded system using an STM32 microcontroller. The system chosen is a **Temperature Monitoring System**, which reads temperature data, processes it, and displays the result while triggering an alert if the temperature exceeds a set threshold.

## 1. System Overview
The **Temperature Monitoring System** consists of:
- **STM32F103C8T6 (Blue Pill)** microcontroller.
- **DS18B20 temperature sensor** for temperature measurement.
- **16x2 LCD display** for visual output.
- **Buzzer** to indicate high temperature alerts.
- **UART interface** for serial monitoring.

## 2. Hardware Components and Connections
### a. Components Used
- STM32F103C8T6 (Blue Pill)
- DS18B20 temperature sensor
- 16x2 LCD with I2C module
- Passive buzzer
- 4.7kΩ pull-up resistor for DS18B20
- Power supply (3.3V/5V)
- Breadboard and jumper wires

### b. Circuit Diagram
The hardware is connected as follows:
- **DS18B20**: Data pin → PA1 (with a 4.7kΩ pull-up resistor to 3.3V)
- **LCD (I2C)**: SDA → PB7, SCL → PB6
- **Buzzer**: Positive pin → PB0, Negative pin → GND
- **Power**: 3.3V and GND from STM32 to all components

## 3. Software Implementation
### a. Required Libraries
We use the **HAL Library** with STM32CubeIDE and include:
- `OneWire` and `DallasTemperature` for DS18B20 communication.
- `LiquidCrystal_I2C` for LCD display control.
- `stdio.h` for serial debugging.

### b. Code Implementation
#### i. Initialization
```c
#include "main.h"
#include "stdio.h"
#include "i2c-lcd.h"
#include "ds18b20.h"

UART_HandleTypeDef huart2;
I2C_HandleTypeDef hi2c1;

#define TEMP_THRESHOLD 30.0

void SystemClock_Config(void);
static void MX_GPIO_Init(void);
static void MX_USART2_UART_Init(void);
static void MX_I2C1_Init(void);

float temperature;
```

#### ii. Reading Temperature
```c
void read_temperature() {
    DS18B20_Start();
    HAL_Delay(750);
    temperature = DS18B20_Read();
    char buffer[16];
    sprintf(buffer, "Temp: %.2fC", temperature);
    lcd_clear();
    lcd_puts(buffer);
}
```

#### iii. Buzzer Alert System
```c
void check_temperature_alert() {
    if (temperature >= TEMP_THRESHOLD) {
        HAL_GPIO_WritePin(GPIOB, GPIO_PIN_0, GPIO_PIN_SET);
        printf("ALERT: High Temp!\n");
    } else {
        HAL_GPIO_WritePin(GPIOB, GPIO_PIN_0, GPIO_PIN_RESET);
    }
}
```

#### iv. Main Loop
```c
int main(void) {
    HAL_Init();
    SystemClock_Config();
    MX_GPIO_Init();
    MX_USART2_UART_Init();
    MX_I2C1_Init();
    lcd_init();
    DS18B20_Init();
    
    while (1) {
        read_temperature();
        check_temperature_alert();
        HAL_Delay(2000);
    }
}
```

## 4. Debugging and Optimization
- **Debugging Methods**:
  - Used `printf()` over UART for real-time debugging.
  - Implemented error handling for I2C and DS18B20 communication.
- **Power Optimization**:
  - Enabled low-power mode when the system is idle.
- **Enhancements**:
  - Added EEPROM logging to store temperature history.
  - Implemented a web interface for remote monitoring.

## 5. Conclusion
This case study demonstrates how an STM32 microcontroller can be used to develop a real-world embedded system. The **Temperature Monitoring System** showcases sensor interfacing, LCD display control, and alert mechanisms, providing a solid foundation for further enhancements like cloud integration and IoT connectivity.

