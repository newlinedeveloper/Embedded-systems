### **🔥 Embedded C Interview Programs & Concepts**  

Here are **commonly asked Embedded C interview programs** with explanations. These cover **bitwise operations, ISR handling, memory management, and peripheral programming.**  

---

## **1️⃣ Endianness Check (Big-Endian or Little-Endian)**
```c
#include <stdio.h>

void checkEndianness() {
    unsigned int num = 0x1;
    char *ptr = (char*)&num;
    
    if (*ptr)
        printf("Little Endian\n");
    else
        printf("Big Endian\n");
}

int main() {
    checkEndianness();
    return 0;
}
```
✅ **Concepts:** Memory representation, byte order.  

---

## **2️⃣ Reverse Bits of a Byte**
```c
#include <stdio.h>

unsigned char reverseBits(unsigned char num) {
    unsigned char rev = 0;
    for (int i = 0; i < 8; i++) {
        rev = (rev << 1) | (num & 1);
        num >>= 1;
    }
    return rev;
}

int main() {
    unsigned char num = 0b11010010;
    printf("Reversed: 0x%X\n", reverseBits(num));
    return 0;
}
```
✅ **Concepts:** Bitwise manipulation, data representation.

---

## **3️⃣ Count Set Bits in an Integer (Hamming Weight)**
```c
#include <stdio.h>

int countSetBits(unsigned int num) {
    int count = 0;
    while (num) {
        count += num & 1;
        num >>= 1;
    }
    return count;
}

int main() {
    unsigned int num = 29;  // Binary: 11101
    printf("Set bits: %d\n", countSetBits(num));
    return 0;
}
```
✅ **Concepts:** Bitwise operations.

---

## **4️⃣ Check if a Number is Power of 2**
```c
#include <stdio.h>

int isPowerOfTwo(unsigned int num) {
    return (num && !(num & (num - 1)));
}

int main() {
    unsigned int num = 16;
    if (isPowerOfTwo(num))
        printf("%d is a power of 2\n", num);
    else
        printf("%d is not a power of 2\n", num);
    return 0;
}
```
✅ **Concepts:** Logical operations.

---

## **5️⃣ Detect Memory Alignment Issues**
```c
#include <stdio.h>

struct Test {
    char a;
    int b;
    char c;
};

int main() {
    printf("Size of struct: %lu bytes\n", sizeof(struct Test));
    return 0;
}
```
✅ **Concepts:** Memory padding, structure alignment.

---

## **6️⃣ Implement a Circular Buffer (Ring Buffer)**
```c
#include <stdio.h>
#define BUFFER_SIZE 5

typedef struct {
    char buffer[BUFFER_SIZE];
    int head, tail;
} CircularBuffer;

void buffer_write(CircularBuffer *cb, char data) {
    if ((cb->head + 1) % BUFFER_SIZE == cb->tail)
        return; // Buffer full
    cb->buffer[cb->head] = data;
    cb->head = (cb->head + 1) % BUFFER_SIZE;
}

char buffer_read(CircularBuffer *cb) {
    if (cb->head == cb->tail)
        return -1; // Buffer empty
    char data = cb->buffer[cb->tail];
    cb->tail = (cb->tail + 1) % BUFFER_SIZE;
    return data;
}

int main() {
    CircularBuffer cb = {{0}, 0, 0};
    buffer_write(&cb, 'A');
    buffer_write(&cb, 'B');
    printf("%c %c\n", buffer_read(&cb), buffer_read(&cb));
    return 0;
}
```
✅ **Concepts:** Data buffering, ring buffer.

---

## **7️⃣ Implement a Simple Software Timer**
```c
#include <stdio.h>

void delay_ms(int milliseconds) {
    volatile int count = milliseconds * 1000;
    while (count--) {}  // Waste CPU cycles
}

int main() {
    printf("Starting delay...\n");
    delay_ms(500);
    printf("Delay complete\n");
    return 0;
}
```
✅ **Concepts:** Software delay, busy-wait loops.

---

