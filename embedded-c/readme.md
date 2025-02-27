Sure! Below is a comprehensive guide covering **all major concepts in Embedded C**, including **fundamentals, hardware interfacing, and communication protocols** with example codes.

---

# **📌 Embedded C Concepts with Code Examples**
Embedded C is a specialized version of C designed for microcontroller programming. It involves direct interaction with hardware components like **GPIO, timers, interrupts, UART, I2C, SPI**, and **low-power modes**.

---

## **1️⃣ Basics of Embedded C**
### **🔹 Structure of an Embedded C Program**
Every embedded C program consists of:
1. **Preprocessor Directives** (`#include`)
2. **Global Variables & Macros**
3. **Function Prototypes**
4. **Main Function**
5. **Infinite Loop (`while(1)`)**
6. **Interrupts or Peripheral Configurations**

### **Example: Simple LED Blinking Program**
```c
#include <reg51.h>  // Header file for 8051 microcontroller

sbit LED = P1^0;   // Define LED pin

void delay() {
    int i, j;
    for (i = 0; i < 1000; i++)
        for (j = 0; j < 100; j++);
}

void main() {
    while (1) {  
        LED = 1;  // Turn LED ON
        delay();
        LED = 0;  // Turn LED OFF
        delay();
    }
}
```

---

## **2️⃣ Data Types & Bit Manipulation**
Embedded C often uses fixed-width types (`uint8_t`, `uint16_t`, etc.) for memory efficiency.

### **Example: Setting and Clearing Bits**
```c
#include <stdint.h>

uint8_t data = 0x0F;  // 00001111

void setBit(uint8_t bit) {
    data |= (1 << bit);  // Set bit
}

void clearBit(uint8_t bit) {
    data &= ~(1 << bit); // Clear bit
}
```

---

## **3️⃣ GPIO (General Purpose Input/Output)**
GPIO allows microcontrollers to **control external devices** (LEDs, buttons, relays).

### **Example: Toggle LED on Button Press (AVR)**
```c
#include <avr/io.h>

int main() {
    DDRB |= (1 << PB0);  // Set PB0 as output
    DDRD &= ~(1 << PD0); // Set PD0 as input

    while (1) {
        if (PIND & (1 << PD0)) { // If button pressed
            PORTB ^= (1 << PB0); // Toggle LED
        }
    }
}
```

---

## **4️⃣ Timers & Delays**
Timers generate precise delays or trigger events at specific intervals.

### **Example: Timer Delay using 8051**
```c
#include <reg51.h>

void Timer0Delay() {
    TMOD = 0x01; // Timer0, Mode1
    TH0 = 0xFC;  // 1ms delay
    TL0 = 0x66;
    TR0 = 1; // Start Timer
    while (TF0 == 0); // Wait
    TR0 = 0; // Stop Timer
    TF0 = 0; // Clear Flag
}

void main() {
    while (1) {
        P1 ^= 0x01; // Toggle LED
        Timer0Delay();
    }
}
```

---

## **5️⃣ Interrupts Handling**
Interrupts **stop current execution** to handle urgent tasks.

### **Example: External Interrupt (8051)**
```c
#include <reg51.h>

void external_ISR() interrupt 0 { 
    P1 = ~P1; // Toggle Port 1 LEDs
}

void main() {
    IE = 0x81; // Enable External Interrupt 0
    while(1);  // Wait for interrupt
}
```

---

## **6️⃣ UART (Serial Communication)**
Used for **debugging, sending data to computers, or interfacing** with other devices.

### **Example: Sending Data via UART (8051)**
```c
#include <reg51.h>

void UART_Init() {
    TMOD = 0x20; // Timer1, Mode2
    TH1 = 0xFD;  // Baud rate 9600
    SCON = 0x50; // 8-bit UART
    TR1 = 1; // Start Timer
}

void UART_Transmit(char ch) {
    SBUF = ch; 
    while (TI == 0);
    TI = 0;
}

void main() {
    UART_Init();
    while (1) {
        UART_Transmit('A');
    }
}
```

---

## **7️⃣ ADC (Analog to Digital Conversion)**
Used to read **sensor values (temperature, light, pressure, etc.)**.

### **Example: Read ADC Value (AVR)**
```c
#include <avr/io.h>

void ADC_Init() {
    ADMUX = (1 << REFS0); // Reference voltage AVcc
    ADCSRA = (1 << ADEN) | (1 << ADSC) | (1 << ADPS2);
}

uint16_t ADC_Read(uint8_t channel) {
    ADMUX = (ADMUX & 0xF8) | channel;
    ADCSRA |= (1 << ADSC);
    while (ADCSRA & (1 << ADSC));
    return ADCW;
}

int main() {
    ADC_Init();
    while (1) {
        uint16_t result = ADC_Read(0);
    }
}
```

---

## **8️⃣ PWM (Pulse Width Modulation)**
Used for **motor speed control, LED dimming, and audio generation**.

### **Example: PWM LED Brightness Control (AVR)**
```c
#include <avr/io.h>

void PWM_Init() {
    DDRB |= (1 << PB3); // PB3 as output
    TCCR0 = (1 << WGM00) | (1 << WGM01) | (1 << COM01) | (1 << CS00);
}

void setPWM(uint8_t duty) {
    OCR0 = duty;
}

int main() {
    PWM_Init();
    while (1) {
        setPWM(128); // 50% brightness
    }
}
```

---

## **9️⃣ I2C (Inter-Integrated Circuit)**
Used for **sensor interfacing (e.g., temperature, OLED, RTC modules).**

### **Example: I2C Master Transmit (AVR)**
```c
#include <avr/io.h>

void I2C_Init() {
    TWBR = 0x48; // Clock speed
    TWSR = 0x00;
    TWCR = (1 << TWEN);
}

void I2C_Start() {
    TWCR = (1 << TWSTA) | (1 << TWEN) | (1 << TWINT);
    while (!(TWCR & (1 << TWINT)));
}

void I2C_Write(uint8_t data) {
    TWDR = data;
    TWCR = (1 << TWEN) | (1 << TWINT);
    while (!(TWCR & (1 << TWINT)));
}

void I2C_Stop() {
    TWCR = (1 << TWSTO) | (1 << TWEN) | (1 << TWINT);
}

int main() {
    I2C_Init();
    I2C_Start();
    I2C_Write(0x50);
    I2C_Stop();
}
```

---

## **🔟 SPI (Serial Peripheral Interface)**
Faster than I2C, used for **SD cards, display modules, and high-speed communication**.

### **Example: SPI Master Transmit (AVR)**
```c
#include <avr/io.h>

void SPI_MasterInit() {
    DDRB = (1 << PB3) | (1 << PB5) | (1 << PB2);
    SPCR = (1 << SPE) | (1 << MSTR) | (1 << SPR0);
}

void SPI_MasterTransmit(char data) {
    SPDR = data;
    while (!(SPSR & (1 << SPIF)));
}

int main() {
    SPI_MasterInit();
    while (1) {
        SPI_MasterTransmit(0xAA);
    }
}
```

---

# **📌 Summary**
✅ **Basic C & GPIO**  
✅ **Interrupts, Timers, Delays**  
✅ **UART, I2C, SPI Communication**  
✅ **ADC & PWM Control**  
✅ **Low-Power Sleep Modes**  

Would you like **detailed explanations** on any specific topic? 🚀
