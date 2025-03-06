Here are some C interview questions for candidates with 3+ years of experience, covering pointers, memory management, data structures, concurrency, and embedded systems.

1. Pointers & Memory Management
Q1: What will be the output of the following code?

#include <stdio.h>
int main() {
    int arr[] = {10, 20, 30, 40};
    int *ptr = arr;
    printf("%d %d\n", *ptr, *(ptr + 2));
    return 0;
}
✅ Expected Answer:

*ptr points to arr[0] → 10
*(ptr + 2) points to arr[2] → 30
Output: 10 30

Q2: What happens if you free a pointer twice?
✅ Expected Answer:

Freeing a pointer twice results in undefined behavior.
It can lead to double free errors, potentially causing a segmentation fault.

Q3: What is memory leakage, and how do you detect it?
✅ Expected Answer:

Memory leakage occurs when dynamically allocated memory (malloc, calloc, realloc) is not freed.
Use Valgrind (valgrind --leak-check=full ./a.out) to detect leaks.
2. Data Structures in C
Q4: How do you implement a linked list in C?
✅ Expected Answer:

Basic singly linked list implementation:

#include <stdio.h>
#include <stdlib.h>

struct Node {
    int data;
    struct Node* next;
};

void insert(struct Node** head, int new_data) {
    struct Node* new_node = (struct Node*)malloc(sizeof(struct Node));
    new_node->data = new_data;
    new_node->next = *head;
    *head = new_node;
}

void printList(struct Node* head) {
    while (head) {
        printf("%d -> ", head->data);
        head = head->next;
    }
    printf("NULL\n");
}

int main() {
    struct Node* head = NULL;
    insert(&head, 10);
    insert(&head, 20);
    insert(&head, 30);
    printList(head);
    return 0;
}
Q5: How do you implement a stack using an array?
✅ Expected Answer:

Basic stack using an array:

#define SIZE 10

struct Stack {
    int top;
    int arr[SIZE];
};

void push(struct Stack* stack, int value) {
    if (stack->top == SIZE - 1) return;
    stack->arr[++stack->top] = value;
}

int pop(struct Stack* stack) {
    if (stack->top == -1) return -1;
    return stack->arr[stack->top--];
}


3. Concurrency & Multithreading

Q6: What is a race condition? How do you prevent it?
✅ Expected Answer:

A race condition occurs when multiple threads access and modify shared data concurrently, leading to unpredictable behavior.
Prevention Methods:
Mutex locks (pthread_mutex_t)
Atomic operations (stdatomic.h)
Semaphore (sem_init, sem_wait, sem_post)
Q7: How do you create threads in C using pthreads?
✅ Expected Answer:

#include <pthread.h>
#include <stdio.h>

void* threadFunc(void* arg) {
    printf("Thread %ld is running\n", (long)arg);
    return NULL;
}

int main() {
    pthread_t thread;
    pthread_create(&thread, NULL, threadFunc, (void*)1);
    pthread_join(thread, NULL);
    return 0;
}
✅ Key Points:

Use pthread_create() to create a thread.
Use pthread_join() to wait for thread completion.


4. Embedded C Questions
Q8: What is the difference between volatile and const in Embedded C?
✅ Expected Answer:

volatile → Prevents the compiler from optimizing variables that can change unexpectedly (e.g., registers, hardware interrupts).
const → Declares a read-only variable.
Example:
volatile int sensor_value;  // Value changes via external hardware
const int max_speed = 100;  // Read-only value


Q9: How do you handle an Interrupt Service Routine (ISR) in C?
✅ Expected Answer:

Use an ISR function to handle interrupts.
Example for AVR microcontrollers:

#include <avr/interrupt.h>

ISR(TIMER1_OVF_vect) {
    // Timer1 overflow interrupt handler
}
Key Points:
ISR should be short and efficient.
Avoid blocking calls inside ISR (e.g., printf, delay).

Q10: How does DMA improve performance in Embedded C?
✅ Expected Answer:

DMA (Direct Memory Access) allows data transfer between memory and peripherals without CPU intervention.
Example (STM32 DMA for UART):

HAL_UART_Transmit_DMA(&huart1, buffer, length);



Certainly! Let's delve into some advanced **Embedded C interview questions** tailored for experienced engineers with over 5 years in the field. These questions encompass **real-world applications**, including **Modbus**, **LoRa**, **Ethernet**, and **USB CDC communication**, aligning with your interests.

---

## **1. Modbus Communication**

**Q1: How would you implement Modbus RTU protocol on an embedded system?**

**Expected Answer:**

- **Understanding of Modbus RTU:** A serial communication protocol commonly used in industrial applications.