## **8️⃣ Simulate a Hardware Interrupt in C**
```c
#include <stdio.h>
#include <signal.h>

void myISR(int sig) {
    printf("Interrupt triggered! Signal: %d\n", sig);
}

int main() {
    signal(SIGINT, myISR);
    printf("Press Ctrl+C to trigger interrupt...\n");
    while (1); // Infinite loop
    return 0;
}
```
✅ **Concepts:** ISR, signal handling.

---

## **9️⃣ UART Send & Receive Simulation**
```c
#include <stdio.h>

void UART_Send(char data) {
    printf("UART TX: %c\n", data);
}

char UART_Receive() {
    return 'R'; // Simulating received data
}

int main() {
    UART_Send('A');
    printf("Received: %c\n", UART_Receive());
    return 0;
}
```
✅ **Concepts:** Serial communication.

---

## **🔟 Find the Largest and Smallest in an Array**
```c
#include <stdio.h>

void findMinMax(int arr[], int size, int *min, int *max) {
    *min = *max = arr[0];
    for (int i = 1; i < size; i++) {
        if (arr[i] > *max) *max = arr[i];
        if (arr[i] < *min) *min = arr[i];
    }
}

int main() {
    int arr[] = {12, 5, 8, 19, 3};
    int min, max;
    findMinMax(arr, 5, &min, &max);
    printf("Min: %d, Max: %d\n", min, max);
    return 0;
}
```
✅ **Concepts:** Pointer-based function calls.

---

## **1️⃣1️⃣ Swap Two Numbers Without Temp Variable**
```c
#include <stdio.h>

void swap(int *a, int *b) {
    *a = *a ^ *b;
    *b = *a ^ *b;
    *a = *a ^ *b;
}

int main() {
    int x = 5, y = 10;
    swap(&x, &y);
    printf("x: %d, y: %d\n", x, y);
    return 0;
}
```
✅ **Concepts:** XOR-based swapping.

---

## **1️⃣2️⃣ Reverse a String In-Place**
```c
#include <stdio.h>
#include <string.h>

void reverseString(char *str) {
    int i, len = strlen(str);
    for (i = 0; i < len / 2; i++) {
        char temp = str[i];
        str[i] = str[len - i - 1];
        str[len - i - 1] = temp;
    }
}

int main() {
    char str[] = "Embedded";
    reverseString(str);
    printf("Reversed: %s\n", str);
    return 0;
}
```
✅ **Concepts:** String manipulation.

---

### **🔥 Advanced Embedded C Interview Programs (SPI, I2C, CAN, RTOS)**
Here are **real-world interview programs** focusing on **peripheral programming (SPI, I2C, CAN) and RTOS-based questions.**  

---

## **1️⃣ SPI Communication - Master Transmit (Bare Metal)**
✅ **Program to Send Data from Master to Slave via SPI**
```c
#include "stm32f4xx.h"

void SPI_Init(void) {
    RCC->APB2ENR |= (1 << 12); // Enable SPI1 clock
    RCC->AHB1ENR |= (1 << 0);  // Enable GPIOA clock

    GPIOA->MODER |= (2 << 10) | (2 << 12) | (2 << 14); // PA5 (SCK), PA6 (MISO), PA7 (MOSI) AF mode
    GPIOA->AFR[0] |= (5 << 20) | (5 << 24) | (5 << 28); // AF5 for SPI1

    SPI1->CR1 = (1 << 2) | (1 << 3) | (1 << 6); // Master mode, SSM, SPE enable
}

void SPI_Send(uint8_t data) {
    while (!(SPI1->SR & (1 << 1))); // Wait for TXE
    SPI1->DR = data;
    while (!(SPI1->SR & (1 << 0))); // Wait for RXNE
    (void)SPI1->DR; // Dummy read
}

int main() {
    SPI_Init();
    SPI_Send(0x55);
    while (1);
}
```
✅ **Concepts:** SPI Master Mode, register-level programming.

---

