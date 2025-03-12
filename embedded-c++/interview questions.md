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

### **🚀 What Next?**
Would you like:
- **Peripheral programming questions (SPI, I2C, CAN)?**
- **RTOS-based interview programs?**
- **Embedded Linux interview concepts?**

Let me know your focus! 🚀