- **Implementation Steps:**

  - **UART Configuration:** Set up UART with appropriate baud rate, parity, and stop bits.

  - **Frame Structure:** Handle address, function code, data, and CRC fields.

  - **CRC Calculation:** Implement CRC-16 for error checking.

  - **State Machine:** Design a state machine to manage request and response cycles.

---

## **2. LoRa Communication**

**Q2: Describe the process of setting up a LoRaWAN node for an IoT application.**

**Expected Answer:**

- **Hardware Selection:** Choose a suitable LoRa transceiver (e.g., SX1276) and microcontroller.

- **LoRaWAN Stack Integration:** Integrate a LoRaWAN stack (e.g., LMIC) into the firmware.

- **Configuration Parameters:** Set DevEUI, AppEUI, AppKey for OTAA or DevAddr, NwkSKey, AppSKey for ABP.

- **Network Join Procedure:** Implement OTAA or ABP to join the LoRaWAN network.

- **Data Transmission:** Use confirmed or unconfirmed messages to send sensor data.

---

## **3. Ethernet Communication**

**Q3: How do you implement a TCP/IP stack on a microcontroller for Ethernet communication?**

**Expected Answer:**

- **TCP/IP Stack Selection:** Use lightweight stacks like lwIP for resource-constrained systems.

- **MAC and PHY Interface:** Configure the microcontroller's MAC and interface with the PHY layer.

- **Memory Management:** Allocate memory for buffers and manage efficiently.

- **Socket Programming:** Implement socket APIs for TCP/UDP communication.

- **Interrupt Handling:** Manage Ethernet interrupts for packet transmission and reception.

---

## **4. USB CDC Communication**

**Q4: Explain the steps to develop a USB CDC device on an embedded platform.**

**Expected Answer:**

- **USB Stack Integration:** Incorporate a USB device stack compatible with the microcontroller.

- **Descriptor Configuration:** Define device, configuration, interface, and endpoint descriptors for CDC.

- **Endpoint Setup:** Configure endpoints for control, data IN, and data OUT transfers.

- **Class-Specific Requests:** Handle CDC class-specific requests like SET_LINE_CODING.

- **Data Handling:** Implement data transmission and reception over the virtual COM port.

---

## **5. Real-Time Operating Systems (RTOS)**

**Q5: What are the challenges of integrating an RTOS into an embedded system, and how do you address them?**

**Expected Answer:**

- **Timing Constraints:** Ensure tasks meet real-time deadlines.

- **Resource Management:** Efficiently allocate CPU, memory, and peripherals.

- **Interrupt Handling:** Prioritize and manage interrupts effectively.

- **Synchronization:** Use mutexes, semaphores to prevent race conditions.

- **Debugging Complexity:** Utilize RTOS-aware debugging tools.

---

## **6. Memory Management**

**Q6: How do you handle memory fragmentation in embedded systems?**

**Expected Answer:**

- **Static Allocation:** Prefer compile-time memory allocation.

- **Memory Pools:** Use fixed-size block allocation to reduce fragmentation.

- **Defragmentation:** Implement algorithms to compact memory, if supported.

- **Heap Management:** Optimize heap usage patterns to minimize fragmentation.

---

## **7. Power Management**

**Q7: Describe strategies to reduce power consumption in embedded systems.**

**Expected Answer:**

- **Low-Power Modes:** Utilize sleep or standby modes of the microcontroller.

- **Peripheral Management:** Disable unused peripherals to save power.

- **Dynamic Voltage Scaling:** Adjust the processor voltage and frequency according to workload.

- **Efficient Coding:** Optimize algorithms to run faster and enter low-power states sooner.

---

## **8. Debugging and Testing**

**Q8: What techniques do you use for debugging embedded systems?**

**Expected Answer:**

- **In-Circuit Debugging (ICD):** Use tools like JTAG or SWD for real-time debugging.

- **Serial Debugging:** Implement UART logging for monitoring system behavior.

- **Logic Analyzers:** Capture and analyze digital signals.

- **Oscilloscopes:** Measure and visualize analog signals.

- **Software Tools:** Use simulators and static analysis tools to identify issues.

---

## **9. Embedded Security**

**Q9: How do you ensure security in embedded systems?**

**Expected Answer:**

- **Secure Boot:** Verify firmware integrity during startup.

- **Encryption:** Use cryptographic techniques to protect data.

- **Authentication:** Implement mechanisms to verify the identity of devices or users.

- **Secure Coding Practices:** Avoid vulnerabilities like buffer overflows.

- **Regular Updates:** Provide firmware updates to patch security flaws.

---

## **10. Communication Protocols**

**Q10: Explain the differences between I2C and SPI communication protocols.**

**Expected Answer:**

- **I2C 