## **2️⃣ I2C Communication - EEPROM Write**
✅ **Writing a Byte to I2C EEPROM (24C02)**
```c
#include "stm32f4xx.h"

void I2C_Init(void) {
    RCC->APB1ENR |= (1 << 21); // Enable I2C1 clock
    RCC->AHB1ENR |= (1 << 1);  // Enable GPIOB clock

    GPIOB->MODER |= (2 << 16) | (2 << 18); // PB8 (SCL), PB9 (SDA) AF mode
    GPIOB->AFR[1] |= (4 << 0) | (4 << 4);  // AF4 for I2C1

    I2C1->CR1 |= (1 << 15); // Reset I2C
    I2C1->CR1 &= ~(1 << 15);
    I2C1->CR2 |= (16 << 0); // 16MHz APB clock
    I2C1->CCR = 80;         // Standard mode
    I2C1->TRISE = 17;       // Maximum rise time
    I2C1->CR1 |= (1 << 0);  // Enable I2C
}

void I2C_Write(uint8_t addr, uint8_t data) {
    I2C1->CR1 |= (1 << 8); // Start
    while (!(I2C1->SR1 & (1 << 0))); // Wait for start condition
    I2C1->DR = 0x50 << 1; // EEPROM address (Write)
    while (!(I2C1->SR1 & (1 << 1))); // Wait for address sent
    I2C1->DR = addr; // Memory address
    while (!(I2C1->SR1 & (1 << 7))); // Wait for TXE
    I2C1->DR = data; // Data
    while (!(I2C1->SR1 & (1 << 7))); // Wait for TXE
    I2C1->CR1 |= (1 << 9); // Stop
}

int main() {
    I2C_Init();
    I2C_Write(0x10, 0x55);
    while (1);
}
```
✅ **Concepts:** I2C protocol, EEPROM interfacing.

---

## **3️⃣ CAN Bus - Sending a Message**
✅ **Basic CAN Transmission (Bare Metal)**
```c
#include "stm32f4xx.h"

void CAN_Init(void) {
    RCC->APB1ENR |= (1 << 25); // Enable CAN1 clock
    RCC->AHB1ENR |= (1 << 0);  // Enable GPIOA clock

    GPIOA->MODER |= (2 << 16) | (2 << 18); // PA8 (TX), PA9 (RX) AF mode
    GPIOA->AFR[1] |= (9 << 0) | (9 << 4);  // AF9 for CAN1

    CAN1->MCR = (1 << 0); // Initialization mode
    while (!(CAN1->MSR & (1 << 0))); // Wait for init mode
    CAN1->BTR = (4 << 16) | (1 << 20) | 5; // Bit timing
    CAN1->MCR &= ~(1 << 0); // Normal mode
}

void CAN_Send(uint32_t id, uint8_t data) {
    CAN1->sTxMailBox[0].TIR = (id << 21); // Standard ID
    CAN1->sTxMailBox[0].TDLR = data;
    CAN1->sTxMailBox[0].TIR |= (1 << 0); // Request transmission
}

int main() {
    CAN_Init();
    CAN_Send(0x123, 0xAA);
    while (1);
}
```
✅ **Concepts:** CAN controller programming, message transmission.

---

## **4️⃣ FreeRTOS - LED Blinking with Two Tasks**
✅ **Using FreeRTOS to Blink Two LEDs at Different Rates**
```c
#include "FreeRTOS.h"
#include "task.h"
#include "stm32f4xx.h"

void LED1_Task(void *pvParameters) {
    while (1) {
        GPIOA->ODR ^= (1 << 5); // Toggle LED
        vTaskDelay(pdMS_TO_TICKS(500));
    }
}

void LED2_Task(void *pvParameters) {
    while (1) {
        GPIOA->ODR ^= (1 << 6);
        vTaskDelay(pdMS_TO_TICKS(1000));
    }
}

int main(void) {
    RCC->AHB1ENR |= (1 << 0);
    GPIOA->MODER |= (1 << 10) | (1 << 12);

    xTaskCreate(LED1_Task, "LED1", 128, NULL, 1, NULL);
    xTaskCreate(LED2_Task, "LED2", 128, NULL, 1, NULL);
    vTaskStartScheduler();

    while (1);
}
```
✅ **Concepts:** Multithreading, task scheduling.

