Certainly! Below is a **comprehensive guide** covering **all major concepts in Embedded C++**, including **object-oriented programming (OOP), hardware interfacing, and real-time concepts** with example codes.

---

# **📌 Embedded C++ Concepts with Code Examples**
Embedded C++ is a subset of **C++** used for microcontroller programming while keeping memory and performance constraints in mind. It extends **Embedded C** with object-oriented features, making code more reusable and modular.

---

## **1️⃣ Basics of Embedded C++**
### **🔹 Structure of an Embedded C++ Program**
Every Embedded C++ program consists of:
1. **Preprocessor Directives** (`#include`)
2. **Global Variables & Macros**
3. **Class Definitions**
4. **Main Function**
5. **Peripheral Initialization**
6. **Infinite Loop (`while(1)`)**

### **Example: Simple LED Blinking Program (OOP Approach)**
```cpp
#include <avr/io.h>
#include <util/delay.h>

// LED Control Class
class LED {
public:
    LED(uint8_t pin) { DDRB |= (1 << pin); } // Set pin as output
    void on(uint8_t pin) { PORTB |= (1 << pin); }
    void off(uint8_t pin) { PORTB &= ~(1 << pin); }
};

int main() {
    LED led(0); // Initialize LED on PB0
    while (1) {
        led.on(0);
        _delay_ms(500);
        led.off(0);
        _delay_ms(500);
    }
}
```

---

## **2️⃣ Object-Oriented Programming (OOP)**
### **🔹 Encapsulation, Inheritance, and Polymorphism**
OOP helps structure embedded applications **efficiently**.

### **Example: Motor Control using OOP**
```cpp
#include <avr/io.h>

// Base Class for Motor Control
class Motor {
protected:
    uint8_t pin;
public:
    Motor(uint8_t p) : pin(p) { DDRB |= (1 << pin); }
    virtual void start() = 0;
    virtual void stop() = 0;
};

// Derived Class: DC Motor
class DCMotor : public Motor {
public:
    DCMotor(uint8_t p) : Motor(p) {}
    void start() override { PORTB |= (1 << pin); }
    void stop() override { PORTB &= ~(1 << pin); }
};

int main() {
    DCMotor motor(0);
    motor.start();
    _delay_ms(1000);
    motor.stop();
}
```

---

## **3️⃣ GPIO (General Purpose Input/Output)**
GPIO is used for **interfacing buttons, LEDs, and sensors**.

### **Example: Button Press LED Toggle (C++ Class)**
```cpp
#include <avr/io.h>

class Button {
public:
    Button(uint8_t pin) { DDRD &= ~(1 << pin); }
    bool isPressed(uint8_t pin) { return (PIND & (1 << pin)); }
};

class LED {
public:
    LED(uint8_t pin) { DDRB |= (1 << pin); }
    void toggle(uint8_t pin) { PORTB ^= (1 << pin); }
};

int main() {
    Button button(0);
    LED led(1);
    while (1) {
        if (button.isPressed(0)) {
            led.toggle(1);
            _delay_ms(300);
        }
    }
}
```

---

## **4️⃣ Timers & Interrupts**
Timers allow precise **delays, PWM, and scheduling**.

### **Example: Timer Interrupt for Blinking LED**
```cpp
#include <avr/io.h>
#include <avr/interrupt.h>

class Timer {
public:
    Timer() {
        TCCR1B |= (1 << WGM12) | (1 << CS12); // CTC mode, Prescaler 256
        TIMSK1 |= (1 << OCIE1A);
        OCR1A = 31250; // 1 sec delay
        sei(); // Enable interrupts
    }
};

class LED {
public:
    LED(uint8_t pin) { DDRB |= (1 << pin); }
    void toggle(uint8_t pin) { PORTB ^= (1 << pin); }
};

LED led(0);
ISR(TIMER1_COMPA_vect) { led.toggle(0); }

int main() {
    Timer timer;
    while (1);
}
```

---

## **5️⃣ UART (Serial Communication)**
Used for **data logging and debugging**.

### **Example: UART Communication using C++ Classes**
```cpp
#include <avr/io.h>

class UART {
public:
    UART(uint16_t baud) {
        UBRR0H = (F_CPU / (16 * baud) - 1) >> 8;
        UBRR0L = (F_CPU / (16 * baud) - 1);
        UCSR0B = (1 << TXEN0);
    }

    void sendChar(char c) {
        while (!(UCSR0A & (1 << UDRE0)));
        UDR0 = c;
    }

    void sendString(const char* str) {
        while (*str) sendChar(*str++);
    }
};

int main() {
    UART uart(9600);
    while (1) {
        uart.sendString("Hello, Embedded C++!\n");
        _delay_ms(1000);
    }
}
```

---

## **6️⃣ PWM (Pulse Width Modulation)**
Used for **motor speed control, LED brightness**.

### **Example: PWM using C++ Classes**
```cpp
#include <avr/io.h>

class PWM {
public:
    PWM() {
        DDRB |= (1 << PB3);
        TCCR0A |= (1 << WGM01) | (1 << WGM00) | (1 << COM0A1);
        TCCR0B |= (1 << CS01);
    }

    void setDutyCycle(uint8_t duty) { OCR0A = duty; }
};

int main() {
    PWM pwm;
    while (1) {
        for (uint8_t i = 0; i < 255; i++) {
            pwm.setDutyCycle(i);
            _delay_ms(10);
        }
    }
}
```

---

## **7️⃣ I2C (Inter-Integrated Circuit)**
Used for **sensor interfacing (e.g., EEPROM, temperature sensors, OLED displays).**

### **Example: I2C Communication (Master)**
```cpp
#include <avr/io.h>

class I2C {
public:
    I2C() {
        TWBR = 0x48;
        TWSR = 0x00;
        TWCR = (1 << TWEN);
    }

    void start() {
        TWCR = (1 << TWSTA) | (1 << TWEN) | (1 << TWINT);
        while (!(TWCR & (1 << TWINT)));
    }

    void write(uint8_t data) {
        TWDR = data;
        TWCR = (1 << TWEN) | (1 << TWINT);
        while (!(TWCR & (1 << TWINT)));
    }

    void stop() { TWCR = (1 << TWSTO) | (1 << TWEN) | (1 << TWINT); }
};

int main() {
    I2C i2c;
    i2c.start();
    i2c.write(0x50);
    i2c.stop();
}
```

---

## **8️⃣ RTOS (Real-Time Operating System)**
RTOS is used for **task scheduling and multitasking**.

### **Example: FreeRTOS Task Scheduling**
```cpp
#include <Arduino_FreeRTOS.h>

void Task1(void *pvParameters) {
    while (1) { digitalWrite(13, HIGH); vTaskDelay(500); digitalWrite(13, LOW); vTaskDelay(500); }
}

void setup() {
    pinMode(13, OUTPUT);
    xTaskCreate(Task1, "LED Blink", 128, NULL, 1, NULL);
}

void loop() {}
```

---

# **📌 Summary**
✅ **Object-Oriented Programming** (Encapsulation, Inheritance, Polymorphism)  
✅ **GPIO, Timers, and Interrupts**  
✅ **UART, I2C, SPI Communication**  
✅ **PWM, ADC, and Motor Control**  
✅ **RTOS for Multitasking**  

Would you like **detailed explanations** on any specific topic? 🚀
