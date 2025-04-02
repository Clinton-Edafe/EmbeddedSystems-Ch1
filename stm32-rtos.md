# Real-Time Operating Systems (RTOS) with STM32

A **Real-Time Operating System (RTOS)** is essential for handling multiple tasks efficiently in embedded systems. STM32 microcontrollers support various RTOS options, including **FreeRTOS**, **RT-Thread**, and **ChibiOS**. This section focuses on **FreeRTOS**, one of the most widely used open-source RTOS solutions.

## 1. Why Use an RTOS?
Using an RTOS in STM32 projects offers several advantages over traditional bare-metal programming:
- **Task scheduling**: Efficiently manages multiple tasks.
- **Multithreading**: Enables concurrent execution of different processes.
- **Priority-based execution**: Critical tasks get executed first.
- **Synchronization and communication**: Handles shared resources through mutexes and semaphores.
- **Power efficiency**: Optimizes processor usage, improving energy efficiency.

## 2. FreeRTOS in STM32
FreeRTOS is a real-time kernel that helps in managing tasks, timing, and synchronization in STM32 microcontrollers. 

### Installing FreeRTOS in STM32CubeIDE
1. Open **STM32CubeMX** and create a new project.
2. Navigate to **Middleware** and enable **FreeRTOS**.
3. Configure the **task scheduler** and define **system tick timer**.
4. Generate code and integrate task functions.

### Task Management in FreeRTOS
In FreeRTOS, **tasks** are independent execution units that run concurrently.

#### Creating a Task
```c
#include "FreeRTOS.h"
#include "task.h"

void Task1(void *pvParameters) {
    while(1) {
        // Task execution code
        vTaskDelay(pdMS_TO_TICKS(1000)); // Delay 1 second
    }
}

int main(void) {
    HAL_Init();
    SystemClock_Config();
    xTaskCreate(Task1, "Task1", 128, NULL, 1, NULL);
    vTaskStartScheduler();
    while(1);
}
```

### Task States in FreeRTOS
Tasks can be in different states:
- **Running**: Currently executing on the CPU.
- **Ready**: Waiting for CPU allocation.
- **Blocked**: Waiting for an event (e.g., semaphore or delay).
- **Suspended**: Temporarily halted and can be resumed.

## 3. Synchronization Mechanisms in FreeRTOS
FreeRTOS provides various synchronization tools to prevent conflicts in shared resources.

### Semaphores
Semaphores help manage resource access between multiple tasks.
```c
SemaphoreHandle_t xSemaphore;

void Task1(void *pvParameters) {
    if (xSemaphoreTake(xSemaphore, portMAX_DELAY)) {
        // Critical section
        xSemaphoreGive(xSemaphore);
    }
}

int main(void) {
    xSemaphore = xSemaphoreCreateBinary();
    xTaskCreate(Task1, "Task1", 128, NULL, 1, NULL);
    vTaskStartScheduler();
}
```

### Mutexes
Mutexes ensure that only one task accesses a shared resource at a time.
```c
MutexHandle_t xMutex;

void Task1(void *pvParameters) {
    if (xSemaphoreTake(xMutex, portMAX_DELAY)) {
        // Critical section
        xSemaphoreGive(xMutex);
    }
}
```

### Message Queues
Queues enable inter-task communication by allowing data to be sent and received.
```c
QueueHandle_t xQueue;

void SenderTask(void *pvParameters) {
    int data = 100;
    xQueueSend(xQueue, &data, portMAX_DELAY);
}

void ReceiverTask(void *pvParameters) {
    int receivedData;
    xQueueReceive(xQueue, &receivedData, portMAX_DELAY);
}
```

## 4. Power Management in FreeRTOS
FreeRTOS provides **tickless idle mode** to reduce power consumption by halting the system when no tasks are running.
```c
void vApplicationIdleHook(void) {
    __WFI(); // Enter low-power mode
}
```

## Conclusion
FreeRTOS enhances the efficiency of STM32 microcontrollers by enabling multitasking, synchronization, and efficient power management. The next section will explore **Communication Protocols in STM32**.
