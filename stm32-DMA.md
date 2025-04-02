# Memory Management and DMA in STM32 Microcontrollers

Efficient memory management is crucial in embedded systems to optimize performance and resource utilization. STM32 microcontrollers offer various memory types and a **Direct Memory Access (DMA)** controller to enhance data transfer efficiency.

## 1. Memory Types in STM32
STM32 microcontrollers feature multiple memory types, each serving a specific purpose:

- **Flash Memory**: Stores firmware and is non-volatile (retains data after power loss).
- **SRAM (Static RAM)**: Used for runtime variables and stack operations.
- **EEPROM (Electrically Erasable Programmable Read-Only Memory)**: Stores non-volatile data, though STM32 devices typically use Flash as EEPROM.
- **CCM (Core Coupled Memory)**: A high-speed memory area optimized for real-time operations in some STM32 models.

### Memory Organization
STM32 memory is divided into sections:

| Memory Type  | Size (Example STM32F103C8) | Purpose |
|-------------|-----------------|----------|
| Flash Memory | 64 KB | Stores program code |
| SRAM | 20 KB | Stores runtime data and stack |
| System Memory | 2 KB | Stores bootloader |

## 2. Stack and Heap Management
The **stack** and **heap** are key components of memory management in STM32:
- **Stack**: Stores function calls, local variables, and return addresses. It follows a Last In, First Out (LIFO) structure.
- **Heap**: Used for dynamically allocated memory (e.g., `malloc()` in C). Excessive heap usage can lead to fragmentation and memory leaks.

### Configuring Stack and Heap in STM32CubeIDE
To modify stack and heap sizes, adjust the `startup_stm32f1xx.s` or `Linker Script` settings.

Example (linker script snippet):
```c
_Min_Heap_Size = 0x200;  /* 512 bytes */
_Min_Stack_Size = 0x400; /* 1024 bytes */
```

## 3. Direct Memory Access (DMA) in STM32
DMA is a powerful feature that allows peripherals to transfer data directly to/from memory without CPU intervention, improving efficiency.

### Benefits of Using DMA
- **Reduces CPU workload**
- **Faster data transfers**
- **Efficient handling of high-speed peripherals like ADC, UART, and SPI**

### Configuring DMA in STM32CubeMX
1. Enable **DMA** in the **Pinout & Configuration** tab.
2. Select the desired peripheral (e.g., USART, ADC, SPI) and configure **DMA mode (Memory-to-Memory, Peripheral-to-Memory, etc.).**
3. Generate initialization code.

### Example: Using DMA for UART Reception
```c
UART_HandleTypeDef huart2;
DMA_HandleTypeDef hdma_usart2_rx;
uint8_t rxBuffer[50];

void MX_DMA_Init(void) {
    __HAL_RCC_DMA1_CLK_ENABLE();
    hdma_usart2_rx.Instance = DMA1_Channel6;
    hdma_usart2_rx.Init.Direction = DMA_PERIPH_TO_MEMORY;
    hdma_usart2_rx.Init.PeriphInc = DMA_PINC_DISABLE;
    hdma_usart2_rx.Init.MemInc = DMA_MINC_ENABLE;
    hdma_usart2_rx.Init.Mode = DMA_CIRCULAR;
    hdma_usart2_rx.Init.Priority = DMA_PRIORITY_HIGH;
    HAL_DMA_Init(&hdma_usart2_rx);
    __HAL_LINKDMA(&huart2, hdmarx, hdma_usart2_rx);
}

void Start_UART_DMA(void) {
    HAL_UART_Receive_DMA(&huart2, rxBuffer, sizeof(rxBuffer));
}
```

### Example: Using DMA for ADC Conversion
```c
ADC_HandleTypeDef hadc1;
uint32_t adcBuffer[10];

void Start_ADC_DMA(void) {
    HAL_ADC_Start_DMA(&hadc1, adcBuffer, 10);
}
```

## Conclusion
Memory management is essential for optimizing STM32 microcontroller performance. Efficiently handling **Flash, SRAM, stack, and heap** ensures system stability, while **DMA significantly enhances data transfer efficiency**. The next section will cover **Real-Time Operating Systems (RTOS) with STM32**.