---

## **5️⃣ FreeRTOS - Binary Semaphore for Synchronization**
✅ **Using a Semaphore to Synchronize Two Tasks**
```c
#include "FreeRTOS.h"
#include "task.h"
#include "semphr.h"

SemaphoreHandle_t xSemaphore;

void Task1(void *pvParameters) {
    while (1) {
        if (xSemaphoreTake(xSemaphore, portMAX_DELAY)) {
            printf("Task 1 Running\n");
            vTaskDelay(pdMS_TO_TICKS(1000));
            xSemaphoreGive(xSemaphore);
        }
    }
}

void Task2(void *pvParameters) {
    while (1) {
        if (xSemaphoreTake(xSemaphore, portMAX_DELAY)) {
            printf("Task 2 Running\n");
            vTaskDelay(pdMS_TO_TICKS(1000));
            xSemaphoreGive(xSemaphore);
        }
    }
}

int main() {
    xSemaphore = xSemaphoreCreateBinary();
    xSemaphoreGive(xSemaphore);

    xTaskCreate(Task1, "Task1", 128, NULL, 1, NULL);
    xTaskCreate(Task2, "Task2", 128, NULL, 1, NULL);
    vTaskStartScheduler();

    while (1);
}
```
✅ **Concepts:** Synchronization, semaphores.

---

### **🔥 Advanced RTOS Topics: Message Queues, Event Groups, and Timers**  
Here are some **key RTOS concepts** with code examples to demonstrate **message queues, event groups, and timers.**  

---

## **1️⃣ FreeRTOS Message Queues - Inter-task Communication**  
✅ **Scenario:** Task 1 sends data to Task 2 using a message queue.  

```c
#include "FreeRTOS.h"
#include "task.h"
#include "queue.h"
#include "stdio.h"

QueueHandle_t xQueue;

void Sender_Task(void *pvParameters) {
    int msg = 100;
    while (1) {
        if (xQueueSend(xQueue, &msg, portMAX_DELAY) == pdPASS) {
            printf("Sent: %d\n", msg);
        }
        vTaskDelay(pdMS_TO_TICKS(1000)); // Send every 1 sec
    }
}

void Receiver_Task(void *pvParameters) {
    int received;
    while (1) {
        if (xQueueReceive(xQueue, &received, portMAX_DELAY) == pdPASS) {
            printf("Received: %d\n", received);
        }
    }
}

int main(void) {
    xQueue = xQueueCreate(5, sizeof(int)); // Queue of 5 integers
    xTaskCreate(Sender_Task, "Sender", 128, NULL, 1, NULL);
    xTaskCreate(Receiver_Task, "Receiver", 128, NULL, 1, NULL);
    vTaskStartScheduler(); // Start scheduler

    while (1);
}
```
✅ **Concepts:** Efficient inter-task communication.

---

## **2️⃣ FreeRTOS Event Groups - Synchronizing Multiple Tasks**  
✅ **Scenario:** Two tasks wait for different bits of an event to be set.  

```c
#include "FreeRTOS.h"
#include "task.h"
#include "event_groups.h"
#include "stdio.h"

EventGroupHandle_t xEventGroup;

#define BIT_TASK1 (1 << 0)
#define BIT_TASK2 (1 << 1)

void Task1(void *pvParameters) {
    while (1) {
        printf("Task 1 waiting for event...\n");
        xEventGroupWaitBits(xEventGroup, BIT_TASK1, pdTRUE, pdFALSE, portMAX_DELAY);
        printf("Task 1 triggered!\n");
    }
}

void Task2(void *pvParameters) {
    while (1) {
        printf("Task 2 waiting for event...\n");
        xEventGroupWaitBits(xEventGroup, BIT_TASK2, pdTRUE, pdFALSE, portMAX_DELAY);
        printf("Task 2 triggered!\n");
    }
}

void EventTriggerTask(void *pvParameters) {
    while (1) {
        vTaskDelay(pdMS_TO_TICKS(2000)); // Simulate an event every 2s
        printf("Triggering Task 1 and Task 2\n");
        xEventGroupSetBits(xEventGroup, BIT_TASK1 | BIT_TASK2);
    }
}

int main(void) {
    xEventGroup = xEventGroupCreate();
    xTaskCreate(Task1, "Task1", 128, NULL, 1, NULL);
    xTaskCreate(Task2, "Task2", 128, NULL, 1, NULL);
    xTaskCreate(EventTriggerTask, "Trigger", 128, NULL, 1, NULL);
    vTaskStartScheduler();

    while (1);
}
```
✅ **Concepts:** Synchronization of multiple tasks with event flags.

---

## **3️⃣ FreeRTOS Software Timer - Periodic Task Execution**  
✅ **Scenario:** A software timer executes a function every 2 seconds.  

```c
#include "FreeRTOS.h"
#include "task.h"
#include "timers.h"
#include "stdio.h"

void TimerCallback(TimerHandle_t xTimer) {
    printf("Timer Callback Triggered!\n");
}

int main(void) {
    TimerHandle_t xTimer = xTimerCreate("Timer", pdMS_TO_TICKS(2000), pdTRUE, NULL, TimerCallback);
    if (xTimer != NULL) {
        xTimerStart(xTimer, 0);
    }
    vTaskStartScheduler(); // Start RTOS
    while (1);
}
```
✅ **Concepts:** Periodic execution without wasting CPU time.

---

## **4️⃣ FreeRTOS Task Notification - Lightweight Inter-task Sync**  
✅ **Scenario:** A task is unblocked when another task sends a notification.  

```c
#include "FreeRTOS.h"
#include "task.h"
#include "stdio.h"

TaskHandle_t xTaskHandle = NULL;

void Notified_Task(void *pvParameters) {
    while (1) {
        ulTaskNotifyTake(pdTRUE, portMAX_DELAY);
        printf("Task Unblocked by Notification!\n");
    }
}

void Notifier_Task(void *pvParameters) {
    while (1) {
        vTaskDelay(pdMS_TO_TICKS(2000)); // Simulate an event every 2s
        printf("Sending Notification\n");
        xTaskNotifyGive(xTaskHandle);
    }
}

int main(void) {
    xTaskCreate(Notified_Task, "Notified", 128, NULL, 1, &xTaskHandle);
    xTaskCreate(Notifier_Task, "Notifier", 128, NULL, 1, NULL);
    vTaskStartScheduler();

    while (1);
}
```
✅ **Concepts:** Efficient task synchronization without using semaphores.

---

## **5️⃣ FreeRTOS Message Queue with ISR (Interrupt-Driven)**
✅ **Scenario:** An interrupt sends a message to a queue, processed in a task.  

```c
#include "FreeRTOS.h"
#include "task.h"
#include "queue.h"
#include "stdio.h"

QueueHandle_t xQueue;

void ISR_Handler(void) { // Simulating an ISR
    int data = 42;
    BaseType_t xHigherPriorityTaskWoken = pdFALSE;
    xQueueSendFromISR(xQueue, &data, &xHigherPriorityTaskWoken);
    portYIELD_FROM_ISR(xHigherPriorityTaskWoken);
}

void Task_Processing(void *pvParameters) {
    int received;
    while (1) {
        if (xQueueReceive(xQueue, &received, portMAX_DELAY) == pdPASS) {
            printf("ISR Data Received: %d\n", received);
        }
    }
}

int main(void) {
    xQueue = xQueueCreate(5, sizeof(int));
    xTaskCreate(Task_Processing, "Processing", 128, NULL, 1, NULL);
    vTaskStartScheduler();

    while (1);
}
```
✅ **Concepts:** Handling asynchronous events with **ISR-safe APIs**.

---


